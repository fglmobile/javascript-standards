The FGL JavaScript Style Guide
==============================

---

The FGL JavaScript Style Guide is largely based on practices from previously published JavaScript Style Guides with some of our own experience thrown in. In a language like JavaScript that does not do a lot to enforce particular coding conventions it is important to define and stick to a set of guidelines to ensure that code is performant, readable and easy to maintain across development cycles. Our style guide is intended to make it simple to achieve this by giving an example-driven approach to coding style in JavaScript.

For anything that is not mentioned specifically in the style guide, refer to (in order of priority) the [Google JavaScript Style Guide], [Airbnb JavaScript Style Guide] and other code in our codebase. Most importantly, if it looks like a messy hack-job **it probably is**.

---

##Table of Contents

0. [Naming](#naming)
0. [Variables](#variables)
0. [Objects and Arrays](#objects-and-arrays)
0. [Functions and Classes](#functions-and-classes)
0. [Whitespace](#whitespace)
0. [Syntax](#syntax)
0. [Commenting](#commenting)
0. [Features to Avoid](#features-to-avoid)

---

## Naming

Type names should follow these conventions:

  - variables should use **camelCaseNaming**

  ```javascript
  var myVariableIsCool = 0;
  ```

  - functions should be **camelCaseFunctions()**

  ```javascript
  var myFunkyTion = function myFunkyTion() {
      //does funky stuff
  };
  ```
  
  - classes should have **CapitalizedFirstLetters**.

  ```javascript
  function ThisIsAClass(type) {
      this.type = type;
      this.importantStuff = "red";
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
//BAD
var foo = 'a string';
foo = 43;
if (foo < SOME_NUMBER) {
  foo = [0, 1, 2];
}

//GOOD
var foo = 'a string';
var bar = 43;
if (bar == SOME_NUMBER) {
  var myArray = [0, 1, 2];
}
```

This uses some more memory but can avoid confusion in variable usage and naming.

Multiple variables of the same type can be declared on the same line, otherwise use a new var statement and new line per variable.

```javascript
var thisIsANumber = 0, thisIsAlsoANumber = 0;
var thisIsAString = 'I doff my hat!';
```

Declare all variables at the top of their scope and initialise all variables.

```javascript
//BAD
function () {
  var aVariable;
    
  //code ...
    
  var anotherVariable;
}

//GOOD

function () {
  var aVariable = 0;
  var anotherVariable = 'myVariableIsTheBest';
  
  //code ...
}
```

Strings should always use `'` single quotes instead of `"` double quotes.

```javascript
//BAD
var myString = "Bad string!";

//GOOD
var myString = 'Good string!';
```

**[§ back to contents](#table-of-contents)**

---

## Objects and Arrays

Objects and arrays should always be created with **literal syntax**.

```javascript
//BAD
var myObject = new Object();
var myArray = new Array();

//GOOD
var myObject = {};
var myArray = [];
```

Array constructors [can cause issues] and it is worth keeping consistency between object and array creation syntax.

**[§ back to contents](#table-of-contents)**

---

## Functions and Classes

Always declare functions with a `var` keyword:

```javascript
var thisFunction = function thisFunction() {
  //stuff
};
```

Always declare classes without a `var` keyword and outside of a block or function.

```javascript
//BAD
if (badger == false) {
  function SomeClass() {
    this.isAClass = true;
  };
}

//GOOD
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

  - Lines should be less than 80 columns (characters), and definitely no more than 100 columns.

  - Indentation should be standard at 2 spaces rather than tabs (you can set your code editor to handle this for you).
  
  ```javascript
  if (myVariable === 'blah') {
      //indent 2
  }
  ```

  - No space after the `function` keyword.
  
  ```javascript
  var myFunction = function() {
      //function
  };
  ```

  - One space between `if`, `while`, `for`, `switch` etc and brackets; one space before the leading `{` brace.
  
  ```javascript
  if (condition) {
      //neat and tidy
  }

  while (condition) {
      //neat and tidy
  }
  ```
  
  - No space before `,` comma separators, one space after. No space padding for function arguments unless the argument is a function decleration itself.
  
  ```javascript
  var thisFunction = function(numberArgument, anotherArgument) {
      //blah
  }

  var anotherFunction = function( function() { /* ... */ } ) {
      //blah
  }
  ```
  
  - Group code lines logically. This is subjective but try to keep code in logical segments for easier reading.
  
  ```javascript
  myVar = A_CONSTANT_VALUE;
  if (myVar) {
      //do something useful
  } else {
      //do something useful
  }
  
  while (myVar > ANOTHER_CONSTANT_VALUE) {
      //do something even more useful
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

  //fit as much on each line as possible, but don't be afraid to break it up
  functionCallWithALongNameAndLotsOfArguments(argumentA, argumentB,
      argumentC, argumentD, argumentE);
  ```
  
  - One filler space in empty functions, object or array constructs. No filler space in function argument lists.
  
  ```javascript
  var aFunction = function() { }; //no space within (), but one within { }

  var anObject = { }; //empty object
  var anArray = [ ]; //empty array
  ```
  
  - Ternary operator `?` and `:` components are space-padded.
  
  ```javascript
  var decisionVariable = a > b ? a : b;
  ```

  - `;` must be the end of the line (no trailing space) and no preceding space.

**[§ back to contents](#table-of-contents)**

---

## Syntax

*TO BE DONE*

**[§ back to contents](#table-of-contents)**

---

## Commenting

*TO BE DONE*

**[§ back to contents](#table-of-contents)**

---

## Features to Avoid

  - `delete` - prefer settig to `null`; deletion of objects is costly and unnecessary.

  - `with()` - avoid colliding properties on objects to begin with - can lead to confusion [and bugs](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=with___%7B%7D#with___%7B%7D).

  - `for-in` with arrays - only use it for iterating over keys in objects, hashes and maps. Can cause [unexpected behaviour](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=for-in_loop#for-in_loop) when used with Arrays.

  - Associative arrays - always use numerical indices with arrays (0, 1 etc); if you want to use other types of keys use an object instead.

  - Multiline strings - prefer concatenated strings instead of using `\` to create multiline strings. It [can cause issues](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Multiline_string_literals#Multiline_string_literals) with whitespace stripping and minifying code.

  - Modifying built-in object prototypes - avoid anything that could easily be a source of unnecessary confusion.

  - Global namespace variables and objects where possible. Prefer keeping variables and objects grouped into functions, classes and namespaces to avoid polluting the global namespace.

**[§ back to contents](#table-of-contents)**

---

[Google JavaScript Style Guide]:https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
[Airbnb JavaScript Style Guide]:https://github.com/airbnb/javascript#the-javascript-style-guide-guide
[can cause issues]:https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Array_and_Object_literals#Array_and_Object_literals