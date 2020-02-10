# Functional Programming

## "Functions" in Scala

Scala runs on the JVM and the JVM was originally designed for Java, a language where functions aren't first class citizens. Because of this, functions don't exist outside of a class.

However, thanks to the syntactic sugar of the `apply()` method [discussed previously](classes.md#the-apply-method), we can call it like a function:


```scala
trait SomeFunction[A, B] {
  def apply(a: A): B
}

// implement the trait as an anonymous class 
val squared = new SomeFunction[Int, Int] {
  override def apply(a: Int): Int = a * a
}

// now that we have an apply method, we can call it like a function
squared(2) // => 4
```


### FunctionX classes in Scala Standard Library

If functions to be used require a class with an apply method, then that would normally force a developer to write lots of classes with different apply method signatures to suit a variety of use cases.

Mercifully, the Scala library provides classes like [Function1](https://www.scala-lang.org/api/current/scala/Function1.html), [Function2](https://www.scala-lang.org/api/current/scala/Function2.html), [Function3](https://www.scala-lang.org/api/current/scala/Function3.html), etc.

So using `Function1` class, we can slightly simplify the example: 

```scala
val squared = new Function1[Int, Int] {
  override def apply(num: Int) = num * num
}

squared(2) // => 4
```

Compared to a language like JavaScript where functions are first class citizens, this extra setup of having `Function` classes will feel _very_ "clunky".

### Function Object Shorthand via the "=>"

Let's simplify the previous code even more because Scala provides a convenient shorthand for implementing functions:

```scala
// long form
val squared = new Function1[Int, Int] {
  override def apply(num: Int) = num * num
}

// short form
val squared2 = new (Int => Int) {
  override def apply(num: Int) = num * num
}

// you can even write curried functions more easily with shorthand notation
val addFnGenerator = new (Int => ((Int) => Int)) {
  override def apply(num: Int) = new (Int => Int) {
    override def apply(num2: Int) = num + num2
  }
}

val add3To = addFnGenerator(3)

add3To(5) // => 3 + 5 = 8
```


