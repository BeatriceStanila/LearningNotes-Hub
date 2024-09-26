### Document vs Relational Database
- regardless of your database (document, relational) you can write a LINQ query to pull back some data, make a change to that and call save changes to save the unit of work back to your database.

Can we treat EF Core as an abstraction over our database, in the sense that we can swap out for using different databases (SQL -> CosmosDB)?
- NO, because the goal is to leverage the power of the different systems and behave differently depending on the backend database.
- Retrieving data from multiple tables from SQL requires using INCLUDE to specifically tell EF to perform a JOIN in order to load all the data needed.
- With Document Databases this is not necessary.
- so this is why we can't swap between databases providers.

DB CONTEXT is the way EF Core connects everything up and tells us how use, how to map to the database and what the model is and everything like that.

  - EF Core with SQL -> creates a schema (the information it's spread accross multiple table)
  - EF Core with Cosmos -> creates a single document represented as a JSON document (document databases represent hierarchical structures of data)
    - related info are collocated (all info is in one document)

  
NullReferanceException: When you use a relation database and you intstruct the EF to get the 'customers', what EF will do is not get the entire 'customer' graph (including all the addresses and the phone numbers) because those are actually saved in different tableswith all the Foreing Key relationships. So, EF will only bring back the from the customer table itself and then if you take that 'customer' and attempt to drill into other tables ('address') it's not populated because it hasn't been loaded.

### Fix issue using INCLUDE (explicitly instructs the EF to load the entire graph or specifically the information neeeded).

!! Always is a good idea to examine the SQL query, even when using LINQ to express your queries.


Convergence: So basically, the competitve environment that exists now between document and relational databases
- it's pushing document databases to do better at things that relational databases are traditionally good at (the transactionality)
- likewise, the fact that the document databases are so good at dealing with docs it's pushing relational databases to have support for document capabilities.

## Mapping to JSON in relational database

'''protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  modelBuilder.Entity<Customer>().OwnsOne(e=>e.Details, b=>b.
    {
      b.OwnsMany(e=>e.Addresses);
      b.OwnsMany(e=>e.PhoneNumbers);
      b.ToJson();
    }
}'''

This will create only a single table (instead of 4), but this single table has a DETAILS column and inside is that details thing for each customer in a JSON document. 
- we are establishing some sort of HYBRID model: it's a relational database but has a JSON document embedding inside a column in that relational database, and this allows us to mix and match.
- we won't have any joins.
- this is possible because 
1. Ef Core has a rich model and so it knows what's behind this things. So, it knows what's in that document.
2. Relational databases has first class support for manipulating and looking into and doing things with JSON documents.

'''await using (var context = new CustomersContext())
  {
    var adultCustomers = await context.Set<Customer>()
      .Where(c => c.Details.Region == "Europe")
      .ToListAsync();
  }'''

- the c.Details maps to a JSON column in the database, and the Region is a property inside that Json document
- EF Core here is leveraging a SQL Server function called JSON_VALUE where you give it a Json document [Details] and a path to a property '$Region'


This is a query that didn't worked in EF7, but works fine in EF8:

'''await using (var context = new CustomersContext())
  {
    var customersWithUkAddresses = await context.Set<Customer>()
      .Where(c => c.Details.Addresses.Any(a => a.Country =="UK"))
      .ToListAsync();
  }'''

- instead looking at a scaler property inside the Region from before, we're now looking at addresses which is an array (JSON array); and because it's a collection we can now compose a link operator (ANY) on top of that thing and effectivley ask the database for all customers where's at least one address in the UK
- so, we can composing a link operator (Any) on a JSON array that's inside a document that's in a column
- SQL Query: use another Json promitive of SQL Server called OPENJSON where the path here is a path to an array of JSON not a scaler 


## Projection of parts of a collection inside a JSON document

Here we look at the addresses and compose an ANY operator on top of those addreses and we also PROJECT OUT things from outside of those addresses. So, we're composing operators both in the WHERE clause and also in the projection, in the SELECT operator.
- fetch all customers where is at least one address in the UK and then we're going to project out the first one.

'''await using (var context = new CustomersContext())
  {
    var customersWithUkAddresses = await context.Set<Customer>()
      .AsNoTracking()
      .Where(c => c.Details.Addresses.Any(a => a.Country =="UK"))
      .Select(c => new
      {
        c.Name,
        UsAddress = c.Details.Addresses.First(a => a.Country == "UK")
      })
      .ToListAsync();
  }'''

### Collection of a primitive type
public List<DateTime>? Visits {get; set;}

- DateTime = primitive type (also string, integers), it's like an inter string, it's not a complex entity type with a lots of properties inside

- with EF Core 8, we can just do a list of primitives and EF Core will implicitly map that to a JSON array in the database 

  - it's not only allows us to serialise this (which could have been done already with commas)
  - it also allows us to query them because EF Core knows what's going on

'''await using (var context = new CustomersContext())
  {
    var customersWithNotes = await context.Set<Customer>()
      .Where(c => c.Visits.Count > 1 )
      .ToListAsync();
  }'''

- ask for the customers who have at least one visit, which requires the db to go and parse what's going on inside that array

OPENJSON can be used on
1. something inside the JSON document
2. an complex array 
3. a primitive collection (which means there's only one column in the table that comes out)
4. a parameter


### This query goes even further: get all the customers where there's at leat one visit within a certain range of years

- so, the years will be extracted out of each one of those DAYTIMES in that primitive collection
- and check whether it's in that range
- 'from' and 'to' are parameters, there are not literals inside the query; they are outside the query and that means that EF will translate them into parameters

'''await using (var context = new CustomersContext())
  {
    var from = 2010;
    var to = 2019;
    var customersInRange = await context.Set<Customer>()
      .Where(c => c.Visits.Any(v => v.Year >= from && v.Year <= to))
      .ToListAsync();
  }'''


### Here there's actually no JSON the database but JSON is integral to this: Send a primitive collection from the client to the database

- ask for all customers that are part of a list
- specialDays is a list parameter
- now we are using OPENJSON over a parameter. If Core took the array from .net and basically serialised it into a string with a JSON array representation; it packed the .net arrat into a JSON array and then it sent that to SQL Server as a string parameter.
- on the SQL Server side, this string parameter is placed into an OPENJSON which unpacks it back from JSON form into a table
- once this is unpacked as dates, we can once again use it in normal way
- now the SQL it's not going to vary if we change the actual dates in the list; only the string parameter (containing the JSON array) it's going to change rather then the SQL

'''await using (var context = new CustomersContext())
  {
    var specialDays = new List<DateOnly>
    {
      new(2008, 5, 20), new(2014, 11, 21), new(2019, 1, 4)
      new(2022, 6, 11), new(2013, 2, 2), new(2023, 5, 4)
    };
    var customersWhoJoinedOnSpecialDays = await context.Set<Customer>()
      .Where(c => specialDays.Contains(c.MemberSince))
      .ToListAsync();
  }'''


### Summary
EF8 expands on previous versions with common patters for both.
EF8 Uses:
1. first-class JSON support in the database
2. With the rich EF model

To enable powerful patterns:
- primitive collections
- collections of entities
- parametr and inline collections (improved 'contains' queries)
