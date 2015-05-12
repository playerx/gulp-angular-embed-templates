# angular-inject-templates
gulp plugin to include the contents of angular templates inside directive's code

Plugin searches for <i>templateUrl: {template url}</i> and replace it with <i>template: {minified template content}</i>. To archive this template first minified with [minimize](https://www.npmjs.com/package/minimize)

Nearest neighbours are:

*   gulp-angular-templates - good for single page applications, combine all templates in one module. *angular-inject-templates* is better for **multi page applications**, where different pages use different set of angular directives so combining all templates in one is not an option. For single page applications they are similar but *angular-inject-templates* doesn't forces you to change your code for using some additional module: just replace template reference with the template code.
*   gulp-include-file - can be used for the same purpose (file include) with *minimize* plugin as transform functions. *angular-inject-templates* do all of this out of the box.

## Install

    npm install --save-dev angular-inject-templates

## Usage

Given the following javascript file:

```javascript
angular.module('test').directive('helloWorld', function () {
    return {
        restrict: 'E',
        templateUrl: 'hello-world-template.html'
    };
});
```

And the following `hello-world-template.html` in the same directory (actually it can be anywhere):

```html
<strong>
    Hello world!
</strong>
```

This module will generate the following file:

```javascript
angular.module('test').directive('helloWorld', function () {
    return {
        restrict: 'E',
        template:'<strong>Hello world!</strong>'
    };
});
```

Using this example gulpfile:

```javascript
var gulp = require('gulp');
var injectTemplates = require('angular-inject-templates');

gulp.task('js:build', function () {
    gulp.src('src/scripts/**/*.js')
        .pipe(injectTemplates())
        .pipe(gulp.dest('./dist'));
});
```

## API

### injectTemplates(options)

#### options.minimize
Type: `Object`

settings to pass in minimize plugin. Please see all settings on [minimize official page](https://www.npmjs.com/package/minimize)

#### options.skipErrors
Type: `Boolean`
Default value: 'false'

default=false should plugin brake on errors (file not found, error in minification) or skip errors and go to next template

## License
This module is released under the MIT license.

