# Chapter 3 - Functions 😀😀😀😀😀

<ShortExercise title="Test titel veranderd2">
    Dit is een test voor idtagger cloud function.
</ShortExercise>

> "People think that computer science is the art of geniuses but the actual
> reality is the opposite, just many people doing things that build on each
> other, like a wall of mini stones."

-Donald Knuth

<p align="center">
    <img  src="https://dwa-courses.firebaseapp.com/img/chapter_picture_3.jpg" alt="img">
</p>

Functions are the bread and butter of JavaScript programming. The concept of
wrapping a piece of program in a value has many uses. It gives us a way to
structure larger programs, to reduce repetition, to associate names with
subprograms, and to isolate these subprograms from each other.

The most obvious application of functions is defining new vocabulary. Creating
new words in prose is usually bad style. But in programming, it is
indispensable.

Typical adult English speakers have some 20,000 words in their vocabulary. Few
programming languages come with 20,000 commands built in. And the vocabulary
that is available tends to be more precisely defined, and thus less flexible,
than in human language. Therefore, we usually have to introduce new concepts to
avoid repeating ourselves too much.

# Defining a function

A function definition is a regular binding where the value of the binding is a
function. For example, this code defines square to refer to a function that
produces the square of a given number:

```js
const square = function(x) {
  return x * x;
};

console.log(square(12));
// → 144
```

<Note>

Voor een function binding kunnen we wel goed voorstellen als een tentakel die de
definitie van de functie grijpt zoals in onderstaande figuur te zien is.

<p align="center">
    <img  src="https://dwa-courses.firebaseapp.com/img/memory_model/chap03/square_verbose.svg" alt="img">
</p>

Omdat de functie-definities te groot kunnen worden voor een dergelijk diagram,
schrijven we meestal Function Definition in plaats van de hele broncode.
Daarnaast schrijven we de naam van de functie soms bovenop het blok van de
functie-definitie. Dit is hieronder te zien:

<p align="center">
   <img  src="https://dwa-courses.firebaseapp.com/img/memory_model/chap03/square_concise.svg" alt="img">
</p>

Om aan te geven dat dit hulpmiddelen voor onszelf zijn en niet in
JavaScript-engine bestaan, geven we de tekst cursief weer.

</Note>

A function is created with an expression that starts with the keyword function.
Functions have a set of parameters (in this case, only x) and a body, which
contains the statements that are to be executed when the function is called. The
function body of a function created this way must always be wrapped in braces,
even when it consists of only a single statement.

A function can have multiple parameters or no parameters at all. In the
following example, makeNoise does not list any parameter names, whereas power
lists two:

```js
const makeNoise = function() {
  console.log('Pling!');
};

makeNoise();
// → Pling!

const power = function(base, exponent) {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};

console.log(power(2, 10));
// → 1024
```

Some functions produce a value, such as power and square, and some don’t, such
as makeNoise, whose only result is a side effect. A return statement determines
the value the function returns. When control comes across such a statement, it
immediately jumps out of the current function and gives the returned value to
the code that called the function. A return keyword without an expression after
it will cause the function to return undefined. Functions that don’t have a
return statement at all, such as makeNoise, similarly return undefined.

Parameters to a function behave like regular bindings, but their initial values
are given by the caller of the function, not the code in the function itself.

<Note>

Zoals je kunt zien in het diagram van de function binding power nemen we de
paramaters en de return-waarde niet als aparte ‘doosjes’ op in de definitie van
de functie.

<p align="center">
   <img  src="https://dwa-courses.firebaseapp.com/img/memory_model/chap03/power_verbose.svg" alt="img">
</p>

De JavaScript engine houdt deze bindings echter wel bij. Verderop laten we zien
waar de engine dit doet.

</Note>

# Bindings and scopes

Each binding has a scope, which is the part of the program in which the binding
is visible. For bindings defined outside of any function or block, the scope is
the whole program—you can refer to such bindings wherever you want. These are
called global.

But bindings created for function parameters or declared inside a function can
be referenced only in that function, so they are known as local bindings. Every
time the function is called, new instances of these bindings are created. This
provides some isolation between functions—each function call acts in its own
little world (its local environment) and can often be understood without knowing
a lot about what’s going on in the global environment.

Bindings declared with let and const are in fact local to the block that they
are declared in, so if you create one of those inside of a loop, the code before
and after the loop cannot “see” it. In pre-2015 JavaScript, only functions
created new scopes, so old-style bindings, created with the var keyword, are
visible throughout the whole function that they appear in—or throughout the
global scope, if they are not in a function.

```js
let x = 10;
if (true) {
  let y = 20;
  var z = 30;
  console.log(x + y + z);
  // → 60
}
// y is not visible here
console.log(x + z);
// → 40
```

Each scope can “look out” into the scope around it, so x is visible inside the
block in the example. The exception is when multiple bindings have the same
name—in that case, code can see only the innermost one. For example, when the
code inside the halve function refers to n, it is seeing its own n, not the
global n.

```js
const halve = function(n) {
  return n / 2;
};

let n = 10;
console.log(halve(100));
// → 50
console.log(n);
// → 10
```

<ShortExercise id="r3CLzHi82RLnYGVaDBZJ" title="Oefening 3.2.1: Scoping rules let and var (A)">

Oefening 3.2.1: Scoping rules let and var (A) Schrijf voor elk van de
onderstaande stukken code de output op van console.log. Kies daarbij uit 42,
undefined, of ReferenceError

Voorbeeld: code

```js
console.log(a1);
let a1 = 'hello';
Voorbeeld: antwoord;
```

```
error
```

De oefening

```js
console.log(a2);
var a2 = 42;
```

</ShortExercise>

<ShortExercise id="gekH3dwXwNvE9qOnQp4F" title="Oefening 3.2.2: Scoping rules let and var (B1)">

```js
if (true) {
  let b1 = 42;
}
console.log(b1);
```

</ShortExercise>

<ShortExercise id="1QrzsmGtb7TMNsU0wRDc" title="Oefening 3.2.3: Scoping rules let and var (B2)">

```js
if (true) {
  var b2 = 42;
}
console.log(b2);
```

</ShortExercise>

<ShortExercise id="8iZux1FzUS7REpQXNZQY" title="Oefening 3.2.4: Scoping rules let and var (C1)">

```js
for (let c1 = 0; c1 < 42; c1++) {
  //do something interesting
}
console.log(c1);
```

</ShortExercise>

<ShortExercise id="tEcMQsnQGUPDEyK3212m" title="Oefening 3.2.5: Scoping rules let and var (C2)">

```js
for (var c2 = 0; c2 < 4; c2++) {
  //do something interesting
}
console.log(c2);
```

</ShortExercise>

<ShortExercise id="7jjeIgOakjQS8uqffFvd" title="Oefening 3.2.6: Scoping rules let and var (D1)">

```js
function test() {
  let d1 = 42;
}
console.log(d1);
```

</ShortExercise>

<ShortExercise id="7vUeILPYPNx6VJDT6hzJ" title="Oefening 3.2.7: Scoping rules let and var (D2)">

```js
function test() {
  var d2 = 42;
}
console.log(d2);
```

</ShortExercise>

