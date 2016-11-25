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

# Requirements
<p align="center">
<a href="https://nodejs.org/en/download/">NodeJS</a>
</p>
* `gulp` — npm install --global gulp-cli


## Create your project file structure
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

## Sample `gulpfile.js`

This file will give you a taste of what gulp does.

```js
var gulp = require('gulp'),
  htmlmin = require('gulp-htmlmin'),
  sass = require('gulp-ruby-sass'),
  cleanCSS = require('gulp-clean-css'),
  imagemin = require('gulp-imagemin'),  
	concat = require('gulp-concat'),
	uglify = require('gulp-uglify'),
	ngAnnotate = require('gulp-ng-annotate'),
  jshint = require('gulp-jshint'),
  inject = require('gulp-inject'),
  webserver = require('gulp-webserver');



gulp.task('lint', function() {
  return gulp.src(['./app/**/*.js','!./app/js/vendor.min.js','!./dist/vendor.min.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

gulp.task('inject', function(){
  return gulp.src('./app/index.html')
    // inject the css files
    .pipe(inject(gulp.src('./app/**/*.css', {read:false}), {relative:true}))
    // inject the js files
    .pipe(inject(gulp.src(['./app/js/vendor.min.js','./app/*.js','./app/modules/**/*js'], {read: false}), {relative:true}))
    .pipe(gulp.dest('./app'));
});

gulp.task('injectbuild', function(){
  return gulp.src('./app/index.html')
    .pipe(inject(gulp.src(['./dist/vendor.min.css','./dist/app.min.css'], {read:false}),{ignorePath: 'dist'}))
    .pipe(inject(gulp.src(['./dist/vendor.min.js','./dist/app.min.js'], {read: false}), {ignorePath: 'dist'}))
    .pipe(gulp.dest('./dist'));
});


gulp.task('bowercss', function() {
    gulp.src([
		'bower_components/bootstrap/dist/css/bootstrap.css'
  	])
  	.pipe(concat('vendor.min.css'))
    .pipe(cleanCSS())
    .pipe(gulp.dest('app/css'))
    .pipe(gulp.dest('dist'));
});

gulp.task('bowerJS', function() {
  gulp.src([
    'bower_components/jquery/dist/jquery.js',
    'bower_components/bootstrap/dist/js/bootstrap.js',
    'bower_components/angular/angular.js',
    'bower_components/angular-ui-router/release/angular-ui-router.js'
  ])
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('app/js'))
    .pipe(gulp.dest('dist'));
});

gulp.task('appCSS',function(){
 sass('styles/*.scss')
 .on('error', sass.logError)
 .pipe(concat('app.min.css'))
 .pipe(cleanCSS())
 .pipe(gulp.dest('app/css'))
 .pipe(gulp.dest('dist'));
});



gulp.task('appJS', function(){
	gulp.src([
		'app/**/*.js',
	])
			.pipe(license('MIT', {tiny: true}))
	.pipe(ngAnnotate())
	.pipe(concat('app.min.js'))
	.pipe(uglify())
	.pipe(gulp.dest('dist'));
});

gulp.task('appHTML', function(){
	gulp.src([
		'app/**/*.html'
	])
	.pipe(htmlmin({collapseWhitespace: true}))
	.pipe(gulp.dest('dist'));
});

gulp.task('imagemin',function(){
    gulp.src('./app/images/*')
        .pipe(imagemin())
        .pipe(gulp.dest('dist/images'));
});

gulp.task('copy', function () {
    gulp.src('bower_components/bootstrap/dist/fonts/*')
       .pipe(gulp.dest('app/fonts'))
        .pipe(gulp.dest('dist/fonts'));
});

gulp.task('watch',function(){
    gulp.watch('app/**/*.js',['appJS']);
    gulp.watch('styles/*.scss',['appCSS']);
});


gulp.task('default', ['lint','bowerJS','bowercss', 'appCSS','watch','copy','inject','serve']);

gulp.task('build', ['lint','bowerJS', 'bowercss', 'appCSS', 'appJS', 'copy','imagemin','injectbuild','appHTML','run']);

gulp.task('run', function() {
  gulp.src('./dist')
    .pipe(webserver({
      livereload: true,
      directoryListing: false,
      open: true,
      port:9000
    }));
});

gulp.task('serve', function() {
  gulp.src('./app')
    .pipe(webserver({
      livereload: true,
      directoryListing: false,
      open: true,
      port:8000
    }));
});
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

3) https://github.com/gulpjs/gulp/blob/master/docs/README.md#articles

