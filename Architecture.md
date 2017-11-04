**Note** This is from Angular's documentation.
## Architecture Overview
HTML and Javascript or typescript (languages that compiles as js) are used by angular framework to build an application.
Angular applications consists of HTML pages with angular specific markups. These are managed by componenet classes and by also services that contain application logic. Components and services are grouped into modules.

The application is launched by bootsrapping the root module. 

#### Modules
Angular apps are modular in nature and are defined by ngModules. Every angular module needs atleast on module called the *root module* 
NgModule is a class with *@NgModule* decorator

**Note:** A *decorator* is a way of wrapping a piece of code with another. Literally, decorating one code with another. This is something called "Functional Composition or Higher order functions". This something that is available in Typescript and can be implemented in js also ( Not a standard yet). In js they are prefixed by **@**. Decorators can be used to modify js classes. Angular uses decorators to attach meta data to classes that describes how classes work.

**NgModule** A decorator function which attaches a single  metadata object which describes the properties of the module. Some of the properties are:
1. **Declarations** - View classes. Three kinds of view classes: *Components*, *Directives*, and *Pipes*.
2. **Exports** - This is a subset of declarations. These are components that need to be exported to other components and can be used in those modules.
3. **Imports** - This is the way how exported classed can be used in the current class.
4. **Providers** - Services can be created by these in a certain module. These are also available to the global collection of services and therefore are available at all parts of the app.
5. **Bootstrap** - This is the main application view called the root component and this hosts all the other views of the app. This property should only be set by the root module.

Ex of a root module

```
src/app/app.module.ts
  import { NgModule } from '@angular/core';
  import { BrowserModule } from '@angular/platform-browser';
  @NgModule({
    imports: [ BrowserModule ],
    providers: [ Logger ],
    declarations: [ AppComponent ],
    exports: [ AppComponent ],
    bootstrap: [ AppComponent ]
  })

  export class AppModule {}
```
It is not needed to export a root module because no modules will use the root component and no modules will have to import a root module.

An application is launched by bootstraping it from the root module. It is most likely done in *main.ts* file.
Ex:
```
src/main.ts
  import { plarformBrowserDynamic } from '@angular/platform-browser-dynamic';
  import { AppModule } from './app/app.module';

  platformBrowserDynamic().bootstrapModule(AppModule);
```

#### NgModules vs Javascript Modules
NgModule is a class from the decorator **@NgModule** and is a fundamental concept for angular.

Javascript modules are different and they are designed to manage a collection of objects. Each file is a module and all objects in the file belong to that module. This module can be made public by using *export* keyword and other modules can use them using *import* keyword. 

#### Angular Libraries
These are javascript modules that are available to import and can be used in the application. Modules are prefixed with a *@angular*. Javascript import statements can be used to import these public modules.

Ex:
A component decorator can be imported from a core library @angular/core as follows.
```
import { Component } from '@angular/core';
```

Apart from components angular modules can be imported as well as follows from core library:
```
import { NgModule } from '@angular/core';

import { BrowserModule } from '@angular/platform-browser'; // from platform-browser library.
```
BrowserModule is need for the NgModule module as a metadata and can be transferred as follows:

`imports: [BrowserModule ]`

The above bits of the code id the same as in the root module declaration. Now it is clear what different statements in the root module declaration signifies.

Also in this case both NgModules and Javascript modules are used together.

#### Components
The view in an app is controlled by a Component. A component consists of an application's logic inside a class. This logic supports the view meaning it determines the actions in the view. Class interacts with the view through an API of properties and methods.
Angular creates, updates, and destroys components as the application is used. There are also different lifecycle hooks like ngOnInit() etc that are triggered based on the lifecycle.

Ex: src/app/hero-list.component.ts(class)
```
export class HeroListComponent implements OnInit {
  heroes: Hero[];
  selectedHero: Hero;

  constructor(private service: HeroService) {}

  ngOnInit() {
    this.heros = this.service.getHeroes();
  }

  selectHero(hero: Hero) {
    this.selectedHero = hero;
  }
}
```

#### Templates
A component's view  is called a template. A template is HTML that renders on the browser. Apart from HTML it'll have angular specific attributes that helps in rendering data.

Ex: src/app/hero-list.component.html
```
  <h2>Hero List </h2>

  <p><i>Pick a hero from the list</i></p>
  <ul>
    <li *ngFor="let hero of heros" (click)="selectHero(hero)">
      {{hero.name}}
    </li>
  </ul>

  <hero-detail *ng-If="selectHero" [hero]="selectHero"></hero-detail>
```

Like discussed before, apart from HTML tags and elements, there are other elements like **\*ngFor**, **\*ngIf**, **{{hero.name}}**, **(click)**, **[hero]**, and **\<hero-detail\>**. These are discussed further in angular syntax.

**\<hero-detail\>** is the component and therefore is a custom element (HeroDetailComponent).
HeroDetailComponent is a child of HeroListComponent.

#### Metadata
Metadata is used to describe how a class is processed.
`class` here is just a js class. There is no relationship to a angular framework. It can be related to angularby using metadata in class. Metadata is supplied as a decorator. Here HeroListComponent becomes a component by defining `@component`.

```
@component({
  selector: 'hero-list',
  templateUrl: './hero-list.component.html',
  providers: [ HeroService ]
})

export class HeroListComponent implements OnInit {
  /* .... */
}
```
Here `@component` decorator. This will be responsible for Angular to know if it's a component or not. This decorator will require a configuration to make sure that angular knows that it is a component. Some of the component configuration options are:
`selector:` CSS selector that is used to insert an instance of hero-list component that needs to be inserted into hero-list tag found in the parnet html.
`\<hero-list\>\</hero-list\>:` When angular sees this it inserts hero-list component into the parent html.
`templateUrl:` Address to the component html.
`providers:` This is an array where dependency injection providers for services that are component are required. This is the only way an angular components to constructor requires.

The template, metadata, and component together forms the view.

Some of the other decorators that are used are `@Injectable`, `@Input`, and `@Output`.

#### Data Binding
When data changes, without a framework, data needs to be pushed into html. All the user responses and requests without a framework would be tedious to handle. In angular data binding is an important aspect and this updates the html and vice versa.

Ex:
```
<li>{{hero.name}}</li>
<hero-details [hero]="selectedHero"></hero-details>
<li (click)="selectHero(hero)"></li>
```

Here, `{{hero.name}}` *interpolation* is used here to display name in the li element.
`[hero]` *property binding* is used to pass the value of `selectedHero` from the parent `HeroListComponent` to the `hero` property of the child `HeroDetailComponent`.
`(click)` *event binding* calls the components `selectHero` method when user clicks on the hero's name.

`Two Way Data Binding` is an important aspect that combines the property binding and event binding in a single notation using `ngModel` directive. 
Ex:
```
<input [(ngModel)]="hero.name">
```
Two way binding work flow is as follows: a data property value flows to the input box from the component as with the property binding. Any changes in the input box flows back to the component. This resets the property value as with event binding.

All Data binding are processed once per javascript event cycle. This happens from the root of the application to every child components.

Data binding plays an important role in the communication between templates and it's components.
They also play an important role in communication between parent and child components.

#### Directives
Angular templates are dynamic. Angular renders them after transforming the DOM according to the instructions given by the directives.

A directive is a class that uses `@Directive` decorator. A component is a directive with template. A `@Component` decorator is a `@Directive` decorator with template-oriented features. Even though componets are technically directives with templates and therefore architecurally different and an important component of Angular architecture.

2 other types of directives called `Structural` and `Attribute` directives exits.
These most commonly appear in the element tag as attributes. These can appear as just names or as a target of an assignment or a binding.

**Structural** directives can be used to alter the DOM by adding, removing, or replacing DOM elements.
Ex:
```
<li *ngFor="let hero of heros"></li>
<app-hero-detail *ngIf="selectedHero"></app-hero-detail>
```

`*ngFor` is used to ad a `li` element for every `hero` in `heros` list
`*ngIf` includes `app-hero-detail` component only if `selectedHero` exists.

**Attribute** directives are used to alter the appearance or behaviour of an existing element. They look like regular html attributes.

`ngModel` directive which is used for two way binding is an attribute directive. This directive modifies the behavior of an existing element like `input` by displaying and modifying the value according to user response or changes in events.

```
<input [(ngModel)]="hero.name">
```

Some are other Structural directives are `ngSwithch`. And Attribute directives like `ngStyle` and `ngClass`.
We can also write custom directives. 

#### Services
A service can be a value, funciton, or even a feature that an application needs.
A service is a class that is narrow and well defined purpose that does a specific activity only.

Some of the examples of service are:
1. Logging Service
2. Data Service
3. Message Bus
4. Tax Collector
5. Application Configuration

In Angular there's no specific definition or a base class that is defined in Angular. It's just a js class. There's no need to register a service. 
Services are extensively used by Components.
Ex: Service that can be used to log to browser console.

```
src/app/logger.service.ts(class)

export class Logger {
  log(msg: any) { console.log(msg) }
  error(msg: any) { console.error(msg) }
  warn(msg: any) { console.warn(msg) }
}
```

In the following example we are using Promise to get data from backend. This service uses `Logger` service and `BackendServie` to handle server communitcation and error logging.

```
src/app/hero.service.ts(class)

export class HeroService {
  private heroes: Hero[] = [];

  constructor(
    private backend: BackendService,
    private logger: : Logger
  ) {}

  getHeros() {
    this.backend.getAll(Hero).then( (heroes: Hero[]) => {
      this.logger.log(`Fetched ${heroes.length} heroes.`);
      this.heroes.push(...heroes);
   });
  }
}
```

Components should be simple and easily testable. Some of the tasks like making server calls, validating user input, or loging to console should not be done in components but in services.

Components main job will only be to enable better user experience. It acts as a mediator between view(template) and application logic (Model in mvc or such architectures). A good component should contain properties and methods that are needed only for data binding. Everything else should be done in services (any nontrivial / significant work). These can be done in components as well but then a component becomes more complicated.

#### Dependency Injection
This is the technique that is used to supply new instance of a class to angular components or services etc. Components need services and these services can be injected using DI technique.
Ex.
```
src/app/hero-list.component.ts(constructor)

constructor( private servie: HeroService) {}
```
When angular creates a component it first asks injector for services that the component requires.

Injector maintains a container that contians service instances that was created previously. When angular asks for a service, if a service instance is not in the container yet it'll try to create on and adds to the container and returns to the component. When all the requested services are returned by the injector, a component's constructor will be called with those services as arguments. This whole process is called dependency injection.

When a injector do not have an instance of requested service, it'll create a new instance. How it creates is explained next.
When a service is created it's provider will be registered with the injector.
A *Provider* is something that creates or returns a service. A provider can be registed in a module or a component.
If a provider is registered at the root module, then the same instance is available everywhere in the app.

Ex:
```
src/app/app.module.ts(module.providers)

providers: [
  BackendService,
  HeroService,
  Logger
]
```

Providers can be registered at component levels as well using `provider` property of `@component` metadata.
Ex:
```
src/app/hero-list.component.ts(component providers)

@component({
  selector: 'app-hero-list',
  template:'',
  providers: [HeroService]
})
```
When a provider is registered at component level, a new instance of the service is created with every new instance of the component.

The above details are the main foundations of an angular application. Some of the other important companants in angular are as follows.

#### Animations
Angular's animation library can be used to animate a component and without using a lot of CSS animation techniques.

#### Change Detection
This is another feature where angular determines if a component property value is changed, when to update the screen, and how it uses zones to intercept asynchronous activity and modify the application based on the changes that happens as a result of such activity.

#### Events
Components and services can be used to raise events by publishing and subscribing to events.

#### Forms
Complex form data entry and HTML based validation and data checking

#### HTTP
Server data can be accessed, saved, and also can invoke server side actions with HTTP client.

#### Life Cycle Hooks
Some of the life cycle acitvities like `init`, 'destroy` etc can detected by monitoring the life time of a component. It'll monitor form a components creation to it's destruction by implementing lifecycle hook interfaces.

#### Pipe
Pipes (|) can be used in templated ot transform the vales to be displayed.
Ex:
```
price | currency: 'USD': true
```
This will display `$3434.33`.

#### Router
Navigate through the pages of a client application but never leave the browser. That is browser will not be re-loaded but data will be.

#### Testing
Angular testing platform can be used to run unit tests.
