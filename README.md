# Code styling guide

All code in any code-base should look like a single person typed it, no matter how many people contributed.

> "Arguments over style are pointless. There should be a style guide, and you  should follow it"
> Rebecca Murphey

## Credit

- https://github.com/rwaldron/idiomatic.js/
- https://github.com/bevacqua/js
- https://github.com/airbnb/javascript

## Conventions

### Strict mode

Always put `'use strict';` at the top of your file. Strict mode allows you to catch nonsensical behavior, discourages poor practices, and is faster because it allows compilers to make certain assumptions about your code.

### Whitespaces

#### Base rules:

- Use 2 spaces (soft tabs). Never use tabs.
- Line length must never exceed **80 characters**.
- File must end with a **single** new line.
- Trailing whitespaces must be trimmed.

#### Line spaces

Use a **single** space line between the module definition and its methods. Use a **single** space line between methods. Avoid using any space at all inside a method (if it is too long, maybe you should rethink what it does).

```javascript
module.exports = {

  getAllPages: function (request, reply) {
    Page.getAllPages(function (pages) {
      reply(pages);
    });
  },
  
  getPageById: function (request, reply) {
    Page.getPageById(request.params.pageId, function (page) {
      if (!page) {
        return reply('Page not found').status(404);
      }
      reply(page);
    });
  }

};
```

Set off operators with spaces.

```javascript
// bad
var x=y+5;

// good
var x = y + 5;
```

---

## Strings

### Quotes

Always use single quotes `'`. Do not use double quotes `"`.

```javascript
// bad
message = "Hello";
message = "Hello, I'm happy";
message = 'Hello ' + name + "!";

// good
message = 'Hello';
message = 'Hello, I\'m happy';
message = 'Hello ' + name + '!';
```

### Multiline strings

 If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

```javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// bad - start the string on a new line
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
var errorMessage = 
  'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
var errorMessage = [
  'This is a super long error that was thrown because ',
  'of Batman. When you stop to think about how Batman had anything to do ',
  'with this, you would get nowhere fast.'
].join('');
```

When programmatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

```javascript
// bad
function inbox(messages) {
  items = '<ul>';
  for (i = 0; i < length; i++) {
    items += '<li>' + messages[i].message + '</li>';
  }
  return items + '</ul>';
}

// good
function inbox(messages) {
  items = [];
  for (i = 0; i < length; i++) {
    items[i] = messages[i].message;
  }
  return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
}
```

---

## Comments

Comments aren't meant to explain what the code does. Good code is supposed to be self-explanatory. If you're thinking of writing a comment to explain what a piece of code does, chances are you need to change the code itself. The exception to that rule is explaining what a regular expression does. Good comments are supposed to explain why code does something that may not seem to have a clear-cut purpose.

**Bad**

```javascript
// create the centered container
var p = $('<p/>');
p.center(div);
p.text('foo');
```

**Good**

```javascript
// the megaphone periodically emits updates for container
megaphone.on('data', function (value) {
  container.text(value); 
});

// one or more digits somewhere in the string
var numeric = /\d+/;
```

Comments are positionned before the code that it comments, on its own line:

**Bad**

```javascript
var numeric = /\d+/; // one or more digits somewhere in the string
```

**Good**

```javascript
// one or more digits somewhere in the string
var numeric = /\d+/;
```

Commenting out entire blocks of code should be avoided entirely, that's why you have version control systems in place!

### Comments annotations

You can use comments to add annotations to the code. Use a COMMAND capitalized, followed with a colon `: ` and the description. May the comment be longer than 80 characters, break it on multiple lines and indent the subsequent lines with two spaces:

```javascript
// FIXME: This has crashed occasionally since v3.2.1. It may
//   be related to the library bla.
```

The authorized verbs are:

- `TODO`: note missing features or functionality that should be added later
- `FIXME`: note broken code that needs to be fixed
- `OPTIMIZE`: note slow or inefficient code that may cause performance problems
- `HACK`: note code smells where questionable coding practices were used and should be refactored away
- `REVIEW`: note anything that should be looked at to confirm it is working as intended

---

## Regex

Prefix regex variable names with a `r`:

```javascript
// bad
var numeric = /\d+/;

// good
var rNumeric = /\d+/;

```

Keep regular expressions in variables, don't use them inline. This will vastly improve readability.

```javascript
// bad
if (/\d+/.test(text)) {
  console.log('so many numbers!');
}

// good
var rNumeric = /\d+/;
if (rNumeric.test(text)) {
  console.log('so many numbers!');
}
```

---

## Variables

### Variables definition

One variable per line. Repeat the `var` keyword on every line. Align the `=` operators. 

```javascript
// bad - not repeating the var
var firstVariable = 1,
    secondVariable = 2;

// bad - not repeating the var
var firstVariable = 1
  , secondVariable = 2;

// bad - = operators not aligned
var firstVariable = 1;
var secondVariable = 2;

// good
var firstVariable  = 1;
var secondVariable = 2;

// acceptable as they are no assigned values
var firstVariable, secondVariable;
```

Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

```javascript
// bad
function() {
  console.log('doing stuff..');
  var name = getName();
  if (name === 'test') {
    return false;
  }
  return name;
}

// good
function() {
  var name = getName();
  console.log('doing stuff..');
  if (name === 'test') {
    return false;
  }
  return name;
}

// bad
function() {
  var name = getName();
  if (!arguments.length) {
    return false;
  }
  return true;
}

// good
function() {
  if (!arguments.length) {
    return false;
  }
  var name = getName();
  return true;
}
```

### Naming

Name variable in english:

```javascript
// bad
заплата = 1000;
salaire = 1000;

// good
salary = 1000;
```

Do not use obscure, abstract, abreviated, single letter variable names. Be explicit, clear and concise. Do not use the variable type in the name:

```javascript
// bad
var string;
var fct;
var d = 'ifficult to know what it is';
var dogArray = [];

// good
var dog = 'gy style';
var dogs = [];
```

### Case

- `camelCase`: Naming functions, methods, variables, objects, instances, etc
- `PascalCase`: Naming constructors, prototypes, etc
- `rDesc`: Naming regular expressions
- `SYMBOLIC_CONSTANTS`: Naming constant

### Type Casting & Coercion

Perform type coercion at the beginning of the statement.

**Strings:**

```javascript
//  => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

Use `parseInt` for Numbers and always with a radix for type casting.

```javascript
var inputValue = '4';

// bad
var val = new Number(inputValue);
var val = +inputValue;
var val = inputValue >> 0;
var val = parseInt(inputValue);

// good
var val = Number(inputValue);
var val = parseInt(inputValue, 10);
```

If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

```javascript
// parseInt was slow, so Bitshifting the String to coerce it to a Number
var val = inputValue >> 0;
```

Booleans:

```javascript
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);
var hasAge = !!age;
```

---

## Functions

### Function declaration

Anonymous:

```javascript
// bad
function (a,b) {}
function ( a,b ) {}
function( a, b ){}
function(a, b) {}
function(a,b) {}

// good
function (a, b) {
  return a + b;
}
```

Named:

```javascript
// bad
function sum( a, b ){}
function sum (a, b) {}
function sum(a,b) {}

// good
function sum(a, b) {
  return a + b;
}
```

Immediately-invoked function expression (IIFE):

```javascript
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

Whenever a method is non-trivial, make the effort to use a named function declaration rather than an anonymous function. This will make it easier to pinpoint the root cause of an exception when analyzing stack traces.

```javascript
// bad
function once (fn) {
  var ran = false;
  return function () {
    if (ran) { return };
    ran = true;
    fn.apply(this, arguments);
  };
}

// good
function once (fn) {
  var ran = false;
  return function run () {
    if (ran) { return };
    ran = true;
    fn.apply(this, arguments);
  };
}
```

Do not define single line functions, let's be consistent. And whitespace is your friend:

```javascript
// bad
function sum(a, b) { return a + b; }

// good
function sum(a, b) {
  return a + b;
}
```

When declaring a function, always use the function declaration form instead of function expressions. Because [hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html).

```javascript
// bad
var sum = function (a, b) {
  return a + b;
}

// bad - variable name will get hoisted, not the assignment
var sum = function sum(a, b) {
  return a + b;
}

// good
function sum(a, b) {
  return a + b;
}
```

Don't declare function inside conditionals and loops (`while`, `for`...).

```javascript
// bad
if (Math.random() > 0.5) {
  sum(1, 3);

  function sum (x, y) {
    return x + y;
  }
}

// good
function sum (x, y) {
  return x + y;
}

if (Math.random() > 0.5) {
  sum(1, 3);
}
```

Return fast. Avoid keeping indentation levels from raising more than necessary by using guard clauses instead of flowing if statements.

```javascript
// bad
function returnLate(foo) {
  var value;
  if (foo) {
    value = 'foo';
  }
  else {
    value = 'bar';
  }
  return value;
}

// good
function returnEarly(foo) {
  if (foo) {
    return 'foo';
  }
  return 'bar';
}
```

Never name a parameter arguments, this will take precedence over the arguments object that is given to every function scope.

```javascript
// bad
function nope(name, options, arguments) {}

// good
function yup(name, options, args) {}
```

Use `||`` to define a default value. If the left-hand value is falsy then the right-hand value will be used:

```javascript
function doSomething(value) {
  value = value || 33;
}
```

Though do not use this for booleans:

```javascript
// bad - would set true even if value is set to false
function doSomething(value) {
  value = value || true;
}

// good - check if value is either null or undefined
function doSomething(value) {
  value = (value == null) ? true : value;
}
```

### Function call

```javascript
// bad
doSomething(arg1,arg2,arg3);
doSomething( arg1 , arg2 , arg3 );
doSomething( arg1, arg2, arg3 );

// good
doSomething(arg1, arg2, arg3);
```

You can chain multiple functions on a single line. If it gets too long, break it with a single method call per line. The chaining operator `.` must then be at the beginning of every subsequent method call.

```javascript
// bad - trailing doc should be on the next line
do('foo').
  then('bar').
  andAfter('baz');

// bad - two methods call on a single line
do('foo').then('bar')
  .andAfter('baz');

// good
do('foo').then('bar').andAfter('baz');

// good
do('foo')
  .then('bar')
  .andAfter('baz');
```

You can use indentation to bring meaning to the chain:

```javascript
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();
```

### On `this`

Beyond the generally well known use cases of call and apply, always prefer .bind( this ) or a functional equivalent, for creating BoundFunction definitions for later invocation. Only resort to aliasing when no preferable option is available.

```javascript
// bad if it can be avoided
function Device( opts ) {
  var self = this;
  this.value = null;
  stream.read(opts.path, function(data) {
    self.value = data;
  });
}

// good
function Device( opts ) {
  this.value = null;

  stream.read(opts.path, function(data) {
    this.value = data;
  }.bind(this));
}
```

If you need to alias `this`, use `_this`:

```javascript
// bad
function() {
  var self = this;
  return function() {
    console.log(self);
  };
}

// bad
function() {
  var that = this;
  return function() {
    console.log(that);
  };
}

// good
function() {
  var _this = this;
  return function() {
    console.log(_this);
  };
}
```

If `.bind()` isn't available, rely on a library one:

```javascript
_.bind()
jQuery.proxy()
// ...
```

Several prototype methods of ES 5.1 built-ins come with a special `thisArg` signature, which should be used whenever possible

```javascript
var obj = { f: "foo", b: "bar", q: "qux" };

Object.keys( obj ).forEach(function( key ) {
  console.log( this[ key ] ); // <-- |this| now refers to `obj`
}, obj ); // <-- the last arg is `thisArg`
```

`thisArg` can be used with `Array.prototype.every`, `Array.prototype.forEach`, `Array.prototype.some`, `Array.prototype.map`, `Array.prototype.filter`.

---

## Conditionals

Same spacing rules as for function declaration apply:

```javascript
// bad
if(condition) {}
if( condition ){}
if(condition) {}
if (condition) {}

// good
if (condition) {}
```

Brackets are enforced. Never use single line conditions. Subsequent `else` and `else if` must be on a new line:

```javascript
// bad - no brackets
if(condition) doSomething();

// bad - single line condition
if(condition) { doSomething(); }

// bad - else/else if must go on a new line
if (condition) {
  doSomething();
} else if (condition2) {
  doSomethingDifferent();
} else {
  doSomethingElse();
}

// good
if (condition1) {
  doSomething();
}
else if (condition2) {
  doSomethingDifferent();
}
else {
  doSomethingElse();
}
```

### Ternary operators

Ternary operators are fine for clear-cut conditionals, but unacceptable for confusing choices. As a rule, if you can't eye-parse it as fast as your brain can interpret the text that declares the ternary operator, chances are it's probably too complicated for its own good. Never ever nest ternary operators.

```javascript
// bad
return a && b ? 11 : a ? 10 : 0;

// good
mobile ? mobile.name : 'Generic Player';

// good
if (a && b) {
  return 11;
}
else if (a) {
  return 10;
}
else {
  return 0;
}
```

### Equality

Avoid using `==` and `!=` operators, always favor `===` and `!==`. These operators are called the "strict equality operators", while their counterparts will attempt to cast the operands into the same value type.

```javascript
// bad
dog == 'gy style'
dog != 'gy style'

// good
dog === 'gy style'
dog !== 'gy style'
```

Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

+ **Objects** evaluate to **true**
+ **Undefined** evaluates to **false**
+ **Null** evaluates to **false**
+ **Booleans** evaluate to **the value of the boolean**
+ **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
+ **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

```javascript
if ([0]) {
  // true: an array is an object, objects evaluate to true
}
```

You can rely on truthy and falsy value for test. Use those shortcuts:

```javascript
// when evaluating that an array has length or not
// good
if (array.length > 0) {}
if (array.length === 0) {}

// but prefer
if (array.length) {}
if (!array.length) {}

// When only evaluating that a string is empty or not
// good
if (string !== "") {}
if (string === "") {}

// but prefer
if (string) {}
if (!string) {}

// When only evaluating that a reference is true or false
// good - and the only right solution to test for boolean
if (foo === true) {}
if (foo === false) {}

// but prefer
if (foo) {}
if (!foo) {}

// When only evaluating a ref that might be null or undefined, but NOT false, // or 0,
// good
if (foo === null || foo === undefined) {}

// but prefer
if (foo == null) {}
```

---

## Arrays

Use the litteral syntax for array creation unless you want to define a fixed size array:

```javascript
// bad
var dogs = new Array();

// good
var dogs = new Array(4);
var dogs = [];
```

Do not leave the trailing comma on the last item. If the array is too long, split it on multiple line, on item per line, indented with 2 spaces:

```javascript
// bad - should be on multiple lines
var people = [{name: 'John'}, {name: 'Arthur'}];

// bad - trailing comma
var people = [
  {name: 'John'}, 
  {name: 'Arthur'},
];

// bad - one item per line and poor indentation
var people = [{
  firstName: 'John',
  lastName: 'Doe'
}, 
{
  firstName: 'Arthur',
  lastName: 'Doe'
}];

// good
var people = [
  {name: 'John'}, 
  {name: 'Arthur'}
];

// good
var people = [
  {
    firstName: 'John',
    lastName: 'Doe'
  }, 
  {
    firstName: 'Arthur',
    lastName: 'Doe'
  }
];

```

If you don't know array length use `Array#push`:

```javascript
var someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

When you need to copy an array use `Array#slice`:

```javascript
// bad
var len       = items.length;
var itemsCopy = [];
var i;
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```

---

## Hashes

Remove trailing comma at the end of the hash. Avoid declaring single line hashes if it has more than one property. If you do, use spaces after the opening `{` and before the closing `}`:

```javascript
// bad
var people = {firstName: 'John'};
var people = { firstName: 'John', lastName: 'Doe' };
var people = {
  firstName: 'John',
  lastName: 'Doe',
};

// ok 
var people = { firstName: 'John' };
var people = {
  firstName: 'John',
  lastName: 'Doe'
};
```

### Accessing properties

Use dot notation when accessing properties.

```javascript
var luke = {
  jedi: true,
  age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```

Use subscript notation `[]` when accessing properties with a variable.

```javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```

---

## Loops

```javascript
// bad - inconsistent
while(condition) iterating++;
for(var i=0;i<100;i++) someIterativeFn();

// good
while (condition) {
  iterating++;
}
for(var i=0; i<100; i++) {
  someIterativeFn();
}

```


---

## Tools

### Linter and style checking

TODO

### EditorConfig

You are highly encouraged to install [EditorConfig](http://editorconfig.org/) in your editor of choice. It will take care of properly configuring your editor for the project you are working on.

### Sublime configuration

Set your editor to display a line at 80 characters:

```json
// add to your config file
"rulers": [80]
```
