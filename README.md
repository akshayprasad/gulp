<p align="center">
  <a href="http://gulpjs.com">
    <img height="257" width="114" src="https://raw.githubusercontent.com/gulpjs/artwork/master/gulp-2x.png">
  </a>
  <p align="center">The streaming build system</p>
</p>


## What is gulp?

- **Automation** - gulp is a toolkit that helps you automate painful or time-consuming tasks in your development workflow.
- **Platform-agnostic** - Integrations are built into all major IDEs and people are using gulp with PHP, .NET, Node.js, Java, and other platforms.
- **Strong Ecosystem** - Use npm modules to do anything you want + over 2000 curated plugins for streaming file transformations
- **Simple** - By providing only a minimal API surface, gulp is easy to learn and simple to use

## Documentation

For a Getting started guide, API docs, recipes, making a plugin, etc. check out our docs!

- Need something reliable? Check out the [documentation for the current release](/docs/README.md)!
- Want to help us test the latest and greatest? Check out the [documentation for the next release](https://github.com/gulpjs/gulp/tree/4.0)!

## Sample `gulpfile.js`

This file will give you a taste of what gulp does.

```js
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    sass = require('gulp-ruby-sass'),
    livereload = require('gulp-livereload'),
    zip = require('gulp-zip'),
    imagemin = require('gulp-imagemin'),
    autoprefixer = require('gulp-autoprefixer');
//Scripts Uglifies task
function errorLog(error){
console.error.bind(error);
this.emit('end');
}
gulp.task('scripts',function(){
  gulp.src('app/**/*.js')
  .pipe(uglify())
   .on('error',errorLog)
  .pipe(gulp.dest('dist/'));
});

//Styles Uglifies task

gulp.task('styles',function(){
 sass('app/**/*.scss')
 .on('error',errorLog)
 .pipe(autoprefixer(autoprefixer({
            browsers: ['last 2 versions'],
            cascade: false
        })))
 .pipe(gulp.dest('app/'))
 .pipe(gulp.dest('dist/'))
 .pipe(livereload());
});


//create zip file task
gulp.task('zip', function(){
    return gulp.src('dist/**/')
        .pipe(zip('archive.zip'))
         .on('error',errorLog)
        .pipe(gulp.dest('zip/'));
});
//image compress task
gulp.task('imagemin',function(){
    gulp.src('app/images/*')
        .pipe(imagemin())
        .pipe(gulp.dest('dist/images'));
});

//watch task Watches JS
gulp.task('watch',function(){
    livereload.listen();
    gulp.watch('app/**/*.js',['scripts']);
    gulp.watch('app/**/*.scss',['styles']);
});


gulp.task('default',['scripts','styles','watch']);
gulp.task('build',['scripts','styles','watch','zip']);
```
## File Structure
```
.
├── app
|   ├── css
│   ├── images
│   ├── js
│   └── modules
│   │    ├── login
│   │    │   └── login.html
│   │    │   └── loginCtrl.js
│   │    └──  signup
│   │        └── signup.html
│   │        └── signupCtrl.js
|   │  
|   ├── app.js 
|   ├── config.js
|   └── index.html
|
├── dist
├── .bowerrc
├── .jshintrc
├── bower.json
├── gulpfile.js
└── package.json
```

## Required plugins
- [gulp](https://www.npmjs.com/package/gulp) - The streaming build system.
- [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin) - gulp plugin to minify HTML.
- [gulp-ruby-sass](https://github.com/sindresorhus/gulp-ruby-sass) - Compile Sass to CSS with Ruby Sass.
- [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css) - Minify css with clean-css.
- [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin) - Minify PNG, JPEG, GIF and SVG images.
- [gulp-concat](https://www.npmjs.com/package/gulp-concat) - Concatenates files.
- [gulp-uglify](hhttps://www.npmjs.com/package/gulp-uglify) - Minify files with UglifyJS.
- [gulp-ng-annotate](https://www.npmjs.com/package/gulp-ng-annotate) - Add angularjs dependency injection annotations with ng-annotate
- [gulp-jshint](https://www.npmjs.com/package/gulp-jshint) - JSHint plugin for gulp (Code Analysis Tool for JavaScript)
- [gulp-inject](https://www.npmjs.com/package/gulp-inject) - A javascript, stylesheet and webcomponent injection plugin for Gulp, i.e. inject file references into your index.html
- [gulp-webserver](https://www.npmjs.com/package/gulp-webserver) - Gulp plugin to run a local webserver with LiveReload


## Incremental Builds

We recommend these plugins:

- [gulp-changed](https://github.com/sindresorhus/gulp-changed) - only pass through changed files
- [gulp-cached](https://github.com/contra/gulp-cached) - in-memory file cache, not for operation on sets of files
- [gulp-remember](https://github.com/ahaurw01/gulp-remember) - pairs nicely with gulp-cached
- [gulp-newer](https://github.com/tschaub/gulp-newer) - pass through newer source files only, supports many:1 source:dest


# Reference Links
1) https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md

2) https://github.com/gulpjs/gulp/tree/master/docs/recipes

