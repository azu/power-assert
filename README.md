power-assert
================================

[![Build Status](https://travis-ci.org/twada/power-assert.svg)](https://travis-ci.org/twada/power-assert)
[![NPM version](https://badge.fury.io/js/power-assert.svg)](http://badge.fury.io/js/power-assert)
[![Dependency Status](https://gemnasium.com/twada/power-assert.svg)](https://gemnasium.com/twada/power-assert)
[![Built with Grunt](https://cdn.gruntjs.com/builtwith.png)](http://gruntjs.com/)

Power Assert in JavaScript. Less code, more information.


DESCRIPTION
---------------------------------------

What is `power-assert`?

 * is an implementation of "Power Assert" concept in JavaScript.
 * provides descriptive assertion messages through standard [assert](http://nodejs.org/api/assert.html) compatible interface.
 * works both on server side and browser side.
 * available via [npm](https://www.npmjs.org/) and [bower](http://bower.io/). 
 * provides [browserify](http://browserify.org/) transform.
 * provides [grunt](http://gruntjs.com/) task and [gulp](http://gulpjs.com/) plugin.
 * is a beta version product. Pull-requests, issue reports and patches are always welcomed.


`power-assert` provides descriptive assertion messages for your tests, like this.

      1) Array #indexOf() should return index when the value is present:
         AssertionError: # /path/to/test/mocha_node.js:10
    
                assert(this.ary.indexOf(zero) === two);
                            |   |       |     |   |
                            |   |       |     |   2
                            |   -1      0     false
                            [1,2,3]


MODULES
---------------------------------------

`power-assert` family provides 9 modules. 5 core modules and 4 instrumentors.

core modules are,

| module | description |
|:-------|:------------|
| [power-assert](http://github.com/twada/power-assert) | Standard `assert` function on top of `empower` and `power-assert-formatter` |
| [empower](http://github.com/twada/empower) | Power Assert feature enhancer for assert function/object. |
| [power-assert-formatter](http://github.com/twada/power-assert-formatter) | Power Assert output formatter. |
| [espower](http://github.com/twada/espower) | Power Assert feature instrumentor core based on the [Mozilla JavaScript AST](https://developer.mozilla.org/en-US/docs/SpiderMonkey/Parser_API). |
| [espower-source](http://github.com/twada/espower-source) | Power Assert instrumentor from source to source (Thin wrapper of `espower`). |

and instrumentors are,

| module | description |
|:-------|:------------|
| [espower-loader](http://github.com/twada/espower-loader) | Node module loader to apply `espower` on the fly. |
| [espowerify](http://github.com/twada/espowerify) | [Browserify](http://browserify.org/) transform to apply `espower` to target files. |
| [grunt-espower](http://github.com/twada/grunt-espower) | Grunt task to apply `espower` to target files. |
| [gulp-espower](http://github.com/twada/gulp-espower) | Gulp plugin to apply `espower` to target files. |


`power-assert` provides standard [assert](http://nodejs.org/api/assert.html) compatible function with Power Assert feature.
(Best fit with [Mocha](http://visionmedia.github.io/mocha/). If you use assert-like objects provided by various testing frameworks such as [QUnit](http://qunitjs.com/) or [nodeunit](https://github.com/caolan/nodeunit). Please use [empower](http://github.com/twada/empower) and [power-assert-formatter](http://github.com/twada/power-assert-formatter) modules directly).


Internally, `power-assert` uses [empower](http://github.com/twada/empower) module to enhance power assert feature into the standard [assert](http://nodejs.org/api/assert.html) module, to run with the power assert feature added code by [espower](http://github.com/twada/espower) module, and prettify output using [power-assert-formatter](http://github.com/twada/power-assert-formatter).


See [power-assert-demo](http://github.com/twada/power-assert-demo) project for power-assert Demo running with mocha.



EXAMPLE
---------------------------------------

### Target test code (using Mocha in this example)

```javascript
var assert = require('power-assert');

describe('Array', function(){
    beforeEach(function(){
        this.ary = [1,2,3];
    });
    describe('#indexOf()', function(){
        it('should return index when the value is present', function(){
            var zero = 0, two = 2;
            assert(this.ary.indexOf(zero) === two);
        });
        it('should return -1 when the value is not present', function(){
            var minusOne = -1, two = 2;
            assert.ok(this.ary.indexOf(two) === minusOne, 'THIS IS AN ASSERTION MESSAGE');
        });
    });
});
```

### Apply one of power assert instrumentors to code above then run tests. See the power-assert output appears.


      $ mocha /path/to/espowered_examples/mocha_node.js
    
      Array
        #indexOf()
          1) should return index when the value is present
          2) should return -1 when the value is not present
    
    
      0 passing (7 ms)
      2 failing
    
      1) Array #indexOf() should return index when the value is present:
         AssertionError: # /path/to/test/mocha_node.js:10
    
                assert(this.ary.indexOf(zero) === two);
                            |   |       |     |   |
                            |   |       |     |   2
                            |   -1      0     false
                            [1,2,3]
    
          at /path/to/node_modules/empower/lib/empower.js:150:20
          at powerAssert (/path/to/node_modules/empower/lib/empower.js:99:17)
          at Context.<anonymous> (/path/to/test/mocha_node.js:13:13)
          at callFn (/path/to/node_modules/mocha/lib/runnable.js:223:21)
          at Test.Runnable.run (/path/to/node_modules/mocha/lib/runnable.js:216:7)
          at Runner.runTest (/path/to/node_modules/mocha/lib/runner.js:374:10)
          at /path/to/node_modules/mocha/lib/runner.js:452:12
          at next (/path/to/node_modules/mocha/lib/runner.js:299:14)
          at /path/to/node_modules/mocha/lib/runner.js:309:7
          at next (/path/to/node_modules/mocha/lib/runner.js:247:23)
          at Object._onImmediate (/path/to/node_modules/mocha/lib/runner.js:276:5)
          at processImmediate [as _immediateCallback] (timers.js:330:15)
    
      2) Array #indexOf() should return -1 when the value is not present:
         AssertionError: THIS IS AN ASSERTION MESSAGE # /path/to/test/mocha_node.js:14
    
                assert.ok(this.ary.indexOf(two) === minusOne, 'THIS IS AN ASSERTION MESSAGE');
                               |   |       |    |   |
                               |   |       |    |   -1
                               |   1       2    false
                               [1,2,3]
    
          at Function.ok (/path/to/node_modules/empower/lib/empower.js:150:20)
          at Context.<anonymous> (/path/to/test/mocha_node.js:48:20)
          at callFn (/path/to/node_modules/mocha/lib/runnable.js:223:21)
          at Test.Runnable.run (/path/to/node_modules/mocha/lib/runnable.js:216:7)
          at Runner.runTest (/path/to/node_modules/mocha/lib/runner.js:374:10)
          at /path/to/node_modules/mocha/lib/runner.js:452:12
          at next (/path/to/node_modules/mocha/lib/runner.js:299:14)
          at /path/to/node_modules/mocha/lib/runner.js:309:7
          at next (/path/to/node_modules/mocha/lib/runner.js:247:23)
          at Object._onImmediate (/path/to/node_modules/mocha/lib/runner.js:276:5)
          at processImmediate [as _immediateCallback] (timers.js:330:15)



HOW TO USE
---------------------------------------

There are three ways to use power-assert.

1. `power-assert` + `espower-loader`: Highly recommended but only works under Node.
2. `power-assert` + `espowerify` : Recommended if you are using [browserify](http://browserify.org/).
3. `power-assert` + `grunt-espower` or `gulp-espower` : Generate instrumented code so works anywhere.



### using `espower-loader`

If you are writing Node.js app/module, you can instrument Power Assert feature without code generation by using `espower-loader`.

First, install `power-assert` and `espower-loader` via npm.

    $ npm install --save-dev power-assert espower-loader

Second, require `power-assert` in your test.

    --- a/test/your_test.js
    +++ b/test/your_test.js
    @@ -1,4 +1,4 @@
    -var assert = require('assert');
    +var assert = require('power-assert');


Third, put `enable-power-assert.js` somewhere in your project, where `pattern` matches to target test files.

```javascript
require('espower-loader')({
    // directory where match starts with
    cwd: process.cwd(),
    // glob pattern using minimatch module
    pattern: 'test/**/*.js'
});
```

Then run mocha, with `--require` option. No code generation required.

    $ mocha --require ./path/to/enable-power-assert test/your_test.js


FYI: You may be interested in [intelli-espower-loader](https://github.com/azu/intelli-espower-loader) to go one step further. With [intelli-espower-loader](https://github.com/azu/intelli-espower-loader), you don't need to create loader file (like `enable-power-assert.js`). Just define test directory in `package.json` wow!



### using `espowerify`

On the browser side and you are using [browserify](http://browserify.org/), you can instrument Power Assert feature via `espowerify`.

First, install `power-assert` and `espowerify` via npm.

    $ npm install --save-dev power-assert espowerify

Second, require `power-assert` in your test.

    --- a/test/your_test.js
    +++ b/test/your_test.js
    @@ -1,4 +1,4 @@
    -var assert = require('assert');
    +var assert = require('power-assert');

Third, apply `espowerify` through browserify transform.

    $ browserify -t espowerify test/your_test.js > dist/your_test.js

Lastly, run your test in your way. For example,

    $ mocha-phantomjs path/to/test.html



### using `grunt-espower`

On the browser side and you are not using [browserify](http://browserify.org/) but [bower](http://bower.io/) and [Grunt](http://gruntjs.com/), you can use `power-assert` via bower, with generated code by `grunt-espower`
(If you prefer more small steps, [espower runner](https://gist.github.com/azu/6309397) and [its variation for Windows](https://gist.github.com/gooocho/6317135) may be useful to start with.)

First, install `power-assert` via bower and `grunt-espower` via npm. This means that you run grunt (on Node), then run tests on browser.

    $ bower install --save-dev power-assert
    $ npm install --save-dev grunt-espower

Second, require `power-assert` family in your test html.

    <script type="text/javascript" src="./path/to/bower_components/assert/assert.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/empower/lib/empower.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/power-assert-formatter/lib/power-assert-formatter.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/power-assert/lib/power-assert.js"></script>

Third, configure `grunt-espower` task to  generate espowered code.

```javascript
grunt.initConfig({

  . . . 

  espower: {
    test: {
      files: [
        {
          expand: true,        // Enable dynamic expansion.
          cwd: 'test/',        // Src matches are relative to this path.
          src: ['**/*.js'],    // Actual pattern(s) to match.
          dest: 'espowered/',  // Destination path prefix.
          ext: '.js'           // Dest filepaths will have this extension.
        }
      ]
    },
  },

  . . . 

})
```

Then, generate espowered code using `espower` task.

    $ grunt espower:test

Lastly, run your test in your way. For example,

    $ grunt test

or

    $ mocha your_test_espowered.js



### using `gulp-espower`

On the browser side and you are not using [browserify](http://browserify.org/) but [bower](http://bower.io/) and [gulp](http://gulpjs.com/), you can use `power-assert` via bower, with generated code by `gulp-espower`
(If you prefer more small steps, [espower runner](https://gist.github.com/azu/6309397) and [its variation for Windows](https://gist.github.com/gooocho/6317135) may be useful to start with.)

First, install `power-assert` via bower and `gulp-espower` via npm. This means that you run gulp (on Node), then run tests on browser.

    $ bower install --save-dev power-assert
    $ npm install --save-dev gulp-espower

Second, require `power-assert` family in your test html.

    <script type="text/javascript" src="./path/to/bower_components/assert/assert.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/empower/lib/empower.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/power-assert-formatter/lib/power-assert-formatter.js"></script>
    <script type="text/javascript" src="./path/to/bower_components/power-assert/lib/power-assert.js"></script>

Third, configure `gulp-espower` task to generate espowered code.

```javascript
var gulp = require('gulp'),
    espower = require('gulp-espower');
  . . . 
gulp.task('espower', function() {
    gulp.src('test/**/*_test.js')
        .pipe(espower())
        .pipe(gulp.dest('espowered'));
});
  . . . 
})
```

Then, generate espowered code using `espower` task.

    $ gulp espower

Lastly, run your test in your way. For example,

    $ gulp test

or

    $ mocha your_test_espowered.js



TESTED FRAMEWORKS
---------------------------------------
* [Mocha](http://visionmedia.github.io/mocha/)
* [QUnit](http://qunitjs.com/)
* [nodeunit](https://github.com/caolan/nodeunit)
* [buster-assertions](http://docs.busterjs.org/en/latest/modules/buster-assertions/)


TESTED ENVIRONMENTS
---------------------------------------
* [Node.js](http://nodejs.org/)
* [Rhino](https://developer.mozilla.org/en/Rhino)
* [PhantomJS](http://phantomjs.org/)
* [RequireJS](http://requirejs.org/)
* [Browserify](http://browserify.org/)


AUTHOR
---------------------------------------
* [Takuto Wada](http://github.com/twada)


LICENSE
---------------------------------------
Licensed under the [MIT](https://raw.github.com/twada/power-assert/master/MIT-LICENSE.txt) license.



MORE OUTPUT EXAMPLES
---------------------------------------

### Target test code (using QUnit in this example)

```javascript
var q = require('qunitjs');

(function () {
    var empower = require('empower'),
        formatter = require('power-assert-formatter'),
        qunitTap = require("qunit-tap");
    empower(q.assert, formatter(), {destructive: true});
    qunitTap(q, require('util').puts, {showSourceOnFailure: false});
    q.config.autorun = false;
})();

q.test('spike', function (assert) {
    assert.ok(true);

    var hoge = 'foo';
    var fuga = 'bar';
    assert.ok(hoge === fuga, 'comment');

    var piyo = 3;
    assert.ok(fuga === piyo);

    var longString = 'very very loooooooooooooooooooooooooooooooooooooooooooooooooooong message';
    var anotherLongString = 'yet another loooooooooooooooooooooooooooooooooooooooooooooooooooong message';
    assert.ok(longString === anotherLongString);

    assert.ok(4 === piyo);

    assert.ok(4 !== 4);

    var falsyStr = '';
    assert.ok(falsyStr);

    var falsyNum = 0;
    assert.ok(falsyNum);

    var ary1 = ['foo', 'bar'];
    var ary2 = ['aaa', 'bbb', 'ccc'];
    assert.ok(ary1.length === ary2.length);
    assert.deepEqual(ary1, ary2);

    var actual = 16;
    assert.ok(5 < actual && actual < 13);

    actual = 4;
    assert.ok(5 < actual && actual < 13);

    actual = 10;
    assert.ok(actual < 5 || 13 < actual);


    var propName = 'bar',
        foo = {
            bar: {
                baz: false
            }
        };

    assert.ok(foo.bar.baz);
    assert.ok(foo['bar'].baz);
    assert.ok(foo[propName]['baz']);


    var truth = true;
    assert.ok(!truth);


    var func = function () { return false; };
    assert.ok(func());


    var obj = {
        age: function () {
            return 0;
        }
    };
    assert.ok(obj.age());


    var isFalsy = function (arg) {
        return !(arg);
    };
    var positiveInt = 50;
    assert.ok(isFalsy(positiveInt));


    var sum = function () {
        var result = 0;
        for (var i = 0; i < arguments.length; i += 1) {
            result += arguments[i];
        }
        return result;
    };
    var one = 1, two = 2, three = 3, seven = 7, ten = 10;
    assert.ok(sum(one, two, three) === seven);
    assert.ok(sum(sum(one, two), three) === sum(sum(two, three), seven));
    assert.ok((three * (seven * ten)) === three);


    var math = {
        calc: {
            sum: function () {
                var result = 0;
                for (var i = 0; i < arguments.length; i += 1) {
                    result += arguments[i];
                }
                return result;
            }
        }
    };
    assert.ok(math.calc.sum(one, two, three) === seven);
});

q.load();
```


### `espower` code above then running under Node.js
    
    # test: spike
    ok 1
    not ok 2 - comment # /path/to/examples/qunit_node.js:17
    #
    #     assert.ok(hoge === fuga, 'comment');
    #               |    |   |
    #               |    |   "bar"
    #               |    false
    #               "foo"
    # , test: spike
    not ok 3 - # /path/to/examples/qunit_node.js:20
    #
    #     assert.ok(fuga === piyo);
    #               |    |   |
    #               |    |   3
    #               |    false
    #               "bar"
    # , test: spike
    not ok 4 - # /path/to/examples/qunit_node.js:24
    #
    #     assert.ok(longString === anotherLongString);
    #               |          |   |
    #               |          |   "yet another loooooooooooooooooooooooooooooooooooooooooooooooooooong message"
    #               |          false
    #               "very very loooooooooooooooooooooooooooooooooooooooooooooooooooong message"
    # , test: spike
    not ok 5 - # /path/to/examples/qunit_node.js:26
    #
    #     assert.ok(4 === piyo);
    #                 |   |
    #                 |   3
    #                 false
    # , test: spike
    not ok 6 - # /path/to/examples/qunit_node.js:28
    #
    #     assert.ok(4 !== 4);
    #                 |
    #                 false
    # , test: spike
    not ok 7 - # /path/to/examples/qunit_node.js:31
    #
    #     assert.ok(falsyStr);
    #               |
    #               ""
    # , test: spike
    not ok 8 - # /path/to/examples/qunit_node.js:34
    #
    #     assert.ok(falsyNum);
    #               |
    #               0
    # , test: spike
    not ok 9 - # /path/to/examples/qunit_node.js:38
    #
    #     assert.ok(ary1.length === ary2.length);
    #               |    |      |   |    |
    #               |    |      |   |    3
    #               |    |      |   ["aaa","bbb","ccc"]
    #               |    2      false
    #               ["foo","bar"]
    # , test: spike
    not ok 10 - # /path/to/examples/qunit_node.js:39
    #
    #     assert.deepEqual(ary1, ary2);
    #                      |     |
    #                      |     ["aaa","bbb","ccc"]
    #                      ["foo","bar"]
    # , expected: [
    #   "aaa",
    #   "bbb",
    #   "ccc"
    # ], got: [
    #   "foo",
    #   "bar"
    # ], test: spike
    not ok 11 - # /path/to/examples/qunit_node.js:42
    #
    #     assert.ok(5 < actual && actual < 13);
    #                 | |      |  |      |
    #                 | |      |  16     false
    #                 | 16     false
    #                 true
    # , test: spike
    not ok 12 - # /path/to/examples/qunit_node.js:45
    #
    #     assert.ok(5 < actual && actual < 13);
    #                 | |      |
    #                 | 4      false
    #                 false
    # , test: spike
    not ok 13 - # /path/to/examples/qunit_node.js:48
    #
    #     assert.ok(actual < 5 || 13 < actual);
    #               |      |   |     | |
    #               |      |   |     | 10
    #               |      |   false false
    #               10     false
    # , test: spike
    not ok 14 - # /path/to/examples/qunit_node.js:58
    #
    #     assert.ok(foo.bar.baz);
    #               |   |   |
    #               |   |   false
    #               |   {"baz":false}
    #               {"bar":{"baz":false}}
    # , test: spike
    not ok 15 - # /path/to/examples/qunit_node.js:59
    #
    #     assert.ok(foo['bar'].baz);
    #               |  |       |
    #               |  |       false
    #               |  {"baz":false}
    #               {"bar":{"baz":false}}
    # , test: spike
    not ok 16 - # /path/to/examples/qunit_node.js:60
    #
    #     assert.ok(foo[propName]['baz']);
    #               |  ||        |
    #               |  |"bar"    false
    #               |  {"baz":false}
    #               {"bar":{"baz":false}}
    # , test: spike
    not ok 17 - # /path/to/examples/qunit_node.js:64
    #
    #     assert.ok(!truth);
    #               ||
    #               |true
    #               false
    # , test: spike
    not ok 18 - # /path/to/examples/qunit_node.js:68
    #
    #     assert.ok(func());
    #               |
    #               false
    # , test: spike
    not ok 19 - # /path/to/examples/qunit_node.js:76
    #
    #     assert.ok(obj.age());
    #               |   |
    #               {}  0
    # , test: spike
    not ok 20 - # /path/to/examples/qunit_node.js:83
    #
    #     assert.ok(isFalsy(positiveInt));
    #               |       |
    #               false   50
    # , test: spike
    not ok 21 - # /path/to/examples/qunit_node.js:94
    #
    #     assert.ok(sum(one, two, three) === seven);
    #               |   |    |    |      |   |
    #               |   |    |    |      |   7
    #               6   1    2    3      false
    # , test: spike
    not ok 22 - # /path/to/examples/qunit_node.js:95
    #
    #     assert.ok(sum(sum(one, two), three) === sum(sum(two, three), seven));
    #               |   |   |    |     |      |   |   |   |    |       |
    #               |   |   |    |     |      |   12  5   2    3       7
    #               6   3   1    2     3      false
    # , test: spike
    not ok 23 - # /path/to/examples/qunit_node.js:96
    #
    #     assert.ok((three * (seven * ten)) === three);
    #                |     |  |     | |     |   |
    #                |     |  |     | |     |   3
    #                |     |  |     | 10    false
    #                |     |  7     70
    #                3     210
    # , test: spike
    not ok 24 - # /path/to/examples/qunit_node.js:110
    #
    #     assert.ok(math.calc.sum(one, two, three) === seven);
    #               |    |    |   |    |    |      |   |
    #               |    |    |   |    |    |      |   7
    #               |    {}   6   1    2    3      false
    #               {"calc":{}}
    # , test: spike
    1..24


Have fun!
