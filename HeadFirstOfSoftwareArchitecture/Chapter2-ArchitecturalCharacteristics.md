_Architectural characteristics_ (the nonfunctional requirments) and _logical components_ before you can choose an _architectural style_ (the decision we make after doing a trade-off analysis). _Architectural decisions_ represent the problem domain.

## What are architectural characteristics? "ility"

An architect has to structural design software systems, for which there are two parts:

- Logical components - the domain of the application.

- Architectural characteristics - the important parts of the construction process of a software system or application, regardless of the problem domain (non-domain).


The architectural characteristics can be defined in three parts 🔺:

1. **Nondomain design considerations (explicit)**

- the _requirments_ specify what the application should do
- we need to work with the BA and others experts as early on as possible to create the requirments docuemnt in order to find out what architectural characteristics we need to support

2. **Influences some structural aspect of the design (implicit)**

 - you must consider many trade-offs when making architectural decisions, such as whether to use a monolithic versus distributed physical architecture.

3. **Crictical or important to application success**

Try to pick as few architectural characteristics as possible, rather than as many as possible. This is because architectural characteristics are:

- impossible to standardize: different organizations use different terms for the same architectural
characteristics; for example, performance and responsiveness might indicate the same behavior.

- synergistic: you often cannot choose one architectural characteristic without considering how it may affect others.

- overabundant: possible architectural characteristics are extraordinarily abundant, and new ones appear all the time; for example, a few years ago there was no such thing as on-demand elasticity via a cloud provider