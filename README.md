gulp-ember-templates
====================

Gulp plugin for compiling ember.js templates.

This plugin will compile templates for use in Ember 1.8 and newer. Use `gulp-ember-templates@0.5.2` to generate templates for Ember 1.7 and older.

Usage
====================
Start by installing ``` gulp-ember-templates ```

```
npm install --save-dev gulp-ember-templates
```

Then you can use the plugin in your ```gulpfile.js``` to output your templates
in one the following formats

### Browser Output

```javascript
var gulp = require('gulp');
var concat = require('gulp-concat');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates())
    .pipe(concat('ember-templates.js')) // make sure to only do concat after
    .pipe(gulp.dest('./some/other/place'));
});
```

Note: ``` concat ``` is not mandatory, however this will produce a single file
to reference in your html page. This must appear after the call to the 
``` gulp-ember-templates ```

### AMD Output

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      type: 'amd'
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

### CJS Output

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      type: 'cjs'
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

### ES6 Output

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      type: 'es6'
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

API Options
====================

### options.type

Type: ``` String ```,
Default: ``` browser ```

This options specifies the output type that will be used. Available types
* ``` browser ``` - Output plain JavaScript files
* ``` amd ``` - Output AMD modules
* ``` cjs ``` - Output CJS modules
* ``` es6 ``` - Output ES6 modules

### options.moduleName

Type: ``` String ```,
Default: ``` templates ```

This options specifies the root module name and is only used
when using the ``` options.type ``` of ``` amd ```

Note: You can specify an empty string if you wish to use the template file name
for the module name

### options.name

Type ``` String|Object|Function ```,
Default: the template file name

This option allows you to specify a fixed name or a transform to be used for 
the template name

#### usages

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

// 'string' usage - this is really only useful when compiling a single template
gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      name: 'some_static_name'
    }))
    .pipe(gulp.dest('./some/other/place'));
});

// 'object' usage - replace all '_' with '/'
gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      name: {
        replace: /_/g, // 'replace' is a regex used to find charaters to replace
        with: '/' // 'with' is the string to replace the matches with
      }
    }))
    .pipe(gulp.dest('./some/other/place'));
});

// 'function' usage
gulp.task('default', function () {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      name: function (name, done) {
        /*
          DO NOT throw errors from here, pass them in as the first
          argument to done
        */
        
        // do something with name and pass it into the callback
        
        done(null, name);
      }
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

### options.compiler

Type: ``` Object ```,
Default: ``` null ```

This option allows you to specify a custom compiler to be used (e.g., the HTMLBars-flavored ember-compile-templates script bundled with Ember 1.10).

#### usages

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function() {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      compiler: require('./bower_components/ember/ember-template-compiler')
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

### options.isHTMLBars

Type: ``` Boolean ```,
Default: ``` false ```

This option allows you to determine whether to use the `Ember.HTMLBars` or the `Ember.Handlebars` namespace in the generated template code.

#### usages

```javascript
var gulp = require('gulp');
var emberTemplates = require('gulp-ember-templates');

gulp.task('default', function() {
  gulp.src('./some/place/*.handlebars')
    .pipe(emberTemplates({
      compiler: require('./bower_components/ember/ember-template-compiler'),
      isHTMLBars: true // Will generate `Ember.HTMLBars.template({ ... })`
    }))
    .pipe(gulp.dest('./some/other/place'));
});
```

### options.precompile

Type: ``` Boolean ```,
Default: ``` true ```

Disable this option to skip template precompilation and instead wrap the template content with Ember.HTMLBars.compile. This will reduce template compilation time during development. Don't disable this option for production build.
