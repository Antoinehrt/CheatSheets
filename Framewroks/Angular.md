# New Angular Project
##  1. Creating the Project

To create the project, use this command: 
```bash
ng new NameOfTheProject
```

If you want to add material to the project, use this command: 
```bash
ng add @angular/material
```

# 2. Creation of the [[GIT]] repository

Don't forget to create or adapt if it's already created the .gitignore.

# 3. Project configuration

## 3.1 Create environment

It's the file were you stock all your environment variable
Create environment using this command:
```bash
ng generate environments
```

To create a new environment, just create new files.

- development
```ts
export const environment = {
  production: false,
  API_URL: 'https://127.0.0.1:8000' //This should be the ip of the distant server
};
```

- staging (This environment is for testing purposing)``
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

## 3.2. Configure target-specific file replacements

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

## 3.3. How to use the import

When you want to use the API_URL variable in your code, import it like this. Do not precise the file. 

``` ts
import { environment } from './../environments/environment';
```


## 3.4. Run the project with the right environment


To build using the using a specific configuration, run the following command:

```bash
ng build --configuration=NameOfYourConfig
```


To serve using the using a specific configuration, run the following command:

```bash
ng serve --configuration=NameOfYourConfig
```


# 4. Architecture

```bash
F:\YourProject\src
├───app
│   ├───core
│   │   ├───common
│   │   ├───directives
│   │   ├───guards
│   │   ├───interceptors
│   │   ├───models
│   │   ├───pipes
│   │   ├───services
│   │   └───validators
│   └───pages
├───assets
│   └───img
└───environments
```

