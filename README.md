# gulp-uglifyjs [![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url]

> Minify multiple JavaScript with UglifyJS2.

This plugin is based off of [gulp-uglify](https://github.com/terinjokes/gulp-uglify)
but allows you to directly use all of Uglify's options. The main difference
between these two plugins is that passing in an array of files (or glob pattern
that matches multiple files) will pass that array directly to Uglify. Uglify
will handle both concatenating and minifying those files.

Letting Uglify handle the concatenation and minification allows for the
generated source map to be able to separate the files in the browser for easier
debugging, and it avoids the intermediate step of `concat()`. Using `concat()`
and then `uglify()` would result in a source map for the already concatenated
file which has the possibility of being quite large, making it hard to find the
code you are wanting to debug.

## Usage

```javascript
var uglify = require('gulp-uglifyjs');

gulp.task('uglify', function() {
  gulp.src('public/js/*.js')
    .pipe(uglify())
    .pipe(gulp.dest('dist/js'))
});
```

This will concatenate and minify all files found by `public/js/*.js` and write
it to a file with the same name as the first file found.

## API

`uglify([filename], [options])`

- filename - optional

  Override the default output filename.

- options - optional

  These options are directly passed to Uglify. Below are some examples of what
  is available. To see a full list, read UglifyJS's [documentation](https://github.com/mishoo/UglifyJS2/#api-reference)

  - `outSourceMap`

    (default `false`) Give a `string` to be the name of the source map. Set to
    `true` and the source map name will be the same as the output file name
    suffixed with `.map`.

    Note: if you would like your source map to point to relative paths instead
    of absolute paths (which you probably will if it is hosted on a server), set
    the `basePath` option to be the base of where the content is served.

  - `mangle`

    (default `true`) Set to `false` to skip mangling names

  - `output`

    (default `null`) Pass an Object if you wish to specify additional [output options](http://lisperator.net/uglifyjs/codegen).
    The defaults are optimized for best compression.

  - `compress`

    (default `{}`) Pass an Object to specify custom [compressor options](http://lisperator.net/uglifyjs/compress).
    Pass `false` to skip compression completely.

  - `enclose`

    (default `undefined`) Pass an Object to wrap all of the code in a closure
    with a configurable arguments/parameters list. Each key-value pair in the
    enclose object is effectively an argument-parameter pair.

  - 'wrap'

    (default `undefined`) Pass a String to wrap all of the code in a closure.
    This is an easy way to make sure nothing is leaking. Variables that need to
    be public `exports` and `global` are made available. The value of wrap is
    the global variable exports will be available as.

  - `exportAll`

    (default `false`) Set to `true` to ake all global functions and variables
    available via the `export` variable. Only available when using `wrap`.

### Examples

To minify multiple files into a 'app.min.js' and create a source map:

```js
gulp.task('uglify', function() {
  gulp.src('public/js/*.js')
    .pipe(uglify('app.min.js', {
      outSourceMap: true
    }))
    .pipe(gulp.dest('dist/js'))
});
```

To minify multiple files from different paths to 'app.js', but skip mangling
and beautify the results:

```js
gulp.task('uglify', function() {
  gulp.src(['public/js/*.js', 'bower_components/**/*.js'])
    .pipe(uglify('app.js', {
      mangle: false,
      output: {
        beautify: true
      }
    }))
    .pipe(gulp.dest('dist/js'))
});
```

[npm-image]: http://img.shields.io/npm/v/gulp-uglifyjs.svg
[npm-url]: https://www.npmjs.org/package/gulp-uglifyjs/

[downloads-image]: http://img.shields.io/npm/dm/gulp-uglifyjs.svg

[travis-url]: https://travis-ci.org/craigjennings11/gulp-uglifyjs
[travis-image]: https://travis-ci.org/craigjennings11/gulp-uglifyjs.svg?branch=master