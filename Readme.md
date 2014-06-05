# JavaScript Style Guide

This is a guide for writing consistent and aesthetically pleasing node.js code.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

This guide was created by [Felix Geisendörfer](http://felixge.de/), edited by
[Michał Weiser](https://plus.google.com/+MichałWeiser/) and is
licensed under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## 4 Spaces for indention

Use 4 spaces for indenting your code and swear an oath to never mix tabs and
spaces - a special kind of hell is awaiting you otherwise.

## Newlines

Use UNIX-style newlines (`\n`), and a newline character as the last character
of a file. Windows-style newlines (`\r\n`) are forbidden inside any repository.
Always leave one new line at the end of any file.

## No trailing whitespace

Just like you brush your teeth after every meal, you clean up any trailing
whitespace in your JS files before committing. Otherwise the rotten smell of
careless neglect will eventually drive away contributors and/or co-workers.

## No trailing comma in objects

Trailing comma at the end of object is problematic in some IE versions. Always
remember not to write it after last object element. This small thing will save
you a lot of future headaches caused by strange errors. 

*Right:*

```js
obj = {
    a: "a",
    b: "b"
};
```

*Wrong:*

```js
obj = {
    a: "a",
    b: "b",
};
```

## Use Semicolons

According to [scientific research][hnsemicolons], the usage of semicolons is
a core value of our community. Consider the points of [the opposition][], but
be a traditionalist when it comes to abusing error correction mechanisms for
cheap syntactic pleasures.

[the opposition]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hnsemicolons]: http://news.ycombinator.com/item?id=1547647

## 80 characters per line

Limit your lines to 80 characters. Yes, screens have gotten much bigger over the
last few years, but your brain has not. Use the additional room for split screen,
your editor supports that, right?

## Use single quotes

Use single quotes, unless you are writing JSON.

*Right:*

```js
foo = 'bar';
```

*Wrong:*

```js
foo = "bar";
```

## Opening braces go on the same line

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
    window.console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
    window.console.log('losing');
}
```

Also, notice the use of whitespace before and after the condition statement.

## Declare one variable per var statement

Declare one variable per var statement to follow the hoisting in JS.
The best way according to my personal experience is to declare one
variable at the top of function scope (C/C++ like style) without any
initializations to prevent mixed line ordering. And afterwards initialize
values wherever you want. This rule generates more code, but you should
never end up with strange hoisting problems.

*Right:*

```js
var keys, values, object, key;

keys   = [
    'foo',
    'bar'
];

values = [
    23,
    42
];

object = {};
while (keys.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
var keys = ['foo', 'bar'],
    values = [23, 42];

var object = {};

while (keys.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

## Naming conventions

### Use lower_case_underscore convention for variables and properties

```js
my_string = 'hello';
b = {
    my_prop: 10
};
```

### Use $ prefix in case of jQuery object variable
```js
$input_box = jQuery('input.box');
```
### Use lowerCamelCase for functions and methods
```js
myFunction = function () {
    return 'function';
};

obj = {
    myMethod: function () {
        return 'method';
    }
};
```
### Use UpperCamelCase for classes
```js
MyClass = function () {
    //constructor
};

a = new MyClass();
```
## Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties,
using all uppercase letters.

*Right:*

```js
SECOND = 1 * 1000;

File = function () {

};

File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
second = 1 * 1000;

function File() {

};

File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Object / Array creation

Never use trailing commas! Always one line for every item. Try to avoid to use
JavaScript reserved words as keys for object properties. If you need to use any
of them for some reason, remember to *always* quote them. Try to avoid usage of
space splited words as object property keys.

JS reserved words based on [ECMA specifications][ecmadocs]:
break, case, catch, continue, debugger, default, delete, do, else, finally, for,
function, if, in, instanceof, new, return, switch, this, throw, try, typeof, var,
void, while, with, class, const, enum, export, extends, import, super, implements,
interface, let, package, private, protected, public, static, yield

*Right:*

```js
a = [
    'hello',
    'world'
];

b = {
    good: 'code',
    'is_generally': 'pretty',
};
```

*Wrong:*

```js
a = ['hello', 'world',];
b = {"good": 'code'
    , is generally: 'pretty'
    };
```

[ecmadocs]: http://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-262%205th%20edition%20December%202009.pdf

## Use the === operator

Programming is not about remembering [stupid rules][comparisonoperators]. Use
the triple equality operator as it will work just as expected. If you decide to
use double equality operator always leave comment why you're doing it.

*Right:*

```js
a = 0;

if (a !== '') {
    window.console.log('winning');
}

```

*Wrong:*

```js
if (a == '') {
    window.console.log('losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

## Do not extend built-in prototypes

Do not extend the prototype of native JavaScript objects. Your future self will
be forever grateful. The one and only reason to extend native objects is to provide
missing functionality in older browsers ([polyfills][polyfillswiki]). 

*Right:*

```js
a = [];

if (!a.length) {
    window.console.log('winning');
}
```

*Wrong:*

```js
Array.prototype.empty = function() {
    return !this.length;
}

a = [];

if (a.empty()) {
    window.console.log('losing');
}
```

[polyfillswiki]: http://en.wikipedia.org/wiki/Polyfill

## Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

*Right:*

```js
isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
    window.console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
    window.console.log('losing');
}
```

## Write small functions

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

## Return early from functions

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

*Right:*

```js
function isPercentage(val) {
    if (val < 0) {
        return false;
    }

    if (val > 100) {
        return false;
    }

    return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
    if (val >= 0) {
        if (val < 100) {
            return true;
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
function isPercentage(val) {
    var isInRange;

    isInRange = (val >= 0 && val <= 100);
    return isInRange;
}
```

## Name your closures

Feel free to give your closures a name. It shows that you care about them, and
will produce better stack traces, heap and cpu profiles. Allowes you to simply
grep your stack trace to find named function.

*Right:*

```js
req.on('end', function onEnd() {
    window.console.log('winning');
});
```

*Wrong:*

```js
req.on('end', function() {
    window.console.log('losing');
});
```

## No nested closures

Use closures, but don't nest them. Otherwise your code will become a mess.

*Right:*

```js
setTimeout(function() {
    client.connect(afterConnect);
}, 1000);

function afterConnect() {
    window.console.log('winning');
}
```

*Wrong:*

```js
setTimeout(function() {
    client.connect(function() {
        window.console.log('losing');
    });
}, 1000);
```

## Use JSDoc comment style

Use [JSDoc][usejsdoc] syntax to be able to generate your documentation.
Try to write comments that explain higher level mechanisms or clarify
difficult segments of your code. Don't use comments to restate trivial
things.

*Right:*

```js
/**
 * 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
 * @type {Array}
 */
matches = item.match(/ID_([^\n]+)=([^\n]+)/));

/**
 * This function has a nasty side effect where a failure to increment a
 * redis counter used for statistics will cause an exception. This needs
 * to be fixed in a later iteration.
 *
 * @param {String}    id    user id
 * @param {Function}  cb    custom callback function
 * @return {Object}   loaded user object
 */
loadUser = function (id, cb) {
    // ...
}

isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
    // ...
}
```

*Wrong:*

```js
// Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
    // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
    // ...
}
```
[usejsdoc]: http://usejsdoc.org/

## Object.freeze, Object.preventExtensions, Object.seal, with, eval

Crazy shit that you will probably never need. Stay away from it.
