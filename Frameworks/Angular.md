---
date: 2025-12-16
tags:
  - Angular
  - Frontend
  - Framework
---
## 📋 **Table of Contents**

- [1. Creating the Project](#1-creating-the-project)
- [2. Creation of the GIT repository](#2-creation-of-the-git-repository)
- [3. Project configuration](#3-project-configuration)
  - [3.1 Create environment](#31-create-environment)
  - [3.2 Configure target-specific file replacements](#32-configure-target-specific-file-replacements)
  - [3.3 How to use the import](#33-how-to-use-the-import)
  - [3.4 Run the project with the right environment](#34-run-the-project-with-the-right-environment)
- [4. Architecture](#4-architecture)
- [5. RxJS – Avoid nested subscribe](#5-rxjs--avoid-nested-subscribe)

---

## 1. Creating the Project

To create the project, use this command:

```bash
ng new NameOfTheProject
```

If you want to add material to the project, use this command:

```bash
ng add @angular/material
```

## 2. Creation of the [[GIT]] repository

Don't forget to create or adapt if it's already created the [[GIT#.gitignore|.gitignore]]

## 3. Project configuration

### 3.1 Create environment

This is the file where you store all your environment variables.
Create environment using this command:

```bash
ng generate environments
```

To create a new environment, simply create a new environment file.

- development

```ts
export const environment = {
  production: false,
  API_URL: 'https://127.0.0.1:8000' //This should be the ip of the remote server
};
```

- staging (This environment is for testing purposes)``

```ts
export const environment = {
  production: false,
  API_URL: 'https://api-staging.example.com'
};
```

- Local-development

```ts
export const environment = {
  production: false,
  API_URL: 'http://127.0.0.1:8000/'
};
```

- Production

```ts
export const environment = {
  production: true,
  API_URL: 'https://yourProject.com' 
};
```
> Naming convention may vary, but Angular recommends `environment.<target>.ts`

### 3.2. Configure target-specific file replacements

The main CLI configuration file, `angular.json`, contains a `fileReplacements` section in the configuration for each build target, which lets you replace any file in the TypeScript program with a target-specific version of that file. This is useful for including target-specific code or variables in a build that targets a specific environment, such as production or staging.

By default no files are replaced. You can add file replacements for specific build targets. For example:

```json
"configurations": {
  "development": {
    "fileReplacements": [
        {
          "replace": "src/environments/environment.ts",
          "with": "src/environments/environment.development.ts"
        }
      ],
      …
```

This means that when you build your development configuration with `ng build --configuration development`, the `src/environments/environment.ts` file is replaced with the target-specific version of the file, `src/environments/environment.development.ts`.

You can add additional configurations as required. To add a staging environment, create a copy of `src/environments/environment.ts` called `src/environments/environment.staging.ts`, then add a `staging` configuration to `angular.json`:

```json
"configurations": {
  "development": { … },
  "production": { … },
  "staging": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.staging.ts"
      }
    ]
  }
}
```

You can add more configuration options to this target environment as well. Any option that your build supports can be overridden in a build target configuration.

You can also configure the `serve` command to use the targeted build configuration if you add it to the "serve:configurations" section of `angular.json`:

```json
"serve": {
  "builder": "@angular-devkit/build-angular:dev-server",
  "options": {
    "browserTarget": "your-project-name:build"
  },
  "configurations": {
    "development": {
      "browserTarget": "your-project-name:build:development"
    },
    "production": {
      "browserTarget": "your-project-name:build:production"
    },
    "staging": {
      "browserTarget": "your-project-name:build:staging"
    }
  }
},
```

### 3.3. How to use the import

When you want to use the API_URL variable in your code, import it like this. Do not precise the file.

``` ts
import { environment } from './../environments/environment';
```

### 3.4. Run the project with the right environment

To build using the using a specific configuration, run the following command:

```bash
ng build --configuration=NameOfYourConfig
```

To serve using the using a specific configuration, run the following command:

```bash
ng serve --configuration=NameOfYourConfig
```

## 4. Architecture

```bash
F:\YourProject\src
├───app
│   ├───core
│   │   ├───dtos
│   │   ├───directives
│   │   ├───guards
│   │   ├───interceptors
│   │   ├───models
│   │   ├───pipes
│   │   ├───services
│   │   └───validators
│   └───pages
├───shared
|   ├───components
|   └───styles	
├───assets
│   └───img
└───environments
```

### Naming conventions (Angular / TypeScript)

> Angular does not enforce file naming conventions, but following common patterns improves readability, tooling, and team collaboration.

### General pattern

```ts
<feature>.<purpose>.ts
```

### Common examples

```ts
user.component.ts 
user.service.ts 
user.model.ts 
user.dto.ts 
user.guard.ts 
user.interceptor.ts 
user.resolver.ts 
user.pipe.ts
...
```

## 5. RxJS – Avoid nested subscribe

> **Problem**: When one HTTP call depends on the result of another, using nested `.subscribe()` leads to unreadable, hard-to-maintain code.

### 5.1 ❌ The problem: nested subscribe

Example of what **not** to do:

```ts
this.userService.getUser(userId).subscribe(user => {
   this.orderService.getOrders(user.id).subscribe(orders => {  
	   this.orders = orders;   
   }); 
});
```

**Issues:**
- Hard to read (callback hell)
- Error handling becomes complex
- Difficult to unsubscribe properly
- Not idiomatic RxJS

---

### 5.2 ✅ The solution: `pipe()` + `switchMap()`

Instead of subscribing multiple times, you **transform the observable**.

```ts
this.userService.getUser(userId).pipe(
   switchMap(user => this.orderService.getOrders(user.id)) ).subscribe(orders => {
  this.orders = orders; 
});

```

**Why this works:**

- `getUser()` emits a `user`
- `switchMap()` uses that value to create a **new observable**
- RxJS automatically:
    - cancels previous inner subscriptions
    - keeps only the latest one

💡 **Rule of thumb**:

> If a second observable depends on the result of the first → `switchMap`

---

### 5.3 How `switchMap` works (simplified)


```ts
sourceObservable.pipe(   
	switchMap(valueFromSource => anotherObservable(valueFromSource)) 
)
```
- Each emission **switches** to a new observable
- Previous inner observable is **cancelled**
- Perfect for:
    - HTTP calls
    - route changes
    - user interactions

---

### 5.4 Comparison with other operators

|Operator|Behavior|
|---|---|
|`switchMap`|Cancels previous request (most common for HTTP)|
|`mergeMap`|Runs all requests in parallel|
|`concatMap`|Queues requests (one after another)|
|`exhaustMap`|Ignores new emissions while one is running|

![[operators.png]]

---

### 5.5 Understanding `.subscribe({ next, error, complete })`

A `subscribe()` can receive an **observer object**:


```ts
this.userService.getUser(userId).subscribe({
	next: user => {
		console.log('User received', user);   
	},   
	error: err => {
		console.error('Error', err);   
	 },   
	 complete: () => {     
		 console.log('Observable completed');   
	 } 
 });
```
#### `next`

- Called **each time** the observable emits a value
- For HTTP calls → usually **once**

#### `error`

- Called if the observable fails
- Automatically stops the stream

#### `complete`

- Called when the observable finishes normally
- HTTP observables always complete

---

### 5.6 Best practice summary

✅ **Do**

- Use `pipe()` to compose observables
- Use `switchMap` for dependent HTTP calls
- Subscribe **once**, at the end

❌ **Avoid**

- Nested `subscribe()`
- Business logic inside `subscribe`
- Multiple subscriptions in components