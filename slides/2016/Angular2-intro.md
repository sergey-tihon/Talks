- title : Angular2 with TypeScript & RxJs in VS Code
- author : Sergey Tihon
- theme : night
- transition : default
- sliderNumber : true

***

## Angular2 
### TypeScript & RxJS & VS Code
#### by [@sergey-tihon](https://twitter.com/sergey_tihon)

***

## Prerequisites

---

![node](images/ang2/nodejs-new-white-pantone.png)

Event-driven I/O server-side JavaScript environment
[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

---

![npm](images/ang2/npm-logo.svg)

Package manager

`npm install npm -g`

***

## TypeScript

_TypeScript is a typed superset of JavaScript that compiles to plain JavaScript._
_Any browser. Any host. Any OS. Open Source._

[www.typescriptlang.org](http://www.typescriptlang.org/)

`npm install -g typescript`

--- 

## Conclusion = `Adopted`
Use it in all new projects instead of JavaScript.

![HappySherlock](images/ang2/HappySherlock.gif)

TypeScript comes to you with `modularity`, `correctness`, `refactoring`, `intellisense`, support of `future EcmaScripts`

---

### Types, Lambdas and Type Inference

JavaScript

    [lang=javascript]
    let hello2 = function(name) {
        return "Hello " + name;
    }

TypeScript

    [lang=typescript]
    let hello = (name:string) => "Hello, " + name;
    // string => string
    
---

### Interfaces

    [lang=typescript]
    interface SPSearchRequest {
        SourceId?: string;
        EnableSorting : boolean;
        Querytext: string;
        RefinementFilters: SPResultsWrapperObject;
        HiddenConstraints: string;
        RowLimit: number;
        SelectProperties: SPResultsWrapperObject;
        HitHighlightedProperties: SPResultsWrapperObject;
        SummaryLength: number;
        EnableQueryRules: boolean;
        StartRow?: number;
        Refiners?: string;
        SortList?: {results:SortByDirection[]} // <--
        TrimDuplicatesIncludeId?: string;
    }

---

### Generic Types

    [lang=typescript]
    class Parent<T> {
        children: T[];
        constructor() {
            this.children = [];
        }
    }
    
---

### Structural Typing (Duck typing)

    [lang=typescript]
    interface Named {
        name: string;
    }

    class Person {
        name: string;
    }

    var p: Named;
    // OK, because of structural typing
    p = new Person();

***

##[Visual Studio Code](https://code.visualstudio.com/)
####Code Editing. Redefined.

![Happy](images/ang2/Happy.gif)

---

![VSCode](images/ang2/vscode.ico)

VS Code includes support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring. 
It is also customizable, so users can change the editor's theme, keyboard shortcuts, and preferences.

---

![VSCode](images/ang2/codebasics_layout.png)

---

## IntelliSense

![intellisense](images/ang2/editingevolved_intellisense.gif)

---

## Go to Definition

![ctrlhover](images/ang2/editingevolved_ctrlhover.png)

---

## Peek Definition

![references](images/ang2/editingevolved_references.png)

--- 

## Reference information

![referenceinfo](images/ang2/editingevolved_referenceinfo.png)

---

## Rename refactoring

![rename](images/ang2/editingevolved_rename.png)

---

## Debugging

![vscode_debugging](images/ang2/vscode_debugging.png)

---

## Git version control

![vscode_git](images/ang2/vscode_git.png)

---

## Configurable

The `.vscode/settings.json` file allows to configure:

- __Editor Configuration__ - font, wrapping, tab size, line numbers, ...
- __Files Configuration__ - exclude filters, encoding, trailing whitespace
- __HTTP Configuration__ - Proxy settings
- __File Explorer Configuration__ - Working Files behavior
- __Search Configuration__ - file exclude filters
- __CSS Configuration__ - CSS linting configuration
- __JavaScript Configuration__ - Language specific settings
- __JSON Configuration__ - Schemas associated with certain JSON files
- __Less/Sass Configuration__ - Control linting for Less/Sass
- __TypeScript Configuration__ - Language specific settings

***

#Angular 2
##[5min QuickStart](https://angular.io/docs/ts/latest/quickstart.html)

[angular.io](https://angular.io/)

---

### Configure TypeScript 

`tsconfig.json`
    
    {
    "compilerOptions": {
        "target": "es5",
        "module": "system",
        "moduleResolution": "node",
        "sourceMap": true,
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "removeComments": false,
        "noImplicitAny": false
    },
    "exclude": [
        "node_modules",
        "typings/main",
        "typings/main.d.ts"
    ]
    }

---

### TypeScript Typings 

`typings.json`

    {
      "ambientDependencies": {
        "es6-shim": "github:DefinitelyTyped/DefinitelyTyped/es6-shim/
    es6-shim.d.ts#6697d6f7dadbf5773cb40ecda35a76027e0783b2"
      }
    }
    
`tsconfig.json`

    {
      "scripts": {
        "postinstall": "npm run typings install"
      }
    }
    
---

### Dependencies `package.json`

    {
    ...
    "dependencies": {
        "angular2": "2.0.0-beta.7", // Released 23.02.2016
        "systemjs": "0.19.22",
        "es6-promise": "^3.0.2",
        "es6-shim": "^0.33.3",
        "reflect-metadata": "0.1.2",
        "rxjs": "5.0.0-beta.2",
        "zone.js": "0.5.15"
    },
    "devDependencies": {
        "concurrently": "^2.0.0",
        "lite-server": "^2.1.0",
        "typescript": "^1.7.5",
        "typings":"^0.6.8"
    }
    }

`npm install`

---

### Scripts `package.json`

    {
    "name": "angular2-quickstart",
    "version": "1.0.0",
    "scripts": {
        "postinstall": "npm run typings install",
        "tsc": "tsc",
        "tsc:w": "tsc -w",
        "lite": "lite-server",
        "start": "concurrent \"npm run tsc:w\" \"npm run lite\" ",
        "typings" : "typings"
    },
    "license": "ISC",
    ...
    }
    
`npm start`

--- 

## First component 
###`app/app.component.ts`

    [lang=typescript]
    import {Component} from 'angular2/core';

    @Component({
        selector: 'my-app',
        template: '<h1>My First Angular 2 App</h1>'
    })
    export class AppComponent { }

---

## Bootstrap it 
###`app/main.ts`

    [lang=typescript]
    import {bootstrap}    from 'angular2/platform/browser'
    import {AppComponent} from './app.component'

    bootstrap(AppComponent);
    
Platform specific bootstrap Browser, Apache Cordova, NativeScript or server-side rendering.

---

### `index.html`

    [lang=html]
    <html> <head>
        <title>Angular 2 QuickStart</title>
        <!-- 1. Load libraries -->
        <!-- 2. Configure SystemJS -->
        <script>
        System.config({
            packages: {        
            app: {
                format: 'register',
                defaultExtension: 'js'
            }}
        });
        System.import('app/main')
                .then(null, console.error.bind(console));
        </script>
    </head>
    <!-- 3. Display the application -->
    <body>
        <my-app>Loading...</my-app>
    </body>
    </html>

***

#Angular 2
##Cool stuff

---

### RxJS : Reactive Extensions for JavaScript

---

### `Http.get` returns `Observable<Response>`

    [lang=typescript]
    interface QuerySuggestion {
        Query:string;
    }
    @Injectable()
    export class SPSuggestionService {
        constructor(private _http:Http) { }
        public getSuggestedQueries(queryText:string, count:number = 5) 
          : Rx.Observable<QuerySuggestion[]> 
        {
            var headers = new Headers();
            headers.append('Accept', 'application/json;odata=verbose');
            return this._http.get(
                    "https://contoso.com/_api/search/suggest"
                    + "?fprequerysuggestions=true"
                    + "&inumberofquerysuggestions="+count
                    + "&querytext='"+queryText.replace("'","''")+"'",
                    { headers: headers })
                .map(res => 
                  <QuerySuggestion[]> res.json().d.suggest.Queries.results);
        };
    }

---

    [lang=typescript]
    import * as Rx from 'rxjs/Rx';
    
    export class SearchComponent implements OnInit {
        queryText = new Control("");
        querySuggestions$: Rx.Observable<QuerySuggestion>;
        
        ngOnInit() {
            this.querySuggestions$ =
                this.queryText.valueChanges
                    .debounceTime(400)
                    .distinctUntilChanged()
                    .filter((value:string) => value.length >= MinQueryLength)
                    .switchMap((value:string) =>
                         this._suggestionService.getSuggestedQueries(value));
        }

Binding

    <li *ngFor="#item of querySuggestions$ | async">
        {{item.Query}}
    </li>

---

### [Performance](http://info.meteor.com/blog/comparing-performance-of-blaze-react-angular-meteor-and-angular-2-with-meteor)

![Ang2 Perf](images/ang2/ang2-perf.png)

---

## Immutable.js
### Controlling Change Detection

- [Change Detection in Angular 2](http://victorsavkin.com/post/110170125256/change-detection-in-angular-2)
- [Angular, Immutability and Encapsulation](http://victorsavkin.com/post/133936129316/angular-immutability-and-encapsulation)
- [Better Support for Functional Programming in Angular 2](http://victorsavkin.com/post/108837493941/better-support-for-functional-programming-in)


    [lang=typescript]
    @Component({
      selector: 'person',
      template: `{{person.name}}`,
      changeDetection: ChangeDetectionStrategy.OnPush // ⇐===
    })
    class DisplayPerson {
    
---

### Hierarchical injectors


![Arch](images/ang2/arch-overview.png)

---

### Routing components


![Routing](images/ang2/angular2-routing-diagram.png)

[Component Router In-Depth](https://auth0.com/blog/2016/01/25/angular-2-series-part-4-component-router-in-depth/)

---

### [TypeScript Extensibility](https://github.com/Microsoft/TypeScript/issues/6508) is coming

![TS Ext](images/ang2/ts-ext.gif)

---

##When release ?

![ng-conf](images/ang2/ng-conf.png)

***

###SharePoint 2013 - Integration Challenges
####Same Origin Policy & Authentication - `CORS`


[http://www.silver-it.com/node/152](http://www.silver-it.com/node/152)

---

## CORS recap

![CORS](images/ang2/cors.png)

---

## Solution 1

---

Adding the necessary HTTP response headers `Access-Control-Allow-Headers`, 
`Access-Control-Allow-Methods` and `Access-Control-Allow-Origin` at IIS level. 

    <system.webServer>
      <httpProtocol>
         <customHeaders>
             <add name="Access-Control-Allow-Origin"  value="http://localhost:3000" />
             <add name="Access-Control-Allow-Headers" 
                  value="Content-Type, Accept, X-Requested-With, X-File-Name"/>
             <add name="Access-Control-Allow-Methods"     value="GET, POST"/>
             <add name="Access-Control-Allow-Credentials" value="true"/>
         </customHeaders>

In a SharePoint context, you can add those headers for a given web app using the IIS console

---

Develop a HTTP module to work around the authentication problem regarding the preflight requests

    [lang=csharp]
    public class CORSPreflightModule : IHttpModule
    {
      private const string OPTIONSHEADER = "OPTIONS";
      private const string ORIGINHEADER = "ORIGIN";
      private const string ALLOWEDORIGIN = "http://localhost:3000";
      void IHttpModule.Dispose() { }
    
      void IHttpModule.Init(HttpApplication context) {
        context.PreSendRequestHeaders += (sender, e) => {
          var response = context.Response;
    
          if (context.Request.HttpMethod.ToUpperInvariant() 
                == OPTIONSHEADER.ToUpperInvariant() &&
              context.Request.Headers[ORIGINHEADER] == ALLOWEDORIGIN)
            { response.StatusCode = (int)HttpStatusCode.OK; }
        };
      }
    }

---

## Solution 2
### `Reverse Proxy`
### Fiddler

---
- Open Fiddler, Rules, Customize Rules
- Find the `OnBeforeResponse` function and add the following


    [lang=javascript]
    static function SetCORSResponseHeaders(oSession: Session,origin: String)  {
      oSession.oResponse.headers.Add("Access-Control-Allow-Headers",
                                     "Content-Type,Authorization,X-RequestDigest");
      oSession.oResponse.headers.Add("Access-Control-Allow-Methods",
                                     "GET,POST,PUT,DELETE,MERGE, Options");
      oSession.oResponse.headers.Add("Access-Control-Allow-Origin",origin);
      oSession.oResponse.headers.Add("Access-Control-Allow-Credentials",true);   
    }
    static function OnBeforeResponse(oSession: Session) {
      var origin:String="http://localhost:3000"; //do not include a ending / character
      //in this case, we know we're in a CORS context since the origin header was sent
      if(oSession.oRequest.headers["Origin"] != '') {
         SetCORSResponseHeaders(oSession,origin);
         //In case we receive a preflight request
         if(oSession.oRequest.headers.HTTPMethod=='OPTIONS'   
            && oSession.oRequest.headers["Origin"]==origin) {    
           oSession.oResponse.headers.HTTPResponseCode  = 200;
           oSession.oResponse.headers.HTTPResponseStatus = "200 OK"; 
         }
      }

***

## Questions?
![Thanks](images/ang2/coffee.gif)

[@sergey-tihon](https://twitter.com/sergey_tihon)
