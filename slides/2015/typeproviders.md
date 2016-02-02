- title : F# Type Providers: The Current State
- description : Gentle overview of F# Type Providers World
- author : Sergey Tihon
- theme : night
- transition : default
- sliderNumber : true

***

## F# Type Providers
### The Current State 

---

## Sergey Tihon

[@sergey_tihon](https://twitter.com/sergey_tihon)

* Live in Minsk
* Solution Architect in [EPAM Systems](http://www.epam.com/)
* Author of [F# Weekly](https://sergeytihon.wordpress.com/category/f-weekly/)
* Ex-board member of [F# Software Foundation](http://foundation.fsharp.org/)
* Co-founder of [Minsk F# User Group](http://www.meetup.com/fsharpminsk/)
* Blog writer : [sergeytihon.wordpress.com](https://sergeytihon.wordpress.com/)
* Maintainer of [F# Community Project Incubation Space](https://github.com/fsprojects)
* Author of [Stanford NLP for .NET](http://sergey-tihon.github.io/Stanford.NLP.NET/)


***

## What are Type Providers?

(In F# since v3.0, August 2012)

---

### Classic TP intro from [Don Syme](https://twitter.com/dsyme)

<div class="fragment">

* The world is information-**rich**

</div>

<div class="fragment">

* Modern applications are information-**rich**

</div>

<div class="fragment">

* Our languages are information-**sparse**

</div>
<div class="fragment">
<font color="red" style="font-weight:bold; font-size:150%"> This is a problem! </font>
</div>

---

### CodeGen - the classic approach

| Source |    | `CodeGen`        |    | Code |
|:------:|:--:|:----------------:|:--:|:----:|
| WCF    |`=>`| svcutil.exe      |`=>`|  T1  |
| SQL DB |`=>`| edmgen.exe       |`=>`|  T2  |
| OData  |`=>`| datrasvcutil.exe |`=>`|  T3  |

---

### A Type Provider is ... 

... a component that provides types, properties, 
and methods for use in your program.

<div class="fragment">

As well as a compiler/IDE extension…

</div>

--- 

### JSON Type Provider

![JSON](images/tp/JSONTP.png)


***

## Which Type Provides exist?

---

### F# 3.0 Built-in Type Provides

* WsdlService
* EdmxFile
* ODataService
* DbmlFile (DBML file based)
* SqlDataConnection (LINQ to SQL)
* SqlEntityConnection (LINQ to Entities)

---

### [FSharp.Data](http://fsharp.github.io/FSharp.Data/)

* CSV Type Provider
* HTML Type Provider
* JSON Type Provider
* XML Type Provider
* **WorldBank Provider**
* Freebase Provider

![FSharp.Data](images/tp/FSharp.Data.png)

--- 

### WorldBank Type Provider

![WB](images/tp/wb.gif)

---

### [FSharp.Management](http://fsprojects.github.io/FSharp.Management/index.html)

* **FileSystem**
* Registry
* WMI
* **PowerShell**
* SystemTimeZonesProvider

![FSharp.Management](images/tp/FSharp.Management.png)

---

### FileSystem Type Provider

    // reference the type provider dll
    #r "FSharp.Management.dll"
    open FSharp.Management
    
    // Let the type provider do its work
    type Users = FileSystem<"C:\\Users\\">
    
    // now you have typed access to your filesystem 
    // and you can browse it via Intellisense
    Users.AllUsers.Path
    
    val it : string = "C:\Users\All Users"

---

### FileSystem TP is awesome for Unit Tests

![FileSystemTPforTests](images/tp/FileSystemTPforTests.png)

Unit tests from [Stanford.NLP.NET](http://sergey-tihon.github.io/Stanford.NLP.NET/)

---

### PowerShell Type Provider

    #r "System.Management.Automation.dll"
    #r "FSharp.Management.PowerShell.dll"
    open FSharp.Management

    type PS = PowerShellProvider< "Microsoft.PowerShell.Management" >

![PowerShell](images/tp/PowerShellProvider.png)

---

### [FSharp.Data.SqlClient](http://fsprojects.github.io/FSharp.Data.SqlClient/)

* **SqlCommandProvider** - type-safe access to full set of T-SQL language
* SqlProgrammabilityProvider - quick access to Sql Server functions, stored procedures and tables in idiomatic F# way
* SqlEnumProvider - generates enumeration types based on static lookup data from any ADO.NET complaint source

![FSharp.Data.SqlClient](images/tp/FSharp.Data.SqlClient.png)

---

### SqlCommandProvider

    open FSharp.Data
    
    [<Literal>]
    let connectionString = 
      @"Data Source=.;Initial Catalog=AdventureWorks2014;Integrated Security=True"
        
    do
      use cmd = new SqlCommandProvider<"
          SELECT TOP(@topN) FirstName, LastName, SalesYTD 
          FROM Sales.vSalesPerson
          WHERE CountryRegionName = @regionName AND SalesYTD > @salesMoreThan 
          ORDER BY SalesYTD
          " , connectionString>()
    
      cmd.Execute(topN=3L, regionName="United States", salesMoreThan=1000000M) 
      |> printfn "%A"

---

### R Provider

The F# Type Provider is a mechanism that enables smooth interoperability 
between `F#` and `R`. The Type Provider discovers R packages that are 
available in your R installation and makes them available as .NET 
namespaces underneath the parent namespace `RProvider`.

![RProvider](images/tp/RProvider.png)

---

### R Provider : Linear regression

    // Random number generator
    let rng = Random()
    let rand () = rng.NextDouble()
    
    // Generate fake X1 and X2 
    let X1s = [ for i in 0 .. 9 -> 10. * rand () ]
    let X2s = [ for i in 0 .. 9 -> 5. * rand () ]
    
    // Build Ys, following the "true" model
    let Ys = [ for i in 0 .. 9 -> 
                5. + 3. * X1s.[i] - 2. * X2s.[i] + rand () ]
    
    let dataset =
        namedParams [
            "Y", box Ys;
            "X1", box X1s;
            "X2", box X2s; ]
        |> R.data_frame
    
    let result = R.lm(formula = "Y~X1+X2", data = dataset)

---

### FSharp.Configuration

* AppSettings
* ResX
* Yaml
* Ini

![FSharp.Configuration](images/tp/FSharp.Configuration.png)

---

### Yaml Type Provider

    Mail:
        Smtp:
            Host: smtp.sample.com
            Port: 25
            User: user1
            Password: pass1
    DB:
        ConnectionString: ...
        NumberOfDeadlockRepeats: 5
        DefaultTimeout: 00:05:00

![YamlConfigProvider](images/tp/YamlConfigProvider.png)


---

### What else exist

* Apiary
* Freebase
* Excel
* Graph
* Math
* Xaml
* CRM
* DbPedia


* Twitter
* RSS
* NuGet
* DGML
* DataStore
* Hadoop/Hive/Hdfs
* MiniCvs
* COM


* FunScript
* Matlab
* IKVM
* Python
* Azure
* S3
* Neo4j
* **Swagger** (WIP)

---

### [Swagger Type Provider](http://sergey-tihon.github.io/SwaggerProvider/) (WIP)

![Swagger](images/tp/Swagger.png)

Welcome to join development

---

### Swagger Sample (WIP)

    #r "SwaggerProvider.dll"
    open SwaggerProvider
    
    [<Literal>]
    let apiSchemaUrl = "http://petstore.swagger.io/v2/swagger.json"
    let PetStore = SwaggerProvider<apiSchemaUrl> 

![SwaggerDemo](images/tp/SwaggerDemo.png)

***

## Type Provider Games!

Created by [Ross McKinlay](https://twitter.com/pezi_pink)

![Cool](images/tp/games.gif)

---

## [2048](http://www.pinksquirrellabs.com/post/2014/07/03/2048-%E2%80%93-Type-Provider-Edition.aspx)

![2048](images/tp/2048.png)

---

## [MineSweeper](http://pinksquirrellabs.com/post/2014/02/02/The-MineSweeper-Type-Provider.aspx )

![MineSweeper](images/tp/MineSweeper.png)

---

## [The North Pole Type Provider](http://pinksquirrellabs.com/post/2014/12/24/The-North-Pole-Type-Provider-Escape-from-Santa%E2%80%99s-Grotto!.aspx)

![NPTP](images/tp/NPTP.png)

Created for [F# Advent Calendar in English 2014](https://sergeytihon.wordpress.com/2014/11/24/f-advent-calendar-in-english-2014/)

---

## [Choose Your Own Adventure Type Provider](http://www.pinksquirrellabs.com/post/2013/07/29/Choose-Your-Own-Adventure-Type-Provider.aspx)

![CYOATP](images/tp/CYOATP.png)

---

## [The Don Syme type provider](http://www.pinksquirrellabs.com/post/2014/02/21/The-Don-Syme-type-provider.aspx)

![DSTP](images/tp/DSTP.png)

***

## How Type Providers work?

---

## F# Code Quotations : Basics

    let a = 42
    //val a : int = 42
    
    let b = <@ 42 @>
    //val b : Quotations.Expr<int> = Value (42)
    
    let c = <@@ 42 @@>
    //val c : Quotations.Expr = Value (42)
    
    let d = <@ 2+2 @>
    //val d : Quotations.Expr<int> =
    //  Call (None, op_Multiply, [Value (2), Value (2)])

---

## F# Code Quotations

    let f =
        <@ let rec g x =
            match x with
            | 0 -> 1
            | _ -> x * g(x-1)
        g 5@>
        
![Quotations1](images/tp/Quotations1.png)

---

## F# Code Quotations : Splicing

    let f3 n =
        <@ let rec g = function
            | 0 -> 1
            | x -> x * g(x-1)
        g %n@>
    
    let combined_quotation =
        f3 <@ 3 @>

![Quotations2](images/tp/Quotations2.png)

---

## [F# Type Provider Starter Pack](https://github.com/fsprojects/FSharp.TypeProviders.StarterPack) 

![TPSP](images/tp/TPSP.png)

https://github.com/fsprojects/FSharp.TypeProviders.StarterPack

---

### Custom Zip TP

![Zip1](images/tp/ZipTP1.png)

---

### Custom Zip TP

![Zip2](images/tp/ZipTP2.png)

---

### Zip TP Sample

![Zip3](images/tp/ZipTP3.png)

---

### Generated vs erased type providers

***

## What's new in F# 4.0

(Recently released : [20 Jul 2015](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-the-rtm-of-visual-f-4-0.aspx))

---

### Support for static parametes to 
### provided methods
    
    ProvidedType.ProvidedMethod<staticParam>
            (providedParam1,..., providedParamM) -> returnType

* Provided method can change its number of parameters with the static parameter 
given to the method. 
* The return type can likewise be changed (and can be a 
provided type which name depends on the input parameter)


    let x = ExampleType()

    let v1:int   = x.Average<1>(1) 
    let v2:float = x.Average<2>(1,2) 
    let v3:float = x.Average<3>(1,2,3) 

---

### Opportunity #1 : Regular Expressions

    let [<Literal>] phonePattern =
        @"^(?:(?<AreaCode>\d{3})-)?(?<PhoneNumber>\d{3}-\d{4})$"

Traditional usage
    
    type PhoneReg = RegexTyped<phonePattern>
    let phone = PhoneReg()
    phone.Match("800-374-2774").PhoneNumber

<div class="fragment">
    
F# 4.0 usage

    RegexTyped.Match<phonePattern>("800-374-2774").PhoneNumber

[Lincoln Atkinson](https://twitter.com/LincolnAtkinson) presented [this demo](https://gist.github.com/latkin/b1e7c066b1b9a2b72d08) in 
[Six Quick Picks from F# 4.0](https://channel9.msdn.com/events/Visual-Studio/Visual-Studio-2015-Final-Release-Event/Six-Quick-Picks-from-Visual-F-40) 

</div>

---

### Opportunity #2 : SafeString

A simple example is a provided **safe string** type which enforces that 
the number of slots in a `String.Format` format string matches the number 
of arguments passed.

![staticmeth.png](images/tp/staticmeth.png)

Sample from [Announcement of F# 4.0 preview](http://blogs.msdn.com/b/fsharpteam/archive/2014/11/12/announcing-a-preview-of-f-4-0-and-the-visual-f-tools-in-vs-2015.aspx)

---

### Opportunity #3 : SqlCommandProvider

**SqlCommandProvider** from [FSharp.Data.SqlClient](http://fsprojects.github.io/FSharp.Data.SqlClient/)

    type MyCommand1 = SqlCommandProvider<"SELECT 42", connStr> 
    type MyCommand2 = SqlCommandProvider<"SELECT 43", connStr>

<div class="fragment">
    
can be rewritten as

    type MyDb = SqlCommandProvider<connStr>

    type MyCommand1 = MyDb.GetCommand<"SELECT 42"> 
    type MyCommand2 = MyDb.GetCommand<"SELECT 43">
    
Benefits:

* Shared DB definition 
* Shared provided types

[Sample](http://fslang.uservoice.com/forums/245727-f-language/suggestions/6097685-allow-static-arguments-to-type-provider-methods-e) provided by [Dmitry Morozov](https://twitter.com/mitekm)

</div>

---

### Opportunity #4: CSV Data manipulation

A modified CSV type provider that lets you add a column.

    type MyCsvFile = FSharp.Data.CsvProvider<"mycsv.csv">
    
    let csvData = MyCsvFile.LoadSample()
    [ for row in csvData -> row.Column1, row.Column2 ]
    
    let newCsvData = csvData.WithColumn<"Column3", "int">()  
    [ for row in newCsvData -> row.Column1, row.Column2, row.Column3 ]
    
[Don Syme](https://twitter.com/dsyme)'s sample from [SampleMethStaticParamProvider](https://github.com/dsyme/SampleMethStaticParamProvider) 

---

### Opportunity #5: Search in the 
### Freebase or DbPedia provider

    type DbPedia = DbPediaProvider<"Some Parameters">
    
    let ctxt = DbPedia.GetDataContext()
    
    let princeTheMusician = ctxt.Ontology.People.Search<"Prince">. 

In the intellisense at the last point the completions for all people matching "Prince" would be shown

[Don Syme](https://twitter.com/dsyme)'s sample from [SampleMethStaticParamProvider](https://github.com/dsyme/SampleMethStaticParamProvider) 

---

### Opportunity #6

#### Any your ...

![amazing](images/tp/amazing.gif)

#### ... idea 

*** 

# Questions?

![Thanks](images/tp/Thanks.gif)


#### [@sergey_tihon](https://twitter.com/sergey_tihon) specially for [fpconf.ru](http://fpconf.ru/)

Powered by [FsReveal](http://fsprojects.github.io/FsReveal/)


***