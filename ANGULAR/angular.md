# Introduction to Angular - Day 1

Today I studied the basics of Angular, its benefits, and the setup process. Here are my notes:

## 1. What is Angular and why use it?

- Angular is a very popular JavaScript framework created and maintained by **Google**. It is used to build web apps.
- **Performance:** It loads fast and updates the screen quickly. Managing code is easy (modularity), it has "dependency injection" to share services, and testing is also easy.
- **Mobile Support:** With Angular, we can build a single application that runs well on both mobile and desktop without any third-party tools.
- **Language:** We write Angular apps in TypeScript (which is the most popular choice).

## 2. ECMAScript vs TypeScript

- **ECMAScript:** This is the official standard name for JavaScript. Every year a new version is released with new features (like ES6/ES2015 which introduced classes, modules, etc.).
- **TypeScript:** It was created by Microsoft. It is a "superset" of JavaScript, meaning it includes all of JavaScript, but has some extra features on top (like **Types**, Interfaces, Classes). To run it, it is first compiled and converted back into plain JavaScript. This makes writing and maintaining code much easier.

## 3. Angular Setup & Installation

1. **Install Node.js:** Angular runs on Node.js. (To check: `node -v` and `npm -v`)
2. **Install Angular CLI:** Write in terminal: `npm install -g @angular/cli`. (To check: `ng version`)
3. **Create and Run First App:**
   - To create a new project: `ng new my-first-app`
   - To go to the folder: `cd my-first-app`
   - To run the app: `ng serve`
   - Open in browser: `http://localhost:4200`

## 4. What is Angular CLI (Command Line Interface)?

CLI is a helper tool that runs in the terminal. It automates boring and manual tasks for us like setting up the project, creating new files, and running the app, so that our focus remains only on writing the app's code.

**Most Used CLI Commands:**

- `ng new <name>` : Creates a new project.
- `ng serve` : Runs the app on the local machine and auto-reloads as soon as the code is saved.
- `ng generate component <name>` (short: `ng g c <name>`) : Creates a new component along with all its files.
- `ng build` : Packages the app for final deployment.
- `ng test` : Runs tests.

---

# Angular Components & Communication

Today, I learned about Angular Components and got a basic introduction to how they communicate. Here is a summary of what I covered:

## 1. What is a Component?

A component is essentially a reusable and independent piece of the User Interface (UI). It contains its own HTML (structure), CSS (styles), and logic (TypeScript).

## 2. Why do we use Components?

- **Reusability:** We can write code once and use it in multiple places.
- **Easy Maintenance:** Since the UI is broken down into smaller pieces, finding and fixing bugs becomes easier.
- **Consistency:** Ensures our app has a uniform look and feel.
- **Testing & Teamwork:** Smaller independent pieces are easier to test, and multiple developers can work on different components at the same time.

## 3. Structure of an Angular Component

An Angular component is made up of three main parts in the code:

1. **A Class:** Where we write our logic (TypeScript).
2. **A Template:** The HTML view of the component.
3. **A Decorator:** Specifically, the `@Component` decorator which tells Angular that the class is a component. It includes metadata like the `selector` (how we use it in HTML) and the `templateUrl`/`styleUrls`.

## 4. Working with Components

- **Creating a component:** We use the Angular CLI command `ng generate component <component-name>` to quickly set up a new component.
- **Data Binding:** We can display data from our class in the HTML template using interpolation, which looks like `{{ propertyName }}`.
- **Displaying a component:** We use the component's `selector` as an HTML tag (e.g., `<app-header></app-header>`) to place it anywhere on the page.

## 5. Component Communication (Theory)

I also learned the basic concept of how components talk to each other:

- Components often have a **Parent-Child** relationship.
- **`@Input`:** Used by a child component to receive data passed down from its parent.
- **`@Output`:** Used by a child component to send events or data back up to its parent.
  _(I will be doing hands-on practice for this tomorrow!)_

---

# Angular Modules & Component Communication (Theory)

Today, I learned about Angular Modules (NgModule) and dived deeper into the theory of Component Communication. Here are my notes:

## 1. What is an Angular Module (NgModule)?

An Angular Module (`NgModule`) is like a container or a box that groups related parts of our application together. It holds:

- **Declarations:** The components, directives, and pipes that belong to this module.
- **Imports:** Other modules whose exported classes are needed by component templates in this module.
- **Exports:** The subset of declarations that should be visible and usable in the component templates of other modules.
- **Providers:** Services that this module contributes to the global collection of services.

## 2. Why do we need Modules?

Modules exist for a few key reasons:

- **Organisation:** They help in keeping related code together (like a `UserModule` or `ProductModule`).
- **Reusability:** A module can be easily exported and reused in other parts of the application or even in different projects.
- **Separation of Concerns:** Helps in maintaining a clean architecture by dividing the app into distinct feature areas.
- **Lazy Loading:** We can load modules only when they are needed (e.g., when a user navigates to a specific route), which makes the initial loading of the app much faster.

## 3. The Shift to Standalone Components

_Important Note:_ While learning modules is essential for reading and maintaining older codebases, modern Angular uses **Standalone Components** by default. This means modules are now completely optional, and we will mostly be using standalone components for any new work.

## 4. Component Communication: @Input and @Output

I also learned how data flows between parent and child components:

- **`@Input()`:** This is used when a parent component wants to send data _down_ to a child component. We use square brackets `[ ]` in the HTML to pass this data (e.g., `[data]="parentData"`).
- **`@Output()`:** This is used when a child component wants to send an event or data _up_ to its parent. It uses an `EventEmitter` and `$event`. We use round brackets `( )` in the HTML to listen for these events (e.g., `(customEvent)="handleEvent($event)"`).
  _(I'll be doing hands-on practice for this tomorrow!)_

---

**1. What is a module, and two reasons modules exist**
A module in Angular is essentially a container that groups related components, services, and other files together. Think of it like a folder that organizes a specific feature of the app.
Two main reasons modules exist are:

- **Organisation:** It keeps related code grouped together, making the project easier to navigate and maintain.
- **Lazy Loading:** It allows us to load only the parts of the app that the user is currently accessing, speeding up the app's initial load time.

**2. Why did Angular move from modules to standalone components?**
Angular moved to standalone components to simplify the learning curve and reduce boilerplate code. With NgModules, developers had to jump between the component file and the module file to declare and configure things, which was confusing and tedious. Standalone components make things more straightforward by allowing a component to manage its own dependencies directly, making the code much easier to write, read, and maintain.

---

# Component Lifecycle Hooks

Every Angular component has a life: it is born, it updates, and it is destroyed. Angular lets you run your own code at each of these moments using lifecycle hooks. Think of hooks as "call me when this happens."

## 1. What is a lifecycle hook?

A component goes through stages: Angular creates it, shows it, updates it when data changes, and finally removes it. At each stage, Angular can **call a method on your component**, if you have written one. That method is a lifecycle hook.

**The idea in one line:** a lifecycle hook is a method Angular runs automatically at a specific moment in the component's life. You write the method, Angular decides when to call it.

Each hook has a matching interface you can add for safety, and each method starts with `ng`.

## 2. The lifecycle, in order

Here are the main hooks, in the order Angular calls them:

| Hook              | When Angular calls it                                       |
| :---------------- | :---------------------------------------------------------- |
| `ngOnInit`        | once, after the component is created and its inputs are set |
| `ngOnChanges`     | whenever an `@Input` value changes                          |
| `ngDoCheck`       | on every change detection run (used rarely)                 |
| `ngAfterViewInit` | once, after the component's view is fully ready             |
| `ngOnDestroy`     | once, just before the component is removed                  |

You do not need all of them. Most of the time you will use just two: `ngOnInit` to set things up, and `ngOnDestroy` to clean things up. Start with those.

## 3. ngOnInit: the most used hook

`ngOnInit` runs **once**, right after the component is created. This is the place to load data, set starting values, or call an API. It is the "the component is ready, do your setup now" moment.

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-hello",
  template: `<h3>{{ message }}</h3>`,
})
export class HelloComponent implements OnInit {
  message: string = "";

  ngOnInit() {
    this.message = "Component is ready!";
    console.log("ngOnInit ran");
  }
}
```

When the component appears, `ngOnInit` runs, sets the message, and the page shows **Component is ready!**. This is where your setup code belongs, not in the constructor.

**Common question:** why not use the constructor? The constructor runs before Angular has set up the component's inputs and view. `ngOnInit` runs after, so your data is ready. Rule of thumb: constructor for simple wiring, `ngOnInit` for real setup work.

## 4. ngOnChanges: react to input changes

`ngOnChanges` runs whenever a parent changes an `@Input` value. Use it when the child needs to react to new data coming in.

```typescript
import { Component, Input, OnChanges } from "@angular/core";

@Component({
  selector: "app-child",
  template: `<p>Name is: {{ name }}</p>`,
})
export class ChildComponent implements OnChanges {
  @Input() name: string = "";

  ngOnChanges() {
    console.log("Input changed, name is now:", this.name);
  }
}
```

Every time the parent sets a new `name`, `ngOnChanges` runs and you can respond to the new value.

## 5. ngOnDestroy: clean up before leaving

`ngOnDestroy` runs **once**, just before Angular removes the component. This is where you clean up: stop timers, cancel subscriptions, remove listeners. Skipping cleanup causes memory leaks.

```typescript
import { Component, OnInit, OnDestroy } from "@angular/core";

@Component({
  selector: "app-timer",
  template: `<p>Seconds: {{ count }}</p>`,
})
export class TimerComponent implements OnInit, OnDestroy {
  count: number = 0;
  timerId: any;

  ngOnInit() {
    // start a timer when the component appears
    this.timerId = setInterval(() => this.count++, 1000);
  }

  ngOnDestroy() {
    // stop the timer before the component is removed
    clearInterval(this.timerId);
    console.log("Timer cleaned up");
  }
}
```

The timer starts in `ngOnInit` and is stopped in `ngOnDestroy`. Without the cleanup, the timer would keep running even after the component is gone, wasting memory. Start in init, clean up in destroy.

## 6. A simple way to remember the order

- **Born:** constructor runs, then `ngOnChanges` (first inputs), then `ngOnInit` (setup).
- **Living:** `ngOnChanges` runs again each time an input changes.
- **View ready:** `ngAfterViewInit` runs once the template is fully rendered.
- **Dying:** `ngOnDestroy` runs just before the component is removed.

## 7. Remember this

Lifecycle hooks are methods Angular calls automatically at set moments in a component's life.

- **ngOnInit** – runs once at the start. Do your setup here (load data, set values).
- **ngOnChanges** – runs when an `@Input` changes. React to new data.
- **ngOnDestroy** – runs once at the end. Clean up here (timers, subscriptions).

**Rule of thumb:** set up in `ngOnInit`, clean up in `ngOnDestroy`. Those two cover most of your needs.
