# Generic-useful-gulpfile-for-Symfony-Projects
### Features

- Simple Gulpfile.js to concat, minify & regroup your assets


### Needs :
    npm install -g gulp-cli
    npm install gulp-clean --no-bin-links
    npm install gulp-clean --no-bin-links
    npm install gulp-concat --no-bin-links
    npm install gulp-copy --no-bin-links
    npm install gulp-if --no-bin-links
    npm install gulp-less --no-bin-links
    npm install gulp-livereload --no-bin-links
    npm install gulp-rename --no-bin-links
    npm install gulp-uglify --no-bin-links
    npm install gulp-uglifycss --no-bin-links
    npm install gulp-order --no-bin-links
    npm install gulp-jquery-closure --no-bin-links
    npm install gulp-jshint --no-bin-links
    npm install jshint --no-bin-links
    
```js
var gulp = require('gulp'),
    clean = require('gulp-clean'),
    concat = require('gulp-concat'),
    jqc = require('gulp-jquery-closure'),
    order = require('gulp-order'),
    gulpif = require('gulp-if'),
    livereload = require('gulp-livereload'),
    less = require('gulp-less'),
    rename = require('gulp-rename'),
    uglify = require('gulp-uglify'),
    uglifycss = require('gulp-uglifycss'),
    jshint = require('gulp-jshint'),
    // gulpCopy = require('gulp-copy'),
    env = 'prod'; // GULP_ENV=prod gulp
// env = process.env.GULP_ENV; // GULP_ENV=prod gulp

var paths = {
    ressources: [
        'web/bundles/mybundle/ressourcesLikePdf/*',
    ],
    stylesheets: [
        'web/assets/vendor/myfile1.css',
        'web/assets/vendor/myfile2.css'
        //[...]
    ],
    stylesheets_to_watch: [
        'web/assets/vendor/myfile1.css',
        'web/assets/vendor/myfile2.css'
        //[...]
    ],
    images: [
        'web/bundles/mybundle/img/**/*'
        //[...]
    ],
    icons: [
        'web/assets/vendor/font-awesome/fonts/*',
        'web/assets/vendor/bootstrap/fonts/*'
        //[...]
    ],
    javascript: [
        'web/bundles/mybundle/js/g.analytics.js',
        'web/assets/vendor/jquery/dist/jquery.min.js',
        'web/assets/vendor/bootstrap/dist/js/bootstrap.min.js'
        //[...]
    ]
};

/*
 Default task
 */
gulp.task('default', ['clean'], function () {
    var tasks = ['fonts', 'images', 'less', 'js'];
    tasks.forEach(function (val) {
        gulp.start(val);
    });
});

/*
 Fonts task
 */
gulp.task('fonts', function () {
    return gulp.src(paths.icons)
        .pipe(rename({dirname: ''}))
        .pipe(gulp.dest('web/assets/fonts/'));
});

/*
 Images task
 */
gulp.task('images', function () {
    gulp.src(paths.images)
        .pipe(gulp.dest('web/assets/img/'));

    gulp.src(paths.ressources)
        .pipe(gulp.dest('web/assets/ressources/'));
});

/*
 Js tasks
 */
gulp.task('js', function () {
    var tasks = ['js-main'];
    tasks.forEach(function (val) {
        gulp.start(val);
    });
});
gulp.task('js-main', function () {
    gulp.src(paths.javascript)
        // .pipe(jshint())
        // .pipe(jshint.reporter('default'))
        .pipe(order(paths.javascript, {base: './'}))
        .pipe(concat('javascript.js'))
        .pipe(gulpif(env === 'prod', uglify()))
        .pipe(gulp.dest('./web/assets/js'))
        .pipe(livereload());

    //you can set more if needed :
    // gulp.src(paths.javascript2)
    //     .pipe(concat('javascript2.js'))
    //     .pipe(order(paths.javascript2, {base: './'}))
    //     .pipe(gulpif(env === 'prod', uglify()))
    //     .pipe(gulp.dest('./web/assets/js'))
    //     .pipe(livereload());
});

/*
 Less task
 */
gulp.task('less', function () {
    gulp.src(paths.stylesheets)
    //.pipe(sourcemaps.init())
    //     .pipe(less())
        .pipe(concat('stylesheets.css'))
        .pipe(gulpif(env === 'prod', uglifycss()))
        .pipe(gulp.dest('./web/assets/css/'))
        .pipe(livereload());

    //you can set more if needed :
    // gulp.src(paths.stylesheets2)
    // //.pipe(sourcemaps.init())
    // //     .pipe(less())
    //     .pipe(concat('stylesheets2.css'))
    //     .pipe(gulpif(env === 'prod', uglifycss()))
    //     //.pipe(sourcemaps.write('./'))
    //     .pipe(gulp.dest('./web/assets/css/'))
    //     .pipe(livereload());

});

/*
 Clean task
 */
gulp.task('clean', function () {
    return gulp.src(['web/assets/css/', 'web/assets/js/', 'web/assets/images/', 'web/assets/fonts/'])
        .pipe(clean());
});

/*
 Watch task
 */
gulp.task('watch', function () {
    var onChange = function (event) {
        console.log('File ' + event.path + ' has been ' + event.type);
    };

    livereload.listen();
    gulp.watch(paths.stylesheets_to_watch, ['less']).on('change', onChange);
    gulp.watch(
        paths.jquery.concat(paths.bootstrap_js).concat(paths.jquery_datatables).concat(paths.javascript),
        ['js-main']
    ).on('change', onChange);
});
```
