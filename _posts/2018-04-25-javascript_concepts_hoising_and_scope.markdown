---
layout: post
title:      "Javascript Concepts: Hoisting and Scope"
date:       2018-04-25 02:17:39 -0400
permalink:  javascript_concepts_hoising_and_scope
---


Scope and hoisting in Javascript can be slightly confusing concepts to a novice JS coder. While I'm learning these concepts, I wanted to write this post as both a future reference for myself or anyone else.

To learn about this topic, I used Kyle Simpson's *You Don't Know JS* book on Github, a great open-source resource for learning about JS. Below is a summary of the points I found most helpful:

### Scope

There are two main ways scope is implemented in JS: 1) function scope and 2) block scope: 

**Function scope**: when a function is declared, a new scope is created. Any variables declared inside that function (with keywords 'var', 'let', and 'const') are accessible only within the function. The only exception to this is if you declare a variable without any keyword (example: a = 2;). Variables declared without keywords var, let, or const will always have global scope.

**Block scope**: block scope only applies to variables declared with 'let' and 'const'. Variables declared inside a code block { } with 'let' or 'const' are scoped  within that code block and are unavailable outside of it. Variables declared with the 'var' keyword are not block-scoped and will be scoped outside of the code block. Consider the  'if' statement below. If you tried to console.log() each variable after the 'if' statement, JS would throw a reference error on all but 'c'. 
```
if (true) {
     let a = 1;     // only available within this code block
		 const b = 2;     // only available within this code block
		 var c = 3;     // available outside this code block
}
```

### Hoisting

The concept of 'hoisting' in JS is a result of the way the JS engine compiles code. Before code is run, the JS engine sweeps through the code and assigns memory space (but does *not* assign a value) to certain variable and function declarations.  This helps us understand why the following code would print 'undefined' even though 'a' is printed to the log even before it is declared or defined. Prior to runtime, the JS engine would run through the code and allocate memory for 'a' (this is why the code does not throw a ReferenceError); however, since the engine does not assign a value to 'a' during that compilation phase, the log prints 'undefined'. This concept is called 'hoisting'.
```
console.log(a);     // prints out 'undefined'

var a = 2;
```
However, not all declarations are hoisted in this manner. Declarations that *are* hoisted include variables declared with 'var' and named function declarations. Declarations that *are not* hoisted include: variables declared with 'let' and 'const', unnamed functions, expression functions, or when a function is declared at the same time it is assigned to a variable (i.e. 'var fn = function myFunction() {...}').
 
