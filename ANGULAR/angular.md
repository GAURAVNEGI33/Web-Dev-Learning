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

# Day 28: Angular Modules & Component Communication (Theory)

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

## Homework

**1. What is a module, and two reasons modules exist**
A module in Angular is essentially a container that groups related components, services, and other files together. Think of it like a folder that organizes a specific feature of the app.
Two main reasons modules exist are:

- **Organisation:** It keeps related code grouped together, making the project easier to navigate and maintain.
- **Lazy Loading:** It allows us to load only the parts of the app that the user is currently accessing, speeding up the app's initial load time.

**2. Why did Angular move from modules to standalone components?**
Angular moved to standalone components to simplify the learning curve and reduce boilerplate code. With NgModules, developers had to jump between the component file and the module file to declare and configure things, which was confusing and tedious. Standalone components make things more straightforward by allowing a component to manage its own dependencies directly, making the code much easier to write, read, and maintain.
