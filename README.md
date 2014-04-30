The FGL JavaScript Style Guide
==============================

---

The FGL JavaScript Style Guide is largely based on practices from previously published JavaScript Style Guides with some of our own experience thrown in. In a language like JavaScript that does not do a lot to enforce particular coding conventions it is important to define and stick to a set of guidelines to ensure that code is performant, readable and easy to maintain across development cycles. Our style guide is intended to make it simple to achieve this by giving an example-driven approach to coding style in JavaScript.

For anything that is not mentioned specifically in the style guide, refer to (in order of priority) the [Google JavaScript Style Guide], [Airbnb JavaScript Style Guide] and other code in our codebase. Most importantly, if it looks like a messy hack-job **it probably is**.

---

##Table of Contents

0. [General](#general)
0. [Naming](#naming)
0. [Variables](#variables)
0. [Objects and Arrays](#objects-and-arrays)
0. [Functions and Classes](#functions-and-classes)
0. [Whitespace](#whitespace)
0. [Syntax](#syntax)
0. [Commenting](#commenting)
0. [Features to Avoid](#features-to-avoid)

---

## General

  - Always wrap a set of functions or objects in an Immediately Invoked Function Expression (IIFE) or a Class (usually this means wrapping the whole js file in an IIFE to [define scope and prevent other bugs](http://blog.coolaj86.com/articles/how-and-why-auto-executing-function.html))
  
  ```javascript
  (function() {
    // All code goes here
  }());
  ```

  - Always use strict mode.
  
  ```javascript
  (function() {
    "use strict";

    function testStrict() {
      var x =  3.14; // This does not cause an error
    }
    x = 3.14; // This causes an error
  }());
  ```

**[§ back to contents](#table-of-contents)**

---

## Naming

Indentifier names should follow these conventions:

  - variables should use **camelCaseNaming**

  ```javascript
  var myVariableIsCool = 0;
  ```

  - functions should be **camelCaseFunctions()**

  ```javascript
  var myFunkyTion = function () {
    // Does funky stuff
  };
  ```
  
  - classes should have **CapitalizedFirstLetters**.

  ```javascript
  function ThisIsAClass(type) {
    this.type = type;
    this.importantStuff = 'red';
  };
  ```

  - constants should be **ALL_UPPER_CASE** and should *not* be declared `const` (see [here](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Constants#Constants))

  ```javascript
  var THIS_IS_CONSTANT = 50;
  ```
  
  - avoid names that would be normally be illegal in common languages; for example keyword names (`class`, `while`) and names that require specific syntax to access (`1234`).

**[§ back to contents](#table-of-contents)**

---

## Variables

Declare all variables using the `var` keyword; avoid polluting the global namespace.

```javascript
var foo = 0;
```

Where possible all variables should be treated as if they are **strictly typed**. Avoid changing the type of a variable where possible, instead preferring a new variable:

```javascript
// BAD
var foo = 'a string';
foo = 43;
if (foo < SOME_NUMBER) {
  foo = [0, 1, 2];
}

// GOOD
var foo = 'a string';
var bar = 43;
if (bar === SOME_NUMBER) {
  var myArray = [0, 1, 2];
}
```

This uses some more memory but can avoid confusion in variable usage and naming.

Multiple variables of the same type can be declared on the same line, otherwise use a new var statement and new line per variable.

```javascript
var thisIsANumber = 0, thisIsAlsoANumber = 0;
var thisIsAString = 'I doff my hat!';
```

Declare all variables at the top of their scope and initialise all variables (note that variables are hoisted to the top of their scope internally so it is good practice to put them there in the first place to make this easier to visualize).

```javascript
// BAD
function () {
  var aVariable;
    
  // Code ...
    
  var anotherVariable;
}

// GOOD

function () {
  var aVariable = 0; // Always initializing, even to 0, tells us the type
  var anotherVariable = 'myVariableIsTheBest'; // A string
  
  // Code ...
}
```

With strings, prefer the use of `'` single quotes instead of `"` double quotes. Where you need to use `'` within a string, use `\'` or if you can use `"` (preferred in html strings), do that instead.

```javascript
// BAD
var myString = "Bad string!";

// GOOD
var myString = 'Good string!';
var htmlString = '<div class="bacon"></div>';
```

**[§ back to contents](#table-of-contents)**

---

## Objects and Arrays

Objects and arrays should always be created with **literal syntax**.

```javascript
// BAD
var myObject = new Object();
var myArray = new Array();

// GOOD
var myObject = { };
var myArray = [ ];
```

Array constructors [can cause issues](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Array_and_Object_literals#Array_and_Object_literals) and it is worth keeping consistency between object and array creation syntax.

**[§ back to contents](#table-of-contents)**

---

## Functions and Classes

For consistency, always declare functions as anonymous function expressions (with a `var` keyword). This implies that all functions must be declared *before* they are used.

```javascript
thisFunction(); // Do NOT try to use functions before they are declared.

var thisFunction = function () {
  // Stuff
};
```

Always declare classes without a `var` keyword and outside of a block or function.

```javascript
// BAD
if (badger === false) {
  function SomeClass() {
    this.isAClass = true;
  };
}

// GOOD
function SomeClass() {
  this.isAClass = true;
};
```

If the class is only required in a closure, prefer the use of an object instance instead.

Attaching a method to an object or class should be done through the prototype:

```javascript
Foo.prototype.bar = function() {
  /* ... */
};
```

**[§ back to contents](#table-of-contents)**

---

## Whitespace

  - Try to keep lines at less than 80 columns (characters) and if possible no more than 100 columns.
  
  - Block braces `{ }` should begin on the same line as the keyword initiating the block.
  
  ```javascript
  while(aFunctionIsTrue()) {
    // Magic.
  }
  ```

  - Indentation should be standard at 2 spaces rather than tabs (you can set your code editor to handle this for you).
  
  ```javascript
  if (myVariable === 'blah') {
    // Indent 2
  }
  ```

  - No space after the `function` keyword.
  
  ```javascript
  var myFunction = function() {
    // Function
  };
  ```

  - One space between `if`, `while`, `for`, `switch` etc and brackets; one space before the leading `{` brace. 
  
  ```javascript
  if (condition) {
    // Neat and tidy
  }

  while (condition) {
    // Neat and tidy
  }
  ```
  
  - No space before `,` comma separators, one space after. No space padding for function arguments unless the argument is a function decleration itself.
  
  ```javascript
  var thisFunction = function(numberArgument, anotherArgument) {
    // Blah
  }

  var anotherFunction = function( function() { /* ... */ } ) {
    // Blah
  }
  ```
  
  - Group code lines logically. This is subjective but try to keep code in logical segments for easier reading.
  
  ```javascript
  myVar = A_CONSTANT_VALUE;
  if (myVar) {
    // Do something useful
  }
  else {
    // Do something useful
  }
  
  while (myVar > ANOTHER_CONSTANT_VALUE) {
    // Do something even more useful
  }

  anotherVar = myVar;
  functionCall(anotherVar);
  ```
  
  - Use single operator spacing.
  
  ```javascript
  var a = 1, b = 2, c = 0;
  c = a + b / a;
  c *= A_CONSTANT;
  ```

  - Indent code that would spill to new lines.
  
  ```javascript
  functionCall(argumentA, argumentB, argumentC)
    .anotherFunction(argumentC)
    .thirdFunction();

  // Fit as much on each line as possible, but don't be afraid to break it up
  functionCallWithALongNameAndLotsOfArguments(argumentA, argumentB,
    argumentC, argumentD, argumentE);
  ```
  
  - One filler space in empty functions, object or array constructs. No filler space in function argument lists unless the argument is an object or array literal or an anonymous function
  
  ```javascript
  var aFunction = function() { }; // No space within (), but one within { }

  var objectFunction = function( { theValue: 50 } ) { };
  var arrayFunction = function( [0, 1, 2, 3] ) { };
  var functionFunction = function( function() { /* ... */ } ) { };

  var anObject = { }; // Empty object
  var anArray = [ ]; // Empty array
  ```
  
  - Ternary operator `?` and `:` components are space-padded.
  
  ```javascript
  var decisionVariable = a > b ? a : b;
  ```

  - `;` must be the end of the line (no trailing space) and no preceding space.

**[§ back to contents](#table-of-contents)**

---

## Syntax

  - Always prefer the `===` and `!==` operators over the `==` and `!=` operators. The former are more efficient as they do not perform type conversion and as our code is strictly typed variables should always be of the correct type in conditionals. The exception here is if you want to check against `null` and `undefined`; in this case, use `==` or `!=`.
  
  ```javascript
  if(myVar === 'Luminosity') {
    // Do a thing.
  }

  if(thisCouldBeNull != null) {
    // Do a thing.
  }
  ```
  
  - Always use semicolons. Do not rely on JavaScript's 'optional' semicolon support. This includes after function declerations (as we use function expressions) but not after `if`, `for`, `while` etc blocks.
  
  ```javascript
  var myFunction = function () {
    // Blah
  }; // Semicolon here.
  ```

  - Prefer dot notation (`.`) for properties when accessing them directly and use subscript (`[]`) notation for accessing properties through a variable. Use subscript notation when accessing properties and functions from an external library. This is to keep support for minification intact; using dot notation on exernal libraries when using minification can lead to issues.
  
  ```javascript
  // Regular properties
  var luke = {
    jedi: true,
    age: 28
  };
  
  // Bad
  var isJedi = luke['jedi'];
  
  // Good
  var isJedi = luke.jedi;

  // Good
  var getProperty = function(prop) {
    return luke[prop]; // Accessed with a variable
  }
  var isJedi = getProp('jedi');
  
  // External library access (included through html or loaded with AJAX)
  var myVariable = 0;
  if(externalLib['functionToCall'] === true) {
    myVariable = externalLib['getANumber'];
  }
  ```
  
**[§ back to contents](#table-of-contents)**

---

## Commenting

  - The main thing to keep in mind with commenting is to **be consistent**.

  - Comments should be `//` style if single-line, `/* */` style if blocking a part of a line, or `/* * */` style if multi-line. Avoid using multiple `//` style comments on their own lines

  ```javascript
  // This is a comment for some code
  var x = 42; // We can also comment like this
  
  /* Commenting parts of a line is acceptable (but try to avoid doing this
   * in production code). Did you notice that this is a multi-line comment?
   */
  var myFunction = function(arg0, arg1, /* arg2 */);
  
  // Don't
  // Do
  // This,

  /* Do
   * this
   * instead.
   */
  ```
  
  - Comments should have a space after the commenting syntax and start with a capital letter unless the comment begins with a function reference.
  
  ```javascript
  // This is a comment

  /* As is this. This is actually a multi-line comment; lets pad it out
   * a bit by talking gibberish.
   */

  // functionReference does a thing.
  ```
  
  - Prefer descriptive identifier names and clean code layout (self-documenting code or code that is easy to read by itself) to obscure names with lots of commenting.
  
  - Use [JSDoc] commenting (`/** * */` syntax) for functions and providing additional information about argument types etc.
  
  ```javascript
  /**
   * Represents a book.
   * @constructor
   * @param {string} title - The title of the book.
   * @param {string} author - The author of the book.
   */
  function Book(title, author) {
    // This is a class constructor
  };

  /**
   * A function with no arguments.
   */
  var myFunction = function() {
    // A simple every day function
  };

  /**
   * Returns the sum of a and b.
   * @param {Number} a - A number
   * @param {Number} b - Another number
   * @returns {Number} Sum of a and b
   */
  var sum = function(a, b) {
    return a + b;
  };

  /**
   * Private function.
   * @private
   */
  var aSecretFunction = function() {
    // Private secret stuff.
  }
  ```

**[§ back to contents](#table-of-contents)**

---

## Features to Avoid

  - `delete` - prefer settig to `null`; deletion of objects is costly and unnecessary.

  - `with` - avoid colliding properties on objects to begin with - can lead to confusion [and bugs](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=with___%7B%7D#with___%7B%7D). Note that `with` is prohibited in strict mode.

  - `for-in` with arrays - only use it for iterating over keys in objects, hashes and maps. Can cause [unexpected behaviour](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=for-in_loop#for-in_loop) when used with Arrays.

  - Associative arrays - always use numerical indices with arrays (0, 1 etc); if you want to use other types of keys use an object instead.

  - Multiline strings - prefer concatenated strings instead of using `\` to create multiline strings. It [can cause issues](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Multiline_string_literals#Multiline_string_literals) with whitespace stripping and minifying code.

  - Modifying built-in object prototypes - avoid anything that could easily be a source of unnecessary confusion.

  - Global namespace variables and objects where possible. Prefer keeping variables and objects grouped into functions, classes and namespaces to avoid polluting the global namespace.

**[§ back to contents](#table-of-contents)**

---

[Google JavaScript Style Guide]:https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
[Airbnb JavaScript Style Guide]:https://github.com/airbnb/javascript#the-javascript-style-guide-guide
[JSDoc]:http://usejsdoc.org/