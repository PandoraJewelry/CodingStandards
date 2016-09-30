# **C# Project Standards**

## Project/repository standards
* Do not prepend "Pandora" to class/namespace/assembly/package name unless it will be shared externally
  * Use the same standard for namespaces
* Project folder(s) should be at the top level in a repository (no need for a \src folder)
* No spaces in project names (to keep VSTS url simple)
* Segregate tests in a separate folder (\tests)
  * *you did create a test project, didn't you?* 
* Prefer NuGet references to direct project references

## Source Control
* Use [Pandora-Jewelry](https://pandora-jewelry.visualstudio.com/) VSTS (instead of PJNA)
* Prefer Git over TFVC
  * Git Flow?

# **C# Coding Standards**

## Tabs vs spaces:
* 4 (or 2?) spaces (don’t use tabs)

## Where to put the curly brace?
* Always enclose blocks in curly braces (avoids scope confusion)

Not
```
    if (<condition>)
        statement(s)
```
or
```
    if (<condition>) statement(s)
```
* Put the opening brace on a new line ([Allman style](https://en.wikipedia.org/wiki/Indent_style#Allman_style))

Use
```
    if (<condition>)
    {
        statement(s)
    }
```
instead of
```
    if (<condition>) {
        statement(s)
    }
```
Exception for auto implemented properties: 
```
    public string MyLittleProperty { get; set; }
```
* [Productivity Power Tools](https://visualstudiogallery.msdn.microsoft.com/34ebc6a2-2777-421d-8914-e29c1dfa7f5d) extension will show matching vertical lines
* [Viasfora](https://visualstudiogallery.msdn.microsoft.com/19609469-380e-4fcf-bcde-e31caeb658b2) extension will enable rainbow braces

## Use [Pascal casing](https://en.wikipedia.org/wiki/PascalCase) for
* Class names
* Method names
* Properties (do not prefix with "Get" or "Set")
* Interface names (after initial "I")
```
    IThisIsVeryUseful instead of IthisIsVeryUseful  
```
* Public member variables
* Namespaces (separate logical components with periods)

## Use [Camel casing](https://en.wikipedia.org/wiki/CamelCase) for
* Parameters
* Local variables
* Private member variables (also prefix with underscore)

[Reference](http://www.c-sharpcorner.com/UploadFile/8a67c0/C-Sharp-coding-standards-and-naming-conventions/)

## Code file conventions:
* One class per file
* Exceptions:
  * Enums

## Methods/functions:
* Goal of [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) <= 10
* [Single responsibility](#solid) - a method/function should only do one thing
* Max # of arguments to method (dependency injection?)

## Return empty string or collection from a function instead of null
* Eliminates the need for null checks
* Prevents null reference errors

Avoid using **this** in code unless it's necessary

**private** modifier is no longer needed on fields/methods

Use **nameof()** whenever possible

Use properties instead of public/protected member variables
```
    public string MyLittleProperty { get; set; }
```
instead of
```
    public string MyLittleProperty;
```

# **Application Standards**

## Minimum application guidelines:
* Security
  * Authentication (login)
    * Azure AD?
    * SSO?
    * OAuth?
  * Authorization (roles/permissions)
    * Claims based?
  * Do not check in username/password in config file (or XML transform file)
    * Set in Azure application settings (what about KeyVault?)
* URLs (DNS)
* Certificates
* Force HTTPS on all routes (except login pages)

## Data integrations:
* Prefer push to target system to pull by target system
* Use ServiceBus to reduce direct coupling
* Target system is responsible for maintaining sync process (if pull is used)

## Javascript: 
* Prefer Typescript over plain javascript
  * Easier to refactor
  * Compile time checking
  * Provides IntelliSense

## Logging:
* ???

## Architectural principles:
* [Principle of Least Surprise](https://en.wikipedia.org/wiki/Principle_of_least_astonishment)
* [K.I.S.S.](https://en.wikipedia.org/wiki/KISS_principle)
* [You Ain't Gonna Need It](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)
* [Don't Repeat Yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ([Rule of Three](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)))
* [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) (principle of least knowledge)
* SRP helps to encapsulate volatility
  * Juval Lowy on [Volatility-based Decomposition](https://www.youtube.com/watch?v=VIC7QW62-Tw)
  * Udi Dahan on [service boundaries](http://udidahan.com/2012/06/23/ui-composition-techniques-for-correct-service-boundaries/)

## <a name="solid"></a>Follow [SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) principles:
Initial | Stands for | Concept | Explanation
------- | ---------- | ------- | -----------
S | SRP | [Single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) | a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
O |	OCP | [Open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle) | “software entities … should be open for extension, but closed for modification.”
L | LSP | [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle) | “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.” See also [design by contract](https://en.wikipedia.org/wiki/Design_by_contract).
I |	ISP | [Interface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) | “many client-specific interfaces are better than one general-purpose interface.”
D |	DIP | [Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) | one should “Depend upon Abstractions. Do not depend upon concretions.”

## Packages
* Use [StructureMap](https://www.nuget.org/packages/StructureMap/) as a dependency injection container
* Use [AutoMapper](https://www.nuget.org/packages/AutoMapper/) to decouple type conversions and minimize constructor parameters (will not need a constructor parameter for every property)
* Use [Entity Framework](https://www.nuget.org/packages/EntityFramework/) as an ORM
  * When should we switch to EF Core?

# **Database Standards**

Audit columns on all database tables
