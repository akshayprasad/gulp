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

## Incremental Builds

We recommend these plugins:

- [gulp-changed](https://github.com/sindresorhus/gulp-changed) - only pass through changed files
- [gulp-cached](https://github.com/contra/gulp-cached) - in-memory file cache, not for operation on sets of files
- [gulp-remember](https://github.com/ahaurw01/gulp-remember) - pairs nicely with gulp-cached
- [gulp-newer](https://github.com/tschaub/gulp-newer) - pass through newer source files only, supports many:1 source:dest

## Want to contribute?

Anyone can help make this project better - check out our [Contributing guide](/CONTRIBUTING.md)!

## Backers

Support us with a monthly donation and help us continue our activities.

[![Backers][backers-image]][support-url]

## Sponsors

Become a sponsor to get your logo on our README on Github.

[![Sponsors][sponsors-image]][support-url]

[downloads-image]: https://img.shields.io/npm/dm/gulp.svg
[npm-url]: https://www.npmjs.com/package/gulp
[npm-image]: https://img.shields.io/npm/v/gulp.svg

[travis-url]: https://travis-ci.org/gulpjs/gulp
[travis-image]: https://img.shields.io/travis/gulpjs/gulp/master.svg

[coveralls-url]: https://coveralls.io/r/gulpjs/gulp
[coveralls-image]: https://img.shields.io/coveralls/gulpjs/gulp/master.svg

[gitter-url]: https://gitter.im/gulpjs/gulp
[gitter-image]: https://badges.gitter.im/gulpjs/gulp.svg

[backer-url]: #backers
[backer-badge]: https://opencollective.com/gulpjs/backers/badge.svg?color=blue
[sponsor-url]: #sponsors
[sponsor-badge]: https://opencollective.com/gulpjs/sponsors/badge.svg?color=blue

[support-url]: https://opencollective.com/gulpjs#support

[backers-image]: https://opencollective.com/gulpjs/backers.svg
[sponsors-image]: https://opencollective.com/gulpjs/sponsors.svg
