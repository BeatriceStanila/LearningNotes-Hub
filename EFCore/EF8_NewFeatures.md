
### Document vs Relational Database

- Regardless of your database type (document, relational), you can write a LINQ query to pull back some data, make a change, and call `SaveChanges()` to save the unit of work back to your database.

#### Can we treat EF Core as an abstraction over our database, in the sense that we can swap out for different databases (e.g., SQL -> CosmosDB)?

**No**, because:
- The goal is to leverage the power of the different systems and behave differently depending on the backend database.
- Retrieving data from multiple tables in SQL requires using `INCLUDE` to specifically instruct EF to perform a `JOIN` in order to load all the data needed. 
- With Document Databases, this is not necessary.

This is why we can't simply swap between database providers.

#### DB Context
- The `DbContext` is how EF Core connects everything and handles mapping to the database and the models.
  - **EF Core with SQL**: Creates a schema where data is spread across multiple tables.
  - **EF Core with Cosmos**: Creates a single document represented as a JSON document (document databases represent hierarchical data structures).
    - Related info is collocated (all data is in one document).

#### NullReferenceException
When using a relational database and instructing EF to get the `customers`, EF will not load the entire customer graph (e.g., addresses, phone numbers) as these are saved in different tables with foreign key relationships. Only data from the customer table itself is fetched. Accessing related data requires explicitly loading it (e.g., using `INCLUDE`).

#### Fixing with `INCLUDE`
Use `INCLUDE` to explicitly instruct EF to load the entire graph or specific information needed.

**Tip**: It's always a good idea to examine the SQL query, even when using LINQ to express your queries.

---

### Convergence

The competitive environment between document and relational databases has led to:
- Document databases improving in transactionality, traditionally a strength of relational databases.
- Relational databases now supporting document capabilities, as document databases excel in dealing with documents.

---

### Mapping to JSON in Relational Database

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  modelBuilder.Entity<Customer>().OwnsOne(e => e.Details, b => 
  {
    b.OwnsMany(e => e.Addresses);
    b.OwnsMany(e => e.PhoneNumbers);
    b.ToJson();
  });
}
```

This creates a single table (instead of 4), but the table has a `DETAILS` column with a JSON document for each customer. This establishes a **hybrid model**: it's a relational database but embeds JSON documents within a column.

- **No joins** are required.
- **Why?** 
  1. EF Core has a rich model and understands the structure inside the JSON.
  2. Relational databases have first-class support for working with JSON documents.

**Example Query**:
```csharp
await using (var context = new CustomersContext())
{
  var adultCustomers = await context.Set<Customer>()
    .Where(c => c.Details.Region == "Europe")
    .ToListAsync();
}
```

Here, `c.Details` maps to a JSON column, and EF Core leverages the SQL Server function `JSON_VALUE` to query the `Region` property inside the JSON.

---

### New Features in EF8

```csharp
await using (var context = new CustomersContext())
{
  var customersWithUkAddresses = await context.Set<Customer>()
    .Where(c => c.Details.Addresses.Any(a => a.Country == "UK"))
    .ToListAsync();
}
```

In EF8, you can query collections (e.g., arrays) within JSON documents. The SQL query uses `OPENJSON` to access an array (e.g., addresses) inside the JSON.

#### Projection of parts of a collection inside a JSON document
You can compose both `WHERE` clauses and projections.

```csharp
await using (var context = new CustomersContext())
{
  var customersWithUkAddresses = await context.Set<Customer>()
    .AsNoTracking()
    .Where(c => c.Details.Addresses.Any(a => a.Country == "UK"))
    .Select(c => new
    {
      c.Name,
      UsAddress = c.Details.Addresses.First(a => a.Country == "UK")
    })
    .ToListAsync();
}
```

---

### Collection of a Primitive Type

```csharp
public List<DateTime>? Visits {get; set;}
```

Primitive types (e.g., `DateTime`, `string`, `int`) can be stored as JSON arrays in EF Core 8. This allows both serialization and querying.

**Example Query**:

```csharp
await using (var context = new CustomersContext())
{
  var customersWithNotes = await context.Set<Customer>()
    .Where(c => c.Visits.Count > 1)
    .ToListAsync();
}
```

EF8 leverages `OPENJSON` to query primitive collections like dates within JSON arrays.

#### More Advanced Query

Get all customers with at least one visit within a specified range of years.

```csharp
await using (var context = new CustomersContext())
{
  var from = 2010;
  var to = 2019;
  var customersInRange = await context.Set<Customer>()
    .Where(c => c.Visits.Any(v => v.Year >= from && v.Year <= to))
    .ToListAsync();
}
```

Here, `OPENJSON` extracts the year from each `DateTime` in the JSON array and filters based on the range.

---

### Sending Primitive Collections from Client to Database

You can send a primitive collection from .NET to SQL Server as a parameter.

```csharp
await using (var context = new CustomersContext())
{
  var specialDays = new List<DateOnly>
  {
    new(2008, 5, 20), new(2014, 11, 21), new(2019, 1, 4),
    new(2022, 6, 11), new(2013, 2, 2), new(2023, 5, 4)
  };
  
  var customersWhoJoinedOnSpecialDays = await context.Set<Customer>()
    .Where(c => specialDays.Contains(c.MemberSince))
    .ToListAsync();
}
```

Here, EF Core serializes the `.NET` array into a JSON array and sends it as a string parameter to SQL Server, where `OPENJSON` unpacks it into a table.

---

### Summary

EF8 expands on previous versions by using common patterns for both document and relational databases. It offers:
1. First-class JSON support in relational databases.
2. A rich EF model to enable powerful patterns, including:
   - Primitive collections.
   - Collections of entities.
   - Parameterized and inline collections (improved `Contains` queries).

