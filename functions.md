# Functions

In Javascript, you define a function like this:

```javascript
function add(x, y) {
  return x + y;
}

// or even more tersely
const add = (x, y) => x + y;

function printSomething(str) {
  console.log(str);
}
```

In Scala, this would be:

```scala
// we must specify what the argument types are going to be
def add(x: Int, y: Int): Int = {
  x + y
}

// or even more tersely.
// Note that there is no return type explicitly specified. That's because Scala
// is smart enough to infer the type as `Int` here.
def add(x: Int, y: Int) = x + y

// Note how it returns a "Unit" return type. This is like saying "function printSomething(): void" in TypeScript. 
// aka. it doesn't actually return a real value
def printSomething(str: String): Unit = println(str)
```

## Last lines of functions are implicitly returned.

In Scala, the last line of a function is implicitly returned. For this last line, the `return` keyword is redundant and therefore not used:

```scala
def add(x: Int, y: Int): Int = {
  println(s"About to add! $x + $y")
  x + y   // <= note how there's no "return" keyword. That's because this is the
          //    last line and therefore not needed.
}
```

## Invoking Functions

Invoking the `add` function defined above is simply done with parentheses (like in JS): `add(3, 5)` 

**NOTE**: Invoking functions with NO parameters is a little different:

```scala
def sayHello() {
  println("Hello")
}

sayHello()  // invoking function. Returns "()" aka. "Unit" type.
sayHello    // also invoking function and the same result as the above statement!
            // This only works because its a function with 0 arguments.
```

## Functions within Functions

Just like in Javascript, you can define functions within functions:

```scala
def someCalculation(x: Int, y: Int): Int = {
  def add(x: Int, y: Int) = x + y
  def multiply(x: Int, y: Int) = x * y

  add(x, y) + multiply(x, y)
}

someCalculation(3, 4) // => 19
```

## Function Parameters 

### Default Parameter Values

As in Javascript, the way to define default parameter values is the same:

```scala
def sayHello(name: String = "Bob"): Unit = {
  println(s"Hello, $name!")
}

sayHello() // => Hello, Bob!
```

### Invoking with Named parameters

Let's say you have a complicated function with lots of arguments in JavaScript:

```javascript
function savePerson(firstName, lastName, age, weight, height, governmentIdentificationNumber, ethnicity) {
  ...
}
```

This would be very unweildy to call. You are bound to mess up the order of the arguments when calling. Yes, you could use something like [The Builder Pattern](https://en.wikipedia.org/wiki/Builder_pattern) so that it becomes something like this:

```javascript
new Person.Builder()
  .firstName('Billy')
  .lastName('Bob')
  .age(48)
  ...
  .build();
```

However this forces you to create the builder with all these methods. 

Scala allows you to simply specify the parameters by name when invoking the function:

```scala
savePerson(
  firstName = "Billy",
  lastName = "Bob",
  age = 45,
  weight = 200,
  ...
)
```

*NOTE*: Order does not matter. Since you are specifying what parameter has what value, the compiler is smart enough to understand, even if you have it in a different order than the one in the method definition.

### Calling by "Name" vs Calling by "Value"

In Scala, there are two ways you can call a function:

* Calling by value
* Calling by name 

Its best explained in an example.

```scala
def callByValue(x: Long): Unit = {
  println("By Value " + x)
  println("By Value " + x)
}

// invoke it but pass in the current time in nano seconds
callByValue(System.nanoTime())

// => By Value 257792457857317
//    By Value 257792457857317
```

This intuitively makes sense and what most JS programmers are used to. This is calling it by "value". It calls `System.nanoTime()`, gets the value and then passes that same number into the function. 

Now let's look at calling by "name":

```scala
def callByName(x: => Long): Unit = {
  println("By Name " + x)
  println("By Name " + x)
}

// invoke it but pass in the current time in nano seconds
callByName(System.nanoTime())

// => By Name 257792512548972
//    By Name 257792512577363
// 😱😱, These two numbers printed are NOT the same!
```

Wait...what happened? Well when we specified the parameter as `x: => Long`, we told the compiler to:

> Take whatever expression that was passed in and evaluate it _EVERY TIME IT IS REFERENCED INSIDE THE FUNCTION_

So each time in the `println(..)` statement above, the program re-ran `System.nanoTime()`


## Recursive Functions

**IMPORTANT**: Although this will be _super_ weird at first, whenever you feel the need to reach for while loops...DON'T. Use recursive functions instead. [This StackOverflow post says it best](https://stackoverflow.com/questions/18674743/is-there-any-advantage-to-avoiding-while-loops-in-scala)

Unlike normal functions, the compiler cannot infer the return types of recursive functions. So this should be explicitly specified (as is best practice anyway) 

```scala
def say(str: String, times: Int): String = {
  if (times == 1) {
    str
  }
  else {
    str + " " + say(str, times - 1)
  }
}

say("Bye", 3) // outputs "Bye Bye Bye"
```

### Tail Recursion

Scala optimizes for tail recursion. 

So what is it exactly? Let's look at two functions that accomplish the same thing:

```scala
// NOTE: We are using BigInt here as factorials can easily grow into such huge numbers that we often can't fit it all in
//       a standard "Int"
def factorial(n: BigInt): BigInt = {
  if (n <= 1) 1
  else n * factorial(n - 1)
}

def tailRecursiveFactorial(n: BigInt): BigInt = {
  def factorialReducer(n: Int, accumulator: BigInt = 1): BigInt = {
    if (n <= 1) accumulator
    else factorialReducer(n - 1, n * accumulator)
  }

  factorialReducer(n)
}
```

If you run these functions like so:

```scala
println(factorial(5000))              // => Results in a StackOverflow
println(tailRecursiveFactorial(5000)) // => Returns some super large number 
```

Why? Why does the first result in a stack overflow but the 2nd doesn't?

Because with `factorial`, every time it hits the else block, the program needs to keep a new stack frame.

So for factorial of 4, it would be:

1. 4 * factorial(3)
2. 3 * factorial(2)
3. 2 * factorial(1)
4. hit base case

This means for a factorial of 5000, it would need to hold 5000 stack frames in memory to get to the base case and then work
backwards. That's often too many frames for the program stack to hold...resulting in a `StackOverflowError`

With `tailRecursiveFactorial`, its a bit different. The stack frames work out to:

1. factorialReducer(3, 4 * 1)
2. factorialReducer(2, 3 * 4 * 1)
3. factorialReducer(1, 2 * 3 * 4 * 1)
4. hit base case 

Notice in this case that the full result is in the `accumulator` the whole time. The `else` block **only** has references to itself. This tells the compiler that every time it recurses, it can actually just throw away the previous stack frames because all the information is already there in the current stack frame for `else`.

This reduces the number of stack frames to just 2:

1. factorialReducer(1, 2 * 3 * 4 * 1)
2. base case.

Making tail recursive functions becomes much easier if you utilize "accumulator" variables that are passed into the next invocation.

### Informing the compiler a method is tail recursive with the `@tailrec` annotation:

```scala
@tailrec
def tailRecursiveFactorial(n: BigInt): BigInt = {
  ...
}
```

This tells the compiler that this is supposed to be a tail recursive function. If the compiler finds that this is not the case, it will throw an error.
