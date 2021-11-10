# Gulp.js

## Install

```bash
#npm install --save-dev

# gulp
npm i -D gulp gulp-cli
# gulp-babel for js
npm i -D gulp-babel @babel/core @babel/preset-env
# gulp-uglify
npm i -D gulp-uglify
# gulp-sass
npm i -D gulp-sass sass
# gulp-clean-css
npm i -D gulp-clean-css
# del
npm i -D del
```

```javascript
// gulpfile.js
const gulp = require('gulp'),
    babel = require('gulp-babel'),
    minify_js = require('gulp-uglify'),
    sass = require('gulp-sass')(require('sass')),
    minify_css = require('gulp-clean-css'),
    del = require('del');

// Env
const SRC = 'src/',
    SRC_SCRIPT = SRC + '/script/**/*.js',
    SRC_STYLE = SRC + '/sass/**/*.scss',
    DEST = 'output/assets',
    DEST_SCRIPT = DEST + '/js/',
    DEST_STYLE = DEST + '/css/',
    CLEAN_PATH = DEST + '/**';

// Task
const clean = function (cb) {

    return del([CLEAN_PATH,]);

}, compile_js = function (cb) {

    return gulp.src(SRC_SCRIPT)
        .pipe(babel({ presets: ['@babel/env',], }))
        .pipe(minify_js({ mangle: false, }))
        .pipe(gulp.dest(DEST_SCRIPT));

}, compile_css = function (cb) {

    return gulp.src(SRC_STYLE)
        .pipe(sass().on('error', sass.logError))
        .pipe(minify_css({ compatibility: 'ie8' }))
        .pipe(gulp.dest(DEST_STYLE));

};

// Export command
exports.default = gulp.series(clean, gulp.parallel(compile_css, compile_js));

exports.clean = clean;
```

## Directory structure

```text
+--- dest
    +--- assets
        +--- css
        +--- js
    home.tpl (html founder template)
    column.tpl
    article.tpl
+--- src
    +--- sass
    +--- script
```
