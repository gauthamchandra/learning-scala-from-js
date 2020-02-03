# Class Inheritance

Scala supports only single parent inheritance and works the same way as ES6 JavaScript and Java.

```scala
class Animal {
  def speak() = "<noise>"
}

class Cat extends Animal

val cat = new Cat
cat.speak() // => "<noise>"
```

## Accessibility modifiers

To make methods or properties private to code outside the class, use the `private` or `protected` modifiers:

```scala
class Animal {
  // only accessible inside this class
  private def genetics() = "<supa-sekrit-dna>"

  // accessible to this class and all child classes that inherit from it 
  protected def digest() = println("Digesting!")

  // public to all
  def speak(): "<noise>"
}

class Cat extends Animal {
  def eat(): Unit = {
    println("Eating!")

    // can call parent methods easily
    // can also be more explicit about parent class methods by using "super"
    // super.digest
    digest
  }
}
```

## Inheriting from classes that have constructors

```scala
class Person(name: String, age: Int) {
  // some code...
}

// You MUST specify the constructor that
class WorkingAdult(name: String, age: Int, job: String) extends Person(name, age)
```

## Overriding methods or properties

In scala, all methods being overriden explicitly need to be prefixed with `override`:

```scala
class Animal {
  val name = "Unknown"
  def speak() = "<noise>"
}

class Cat(catName: String) extends Animal {
  override val name = catName
  override def speak() = "Meow!"
}

// or even shorter, you can use the override directly in constructor arguments!
class Cat(override val name: String) extends Animal {
  override def speak() = "Meow!"
}

val cat = new Cat("Whiskers")
cat.name // => Whiskers
```

## Restricting Inheritence

### Preventing classes or methods from being inherited

If you want to ensure noone can inherit from the class, use the `final` keyword just like in Java.

```scala
final class Animal
class Cat extends Animal // => this throws a compiler error
```

The `final` keyword can also be used on individual methods to prevent them from being overriden:

```scala
class Animal {
  final def speak() = "<noise>"
}

class Cat extends Animal {
  override def speak() = "Meow" // => throws an error because the parent method is "final"
}
```

### Allowing inheritance to only happen inside the same file

To allow inheritance in _only_ the file where the class resides, use the `sealed` keyword:

```scala
/* ====== start of Animal.java ====== */
sealed class Animal

// these are all allowed because this is in the same file as the Animal class declaration
class Cat extends Animal
class Dog extends Animal
class Elephant extends Animal

/* ====== end of Animal.java ====== */

/* ====== start of Test.java ====== */
class Panda extends Animal // => Throws a compiler error because the class can't be extended from outside the file 
```

## Abstract Classes

If you want to have a class that has missing functionality that needs to be implemented by child classes, use abstract classes.

```scala
abstract class Animal {
  // no value assigned to constant.
  // needs to be implemented by child class 
  val name: String

  // no method body provided
  // needs to be implemented by child class.
  def poop(): Unit

  // method has concrete body so does NOT need to implemented by child class
  def speak() = "Hello!"
}

class Cat extends Animal {
  override val name = "Cat"
  
  override def poop() = println("Going to the cat litter to do my business!")
}

val cat = new Cat
cat.poop
```

**NOTE to non-JVM devs**: Because Abstract classes are missing functionality, they cannot be created directly. They must be inherited by a child class that implements those methods 

## Anonymous Classes

What if you wanted to dynamically implement an abstract class for a one time use?

Enter anonymous classes:

```scala
abstract class Animal {
  def speak: String 
}

// note how this is defined using curly braces
// that allows us to implement the animal class on demand
// and get back the instance
val mysteryAnimal = new Animal {
  override def speak = "Hi, I am a mystery animal"
}

println(mysteryAnimal.speak)    // => "Hi, I am a mystery animal"
println(mysterAnimal.getClass)  // => "class some.package.AnonymousClasses$$anon$1"
```

What is actually happening is that the Scala compiler dynamically generates a class name (which in this case is `AnonymousClasses$$anon$1`) with the class body specified above, then instantiates it:

```scala
// what it actually does behind the scenes
class AnonymousClasses$$anon$1 extends Animal {
  override def speak = "Hi, I am a mystery animal"
}

val mysteryAnimal = new AnonymousClasses$$anon$1

// <rest of code here>
```

This should be pretty familiar to devs of JVM languages (like Java/Kotlin)

## Traits

Abstract classes are considered abstract if at least one member of the class is missing an implementation. Traits operate the same way except instead of using `extends` keyword, you would use the `with` keyword

**NOTE to Java devs**: [From Scala 2.12 onward, Traits compile down to interfaces so they can be thought of as the same thing](https://www.scala-lang.org/news/2.12.0-RC1/#trait-compiles-to-an-interface)

```scala
trait Carivore {
  def eats(animal: Animal)
  val lovesMeat = true
}

trait Hairy {
  def furThickness(): Int
}

class Cat extends Animal with Carnivore with Hairy {
  def eats(animal: Animal) = println("Eating an animal. :D")    
  def furThickness() = 3
}
```

### Traits vs Abstract Classes

There are a few key differences between traits and abstract classes:

1. Multiple Traits can be used with 1 class unlike abstract classes which is restricted to just 1. 
2. Traits do NOT have constructors while abstract classes do 
3. Traits describe _behavior_ while abstract classes are generally "things"

## `???` method

Scala has a method called `???` that can be used when you are marking some method as "not yet implemented".

This is useful if you have methods in a class that aren't yet implemented. In other languages like JavaScript, you would have a `// TODO: STUB` comment above some method or even throw an Exception. In Scala, you use use `???`:

```scala
object Foo {
  def bar(): Int = ???
}
```

In fact, if you actually go to the built-in class, `Predef.scala` you can actually see the method definition as:

```scala
  /** `???` can be used for marking methods that remain to be implemented.
   *  @throws NotImplementedError when `???` is invoked.
   *  @group utilities
   */
  def ??? : Nothing = throw new NotImplementedError
```

Note how it uses the `Nothing` type. That's because in Scala everything requires a type but in this case, `Nothing` is appropriate because its the subtype of all types and so can be used consistently without breaking the type the method might expect. [This explains it best](https://stackoverflow.com/questions/35848904/understanding-of-scala-nothing-type)
