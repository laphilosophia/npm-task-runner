# npm-build-boilerplate

A collection of packages that build a website using `npm scripts`.

* [List of packages used](#list-of-packages-used)
* [Using in your project](#using-in-your-project)
* [List of available tasks](#list-of-available-tasks)
* [Need help?](#need-help)

## List of packages used
[autoprefixer](https://github.com/postcss/autoprefixer), [browser-sync](https://github.com/Browsersync/browser-sync), [eslint](https://github.com/eslint/eslint), [imagemin-cli](https://github.com/imagemin/imagemin-cli), [node-sass](https://github.com/sass/node-sass), [onchange](https://github.com/Qard/onchange), [npm-run-all](https://github.com/mysticatea/npm-run-all), [postcss-cli](https://github.com/code42day/postcss-cli), [svgo](https://github.com/svg/svgo), [svg-sprite-generator](https://github.com/frexy/svg-sprite-generator), [uglify-js](https://github.com/mishoo/UglifyJS2).

Many, many thanks go out to Keith Cirkel for [his post](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/) and his useful CLI tools!

## Using in your project
* First, ensure that node.js & npm are both installed. If not, choose your OS and installation method from [this page](https://nodejs.org/en/download/package-manager/) and follow the instructions.
* Next, use your command line to enter your project directory.
  * If this a new project (without a `package.json` file), start by running `npm init`. This will ask a few questions and use your responses to build a basic `package.json` file. Next, copy the `"devDependencies"` object into your `package.json`.
  * If this is an existing project, copy the contents of `"devDependencies"` into your `package.json`.
* Now, copy any tasks you want from the `"scripts"` object into your `package.json` `"scripts"` object.
* Finally, run `npm install` to install all of the dependencies into your project.

You're ready to go! Run any task by typing `npm run task` (where "task" is the name of the task in the `"scripts"` object). The most useful task for rapid development is `watch`. It will start a new server, open up a browser and watch for any SCSS or JS changes in the `source` directory; once it compiles those changes, the browser will automatically inject the changed file(s)!

## List of available tasks
### `clean`
  `rm -f dist/{styles/*,scripts/*,images/*}`

  Delete existing dist files

### `autoprefixer`
  `postcss -u autoprefixer -r dist/styles/*`

  Add vendor prefixes to your CSS automatically

### `scss`
  `node-sass --output-style compressed -o dist/styles source/styles`

  Compile Scss to CSS

### `lint`
  `eslint source/scripts`

  "Lint" your JavaScript to enforce a uniform style and find errors

### `uglify`
  `mkdir -p dist/scripts && uglifyjs source/scripts/*.js -m -o dist/scripts/app.js && uglifyjs source/scripts/*.js -m -c -o dist/scripts/app.min.js`

  Uglify (minify) a production ready bundle of JavaScript

### `imagemin`
  `imagemin source/images/* -o dist/images`

  Compress all types of images

### `icons`
  `svgo -f source/images/icons && mkdir -p dist/images && svg-sprite-generate -d source/images/icons -o dist/images/icons.svg`

  Compress separate SVG files and combine them into one SVG "sprite"

### `serve`
  `browser-sync start --server --files 'dist/styles/*.css, dist/scripts/*.js, **/*.html, !node_modules/**/*.html'`

  Start a new server and watch for CSS & JS file changes in the `dist` folder

### `build:styles`
  `run-s scss autoprefixer`

  Alias to run the `scss` and `autoprefixer` tasks. Compiles Scss to CSS & add vendor prefixes

### `build:scripts`
  `run-s lint concat uglify`

  Alias to run the `lint`, `concat` and `uglify` tasks. Lints JS, combines `source` JS files & uglifies the output

### `build:images`
  `run-s imagemin icons`

  Alias to run the `imagemin` and `icons` tasks. Compresses images, generates an SVG sprite from a folder of separate SVGs

### `build`
  `run-s build:*`

  Alias to run all of the `build` commands

### `watch:css`
  `onchange 'source/**/*.scss' -- run-s build:styles`

  Watches for any .scss file in `source` to change, then runs the `build:styles` task

### `watch:js`
  `onchange 'source/**/*.js' -- run-s build:scripts`

  Watches for any .js file in `source` to change, then runs the `build:scripts` task

### `watch:images`
  `onchange 'source/images/**/*' -- run-s build:images`

  Watches for any images in `source` to change, then runs the `build:images` task

### `watch`
  `run-p serve watch:*`

  Run the following tasks simultaneously: `serve`, `watch:styles`, `watch:scripts` & `watch:images`. When a .scss or .js file changes in `source` or an image changes in `source/images`, the task will compile the changes to `dist`, and the server will be notified of the change. Any browser connected to the server will then inject the new file from `dist`

### `postinstall`
  `run-s build watch`

  Runs `watch` after `npm install` is finished


## Need help?
Feel free to [create an issue](https://github.com/clubjolly/npm-task-runner/issues), [tweet me](http://twitter.com/laphilosophias), or [send me an email](mailto:erdemarslan@ymail.com). I'd be glad to help where I can!
