# Looping and Control Expressions 

## `if` keyword

Works exactly like in JavaScript.

```scala
val someCondition = true

if (someCondition) {
  // do something
}
```

## Ternary expressions

In Javascript, ternary expressions are written like this:

```javascript
const x = y < z ? 'greater' : 'lesser';
```

In Scala, there is no ternary operator. This is because you can assign the result of `if-else` like so:

```scala
val x = if (y < z) "greater" else "lesser"

// it can even be written long form if you have some complicated multi line logic!
val x = if (y < z) {
  // some complicated logic
  // multiline
  // logic
  "greater"  // <= the last line of the if/else is what is set to `x`
} else {
  "lesser"
}
```

The ability to write multi-line if/else conditions as well as short, 1 liner readable if/else statements makes this feature much more powerful than what is available in JS.

**NOTE**: This is considered an "if _expression_", not an "if statement".

Why does this matter? Because, you can write this statement:

```scala
val someCondition = true

println(if (someCondition) 5 else 3)

// prints "5" to screen. No errors are thrown!
```

## Code Blocks

In Scala, you can write blocks of code and assign them to variables. 

```scala
val aCodeBlock = (
  val x = 2
  val y = x + 1 

  if (y > 2) "greater" else "lesser"
)
```

*The last line of the code block is always what the variable is evaluated to*. So the case above, `aCodeBlock` is evaluated to string `greater`.

If the code block was like this:

```scala
val aCodeBlock = (
  val x = 2
  val y = x + 1 

  if (y > 2) "greater" else "lesser"
  3  // <= This is the last line so this is what it gets evaluated to. 
)
```

Then `aCodeBlock` would be evaluated to `3`.


## ALMOST EVERYTHING IS AN EXPRESSION

In Scala, almost everything is considered an expression (with the exceptions being class and method declarations).

First let's make sure we know what the difference between statement and expression is. 

* A *statement* is some action or command.
* An expression is some some action or command that **evaluates to a value**

In JavaScript,

```javascript
const x = 3;                      // expression because it evaluates to a value
const y = someCondition ? 5 : 3;  // expression because it evaluates to a value

function printTo3() {
  console.log(1);
  console.log(2);
  console.log(3);
}

printTo3();                       // statement.

if (x === 3) {                    // statement
  printTo3();
}
```

Again, everything is considered an expression in Scala because everything ALWAYS returns a value.

```scala
const x = 3                             // an expression (returns 3)
const y = if (someCondition) 5 else 3   // an expression (returns 5 or 3) 

def printTo3() {
  println(1);
  println(2);
  println(3);
}

printTo3()                              // an expression (returns "Unit" type which is the equivalent of "void" in other languages) 
println(printTo3())                     // an expression (returns "Unit" type since println() doesn't return some concrete value). 
                                        // outputs "()" which is what is returned for "Unit" aka. "void" types.

if (x == 3) {                           // this block of code would evaluate to "Unit" as well
  printTo3()
}

```


