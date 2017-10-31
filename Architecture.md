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
