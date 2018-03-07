# WordPress-Workflow-With-Gulp-and-Browsersync

Considering you have a wordpress instalation on you computer... 

The first thing you should do is install [Node](https://nodejs.org) and [Gulp](https://gulpjs.com/).

## Gulp and browsersync instalation

### Creat a packaje.json file

* Open the terminal on your wp theme directory `$ cd .../wp-content/theme/your-theme`
* `$ npm init` to creat a packaje.json file

### Install gulp, gulp sass, browsersync and gulp-imagemin

* `$ npm install gulp gulp-sass browser-sync gulp-imagemin --save-dev`

Note that you have inside your theme folder a node_modules folder

The next step is creat a `gulpfile.js` and inside your gulpfile.js is where all the magic happens.

### To set up gulpfile.js

open gulpfile.js.

##### Include de necessary modules
```
var gulp = require('gulp'),
    browserSync = require('browser-sync'),
    sass = require('gulp-sass');
    
    
```

##### Configure o BrowserSync.
```
 gulp.task('browser-sync', function () {
      var files = [
          './style.css',
          './*.php',
          './js/*.js'
      ];

      //Iniciando BrowserSync com o PHP
      browserSync.init(files, {
          proxy: "http://localhost:8888/"
      });
  });
  
```
##### Configure sass task to run when specified .scss file change
##### Browsersync will also reload browsers
 ```
    gulp.task('sass', function () {
        return gulp.src('sass/*.scss')
            .pipe(sass({
                    'outputStyle' : 'compressed'
                }))
            .pipe(gulp.dest('./'))
            .pipe(browserSync.stream())
    });
 ```
 
 ##### Otimizando images
 ```
    gulp.task('img', function() {
        gulp.src('assets/images/*.{png,jpg,gif}')
            .pipe(imagemin({

                optimizationLevel: 7,
                progressive: true

            }))
            .pipe(gulp.dest('img'))
    });
 ```
##### Creat a default task that can be called using 'gulp.
##### The task will process sass, run browser-sync and start watching for changes.
```
  gulp.task('default', ['sass', 'browser-sync'], function () {
      gulp.watch("sass/**/*.scss", ['sass']);
  })

```

Now, open yor terminal, make sure it's the root folder and tipe `$ gulp ` 

