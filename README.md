after updated to Gulp 4 I needed to update my gulpfile.js
# WordPress Workflow With Gulp-4, Gulp-sass, Gulp-watch, Gulp-concat and Browsersync

Considering you have a wordpress instalation on you computer... 

The first thing you should do is install [Node](https://nodejs.org) and [Gulp](https://gulpjs.com/).

## Gulp and browsersync instalation

### Creat a packaje.json file

* Open the terminal on your wp theme directory `$ cd .../wp-content/theme/your-theme`
* `$ npm init` to creat a packaje.json file

### Install gulp extentions and browsersync

* `$ npm install gulp gulp-watch gulp-sass gulp-concat gulp-uglify browser-sync --save-dev`

Note that you have inside your theme folder a node_modules folder

The next step is creat a `gulpfile.js` and inside your gulpfile.js is where all the magic happens.

### To set up gulpfile.js

open gulpfile.js.

##### Include the necessary modules
```
var gulp = require( 'gulp' ),
    watch = require( 'gulp-watch' ),
    sass = require( 'gulp-sass' ),
    concat = require('gulp-concat'),
    uglify = require('gulp-uglify'),
    browserSync = require('browser-sync').create()
    
    
```

##### create variables to watch
```
// Sass files to watch
var cssFiles = [
    './sass/**/*.scss'
];

// Js files to watch
var jsFiles = [
    './js/scripts.js'
];
    
    
```

##### Configure the BrowserSync.
```
// automatically reloads the page when files changed
var browserSyncWatchFiles = [
    './*.css',
    './js/*.js',
    './**/*.php'
];

// see: https://www.browsersync.io/docs/options/
var browserSyncOptions = {
    watchTask: true,
    proxy: "http://localhost:8080/" 
}
  
```
##### Configure sass task to run when specified .scss file change
##### Browsersync will also reload browsers
 ```
   gulp.task('sass', function() {
    return gulp.src('./sass/style.scss')
        .pipe(sass({
            errLogToConsole: true,
            precision: 8,
            noCache: true,
        }).on('error', sass.logError))
        .pipe(gulp.dest('.'))
        .pipe(sass({ outputStyle:'compressed'}).on('error', sass.logError))
        .pipe(gulp.dest('.'))
        .pipe(browserSync.reload({stream: true}))
    });
 ```
 
 ##### Js - Creates a regular and minified .js file in root
 ```
gulp.task('compress', function () {
    return gulp.src([
        "./js/*.js"
    ])
        .pipe(concat('scripts.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./js'))
        .pipe(browserSync.reload({stream: true}))
});
 ```
 
 ##### Starts browser-sync task for starting the server.
 ```
    gulp.task('browser-sync', function() {
        browserSync.init(browserSyncWatchFiles, browserSyncOptions);
    });
 ```
 
##### Start the livereload server and watch files for change
##### The task will process sass, run browser-sync and start watching for changes.
```
  gulp.task( 'watch', function() {

    gulp.watch( cssFiles, gulp.parallel('sass') );
    gulp.watch( jsFiles, gulp.parallel('compress') );

});


gulp.task( 'default', gulp.parallel('watch', 'browser-sync'));

```

Now, open yor terminal, make sure it's the root folder and type `$ gulp ` 

