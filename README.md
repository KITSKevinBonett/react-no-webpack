# react-no-webpack
POC for React UI library + CSS Modules + Sass (but without Webpack)

## Test components in browser
- `npm run build`
- Open "index.html" in browser.

## Export libary for consumption as external package in another project
- `npm run lib`
- `npm link` - one time operation to register library as a global package.
- In another project, `npm link react-no-webpack` - one time operation.
- Can now `import { WidgetCSS, WidgetSass } from 'react-no-webpack';` etc.

## So how does all this work without Webpack?

### 1. Node CLI and NPM scripts

As defined in "package.json".

- ```"lib": "babel ./src/lib --out-dir ./lib --copy-files"``` - **babel** transpiles ES6 syntax to ES5 for exporting library. Babel references ".babelrc" file for required presets.
- ```"build": "browserify ./src/index.js -o ./build/bundle.js -t babelify``` - this portion of the "build" uses **browserify** to bundle all our modules into 1 single output file (to enable us to view React components in browser), and **babelify** plugin to transpile ES6 to ES5.
- ```"build": "browserify -p [ css-modulesify --before postcss-nested --before postcss-simple-vars -o build/main.css ]"``` - this portion of the "build" uses **css-modulesify** plugin to provide CSS Modules functionality (outputting a single CSS file, and allowing some basic Sass functionality via **postcss** plugins).

### 2. Library setup

#### "package.json" configuration

- Package name has been defined - e.g. ```"name": "react-no-webpack"```.
- Library entry point has been defined - e.g. ```"main": "lib/index.js"```.
- Library has been exported with ```npm run lib```.

#### Component configuration

- Each component in "src\lib\components" has an associated ".js", ".scss" and ".json" file.
- Each component is exported as a module in "src\lib\index.js" - e.g. ```export { default as WidgetCSS } from './components/WidgetCSS';```.
