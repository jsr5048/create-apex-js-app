# create-apex-js-lib

## Introduction

create-apex-js-lib bootstraps a JavaScript project for APEX, that bundles all your source code into a library. During the process of bundling the library, your source code is transformed in the following ways:

-   All JavaScript code is bundled into one file
-   All JavaScript code is transpiled to ES5 syntax
-   Language features that are not part of ES5 and cannot be transpiled (eg. Promise, Map, Set, ...) are automatically polyfilled if you use them in your code
-   In development mode, source maps are created inline within the bundle file to ease debugging in the browsers development console
-   ESLint statically checks the source code with the "eslint:recommended" ruleset
-   CSS files that are imported into the JavaScript source files are extracted with vendor prefixes added into a seperate CSS file
-   All code is minified and shortened

The result of the build process is a JavaScript bundle file, that contains your transpiled source code with the dependencies and all the required polyfills, as well as a CSS file that included the CSS code of all imported CSS files. The bundled file can be used in APEX and exposes your code in one library variable to your application.

All default settings can be changed according to your need after you have created the library.

## Prerequisites

To create your JavaScript library for APEX please make sure you have the follwing installed:

-   node.js >= 8.9.0
-   npm >= 5.2.0

The following tools are optional but greatly enhance your development experience:

-   Visual Studio Code
-   Prettier Plugin for Visual Studio Code (a configuration file for Prettier is included)

## Installation

To create your JavaScript library for APEX, run the following command in your shell with your library name replaced (the library name may only include letters, numbers, underscores and hashes):

```bash
npx create-apex-js-lib <library-name>
```

This will download and run the project generator in one step, without leaving anything installed on your machine from the generator application itself.

You can also use a specific version for the generation of your library if you wish:

```bash
npx create-apex-js-lib@1.0.1 <library-name>
```

This uses version 1.0.1 of the project to create your library.

Alternatively, you can also install the package globally and run it seperately:

```bash
npm i -g create-apex-library
create-apex-library <library-name>
```

Please note, that if you are using this way, you need to manually update the create-apex-js-lib project, in order to create your library with the newest version.

### Options

By default, the required dependencies for your library are installed during the creation. If you do not want this and rather install the dependencies at a later point yourself, you can achieve this by running:

```bash
npx create-apex-js-lib <library-name> --noinstall
```

### Questions during creation of library

During the creation of your library, you will be asked a few additional questions:

-   **Library code**: This will be the code that is exposed on your APEX page and contains all of your source code. As with the library name, the library code may only include letters, numbers, underscores and hashes. By default, your library name is used for the library code as well.

-   **Initial version**: Define the initial version of your library. The version needs to follow the [semantic versioning](https://semver.org/) rules. By default, version 1.0.0 is used.

## Usage

The project contains a "src" folder with a file called "main.js". This is the default entry point for the bundle generation and the place where your JavaScript code should go. You can import any node package that you have installed or local modules that you have created in seperate files. All exported variables, objects or functions within the main.js file will be accessible in your library. Check the "examples" folder in your project to see how you can add your code.

### Development build

To create a bundle for development, issue the following command from the root of the project:

```bash
npm run dev
```

This will create the bundled JavaScript and CSS files with your transformed source code in the "dist" folder. Further it will watch the source folder for changes and will rebundle the file whenever a change occurs.

### Create documentation

To create the JSDoc documentation from your source code, run the follwing command:

```bash
npm run doc
```

This will create the documentation in the "dist/doc" folder of your project.

### Prodution build

To bundle everything for production, issue the following command:

```bash
npm run build
```

This will create JSDoc documentation and the bundled JavaScript and CSS files, but without source maps. With the "build" command, only the build is run and no watcher for further changes is started.

## Configuration

You can change your build to your needs by changing the following configuration files:

-   [package.json](https://docs.npmjs.com/files/package.json): Root configuration file of your project. Includes the name of your library, the exposed library code, the version of your library and many more.
-   [rollup.config.js](https://rollupjs.org/guide/en): Configuration file for the bundle process of your library
-   [.eslintrc.json](https://eslint.org/docs/user-guide/configuring): Settings file for ESLint. By default the predefined [eslint:recommended](https://eslint.org/docs/rules/) rules are used for ESlint. If you want to use another set, you can change it in this configuration file. The .eslintignore file in addition includes all the files that should be ignored by ESLint
-   [.prettierrc](https://github.com/prettier/prettier): Rules for the Prettier extension in VSCode
-   [jsdoc.conf](http://usejsdoc.org/about-configuring-jsdoc.html): Configuration file for documentation creation with JSDoc

### Externals

Externals are parts of your library, that will not be included in your bundle, as they are already loaded on your page. By default, the apex and jQuery libraries are excluded from the bundle process.

#### apex

The apex JavaScript API is passed to your library as an argument when it is loaded. You therefore need to ensure, that your library is loaded after the apex library.

#### jQuery

The jQuery library is included in the apex JavaScript API and can be referenced with "apex.jQuery". Additionally, you can map "apex.jQuery" to the "$" variable in your files if you wish such as:

```javascript
const $ = apex.jQuery;
```

As you are working in your own namespace, it is safe to override the $ variable. This way you ensure that you always access the jQuery library from the apex JavaScript API while beeing able to continue to use the "$" shortcut.

#### Other

For other external libraries that are already available on your site and you do not want to add to your library as a dependency, you can extend the configuration. In the rollup.config.js file you will find a "globals" and "external" attribute that you can amend to your needs.

## Author

Daniel Frech, 2018

## License

[MIT](LICENSE)
