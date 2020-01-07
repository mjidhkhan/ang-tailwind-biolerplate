# AngTailwind

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.3.21.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).

<hr>

## Adding Tailwindcss By:
## Written By [Sean Kerwin](https://dev.to/seankerwin/angular-8-tailwind-css-guide-3m45)

We'll be utilising the `@angular-builders/custom-webpack` package which will essentially allow us to tailor the webpack build to add tailwind into the build process.

First things first, install the following:

```shell
$ npm i tailwindcss postcss-scss postcss-import postcss-loader @angular-builders/custom-webpack -D
```
Next you need to open up your Angular project, I'm going to assume you're using sass, as well, why the fuck not?

Open your styles.scss file and add the following at the top.
```css
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';
```
Next, you need to create the tailwind config file, to do so, open your terminal and smash in the following (inside the directory of your app of course).
```shell
npx tailwind init
```

Now we need to extend the webpack config, first, start by `creating webpack.config.js` at the root of your project, this is what it should look like
```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.scss$/,
                loader: 'postcss-loader',
                options: {
                    ident: 'postcss',
                    syntax: 'postcss-scss',
                    plugins: () => [
                        require('postcss-import'),
                        require('tailwindcss'),
                        require('autoprefixer'),
                    ]
                }
            }
        ]
    }
};
```
Once you've added that, modify the angular.json file to tell it to use the custom builder and config file.
```js
{
  "architect": {
    "build": {
      "builder": "@angular-builders/custom-webpack:browser",
      "options": {
        "customWebpackConfig": {
          "path": "./webpack.config.js"
        }
      }
    },
    "serve": {
      "builder": "@angular-builders/custom-webpack:dev-server",
      "options": {
        "customWebpackConfig": {
          "path": "./webpack.config.js"
        }
      }
    }
  }
}
```
Now we're all up and ready to rumble, add some tailwind classes to an object and run

```shell
$ npm start
# or
$ ng serve
```
Extra

The whole point of Tailwind is to create custom utilities, you can do so by doing the following:

Modify your styles.scss and add 2 new custom imports.
```css
@import 'tailwindcss/base';
@import 'tailwindcss/components'; 
@import './custom-tailwind/custom-components.css'; // added
@import 'tailwindcss/utilities';
@import './custom-tailwind/custom-utilities.css'; // added
```
Make sure that they're added in the exact order above.

You can now create a folder inside the src directory called custom-tailwind
Inside this, you create the 2 files the same names as above, now you can create custom reusable utilities and components, for example:

```css
custom-components.css
.btn {
  @apply px-4;
  @apply py-2;
  @apply rounded;
  @apply bg-indigo-500;
  @apply text-white;
  transition: 200ms;
}

.btn:hover {
  @apply bg-indigo-600;
}
```
And now anywhere you want a button, just give it the class .btn and it will apply all the tailwind classes.

For custom-utilities.css, you can do the following:
```css
.rotate-0 {
    transform: rotate(0deg);
}
.rotate-90 {
    transform: rotate(90deg);
}
.rotate-180 {
    transform: rotate(180deg);
}
.rotate-270 {
    transform: rotate(270deg);
}
```
And now anywhere, you can just call these classes. Neat huh!

Enjoy the power that is TailwindCSS + Angular 8!


