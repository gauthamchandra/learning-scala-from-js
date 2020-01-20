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
