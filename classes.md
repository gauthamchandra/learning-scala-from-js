# Classes

## Declaring classes and instantiating them

Let's say you have a class `Person` in JavaScript that looks like this:

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  speakTo(name) {
    console.log(`Hi ${name}, my name is ${this.name} and I am ${this.age} years old. Pleased to meet you!`);
  }
}

// then later, you instantiate like this:
const person = new Person("John", 30);
person.speakTo("Bob"); // => logs "Hi Bob, my name is John and I am 30 years old. Pleased to meet you!"
```

If you wanted to do the same thing in Scala, it would be this:

```scala
// in scala, the constructor arguments are part of the class declaration line!
class Person(name: String, age: Int) {

  // which means that everything inside this class automatically has access to the "name" and 
  // "age" constructor args. The "this.name" is used to differentiate between class and method vars
  def speakTo(name: String): Unit = {
    println(s"Hi $name, my name is ${this.name} and I am $age years old. Pleased to meet you!")
  }
}

// you can even declare classes with no body
class ClazzWithNoBody

// then you instantiate and call the methods like so:
val person = new Person("John", 30)
person.speakTo("Bob")

val foo = new ClazzWithNoBody
```

### Shorthand for exposing constructor arguments as PUBLIC instance variables


```scala
// If you add a "val" prefix to any constructor argument like so:
class Person(val name: String, age: Int)

val person = new Person("John", 30)

// then you can directly reference them
person.name // => John
person.age  // => ERROR (age constructor argument) isn't prefixed with "val"
            //    so its not available outside the class!
```

### The Class Body

In Scala, anything inside the class body can be any arbitrary expression (and not just methods).

**IMPORTANT**: All code inside the class is executed _everytime an instance is created_  

```scala
class Person(name: String, age: Int) {
  println("This will be printed everytime this class is instantiated!")

  def speak = println("Speaking!")
}
```

```javascript
// The easiest way to think about this is that its the equivalent of Javascript Function Type:
function Person(name, age) {
  console.log('This will be printed everytime this class is instantiated!');

  this.speak = () => println('Speaking!');
}

// OR that everything is inside a JS class constructor
class Person {
  constructor(name, age) {
    console.log('This will be printed everytime this class is instantiated!');

    this.speak = () => println('Speaking!');
  }
}
```

### Multiple or "Auxiliary" Constructors

In JavaScript, a class can only have one constructor:

```javascript
// JS throws a "duplicate constructor in the same class" error.
class Foo {
  constructor(someArg) {}
  constructor(arg1, arg2) {}
}
```

Scala (and Java and a few other languages) unlike JavaScript allows the user to specify multiple constructors for the same class. This is used sometimes to allow multiple ways to construct the same class (often initialized with default arguments).

```scala
// 1st constructor (as part of class declaration)
class Person(name: String, age: Int) {
  // use "this" keyword as the name of the method to define a 2nd constructor
  def this(name: String) = {
    this(name, 0)
  }

  // define a 3rd constructor
  def this() = {
    this("John Doe")
  }
}

// this will call the 3rd constructor, which utilizes the 2nd constructor, which utilizes primary constructor
val person = new Person
```

Most of the time, secondary constructors aren't very useful (as you can use default and named parameters with your constructor arguments). 


## Notations (ways of calling methods)

### Infix Notations (aka. operator notation) = Woaahhhh! 😲😲

When you have a method of a class **that only takes 1 argument**, then Scala allows you to call your method in a totally different way:

```scala
class Dog(name: String) {
  def runsTo(name: String) = println(s"The dog ${this.name}, runs to $name")
}

let dog = new Dog("Scruffy")

// normally called like this:
dog.runsTo("Joe") // => prints "The dog Scruffy, runs to Joe"

// but you could also call it like...this!
dog runsTo "Joe"  // => prints "The dog Scruffy, runs to Joe"
```

Unlike other languages, I can use special characters like "+" in Scala as method names so I can even do:

```scala
class Dog(name: String) {
  def +(name: String) = println(s"The dog ${this.name}, runs to $name")
}

let dog = new Dog("Scruffy")
dog + "Joe" // this is "infix" notation.
```

Which might prompt the question:

> Wait! Wait! Then how does Scala know when I use an operator (e.g. 9 + 9) and when I use the method "+"? 🤔 

The answer is simple: The operator "+" [**is actually a method on the `Int`**](https://www.scala-lang.org/api/2.12.2/scala/Int.html)!!

Effectively... 

```
9 + 9  // => 18 (Infix notation)
9.+(9) // => 18 (same as the line above) (Normal method notation)
```

In fact, ALL OPERATORS ARE METHODS. This is almost exactly how it works in the Ruby Programming language as well.

### Unary Prefix Notation

Lets look at this statement:

```scala
val x = -1
```

This is actually also a method invocation! The above statement is _exactly_ the same as:

```scala
val x = 1.unary_-

// There is actually a method in Int that has this method defined:
class Int {
  // Note the space between the ":" and the method name.
  // If we don't add the space, Scala will think the ":" is part of the method name
  def unary_- : String = { /* some code */ } 
}
```

These are known as unary operators and work on **ONLY** the following:

* `-`
* `+`
* `~`
* `!`

So lets define one below:

```scala
class Dog(name: String) {
  def unary_! = "I am actually a cat in disguise! Hehehe"
}

val dog = new Dog("Scruffy")
println(!dog) // prints out "I am actually a cat in disguise! Hehehe"
```

### Postfix Notation

For methods without any arguments, Scala allows someone to write a method like this:

```scala
class UserManager {
  // inspired by the joke Android API function
  def isUserAGoat = false
}

val u = new UserManager

u.isUserAGoat // => false. (the standard way of calling it)
u isUserAGoat // => false. (the "postfix notation" way of calling it)
              // note the space between the instance and the method name 
```

## The `apply()` method.

In Scala, if your class has a method `apply()` that takes no arguments and returns a `String`, then it allows you to write a shorthand:

```scala
class Foo {
  def apply(): String = "Consider myself being applied"
}

val foo = new Foo

// how you normally call it
foo.apply()

// or even shorter
foo.apply

// but in Scala, because you defined this special "apply" method, you can now
// use this shorthand to call the instance like a function!
foo() // <= this is only possible because you defined the apply() method
```
