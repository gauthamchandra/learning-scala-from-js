# Primitive Types and other Basics:

## Semicolons

Semicolons are not necessary in Scala and are discouraged unless you want to define multiple statements of code onto 1 line (but this is considered a bad practice).

## Declaring Variables

To declare a variable, use:

```scala
var x = 3 // same as JS here, "var" indicates a mutable, variable.
var y: Int = 3 // in Scala, you can explicitly give a type to a variable (like in Flow/TypeScript)

x = 5 // no error
y = 5 // no error
y = "Hello" // <= Error because we told the compiler that y can only be an integer! 
```

In the example above, the declarations for `x` and `y` are functionally equivalent but specifying a type tells the compiler that it can _only_ be of that type. If you do not specify a type, the compiler will try to infer the type (like in TypeScript or Flow).


If you want to ensure it is a constant:

```scala
val x = 3 // equivalent to JS's "const x = 5" 

x = 5 // <= This will throw an error by the compiler as you cannot change the value
```

## The Primitive Types:

They are `String`, `Char`, `Boolean`, `Double`, `Float`, `Int`, `Short`, `Long`, `Byte`. 

```scala
val str: String = "Foo" // Same as JS's "String" type. Strings are ALWAYS double quoted.
val c: Char = 'C' 			// Represents a single character in a string. Chars are ALWAYS single quoted.
val b: Boolean = false 	// Same as JS's "Boolean" type
val d: Double = 323.489	// 64 bit floating point number. Similar to JS's Number. 
val f: Float = 439.4f		// 32 bit signed integer. Similar to JS's Number but can't hold very big numbers.
												// Note the 'f' that is at the end. This tells the compiler its a float. Otherwise, its treated as a double!
val i: Int = 5					// 32 bit signed integer. similar to JS's "Number" type if it only supported whole numbers.
val s: Short = 489			// similar to Int but is only 16 bit signed value so its meant to hold only small numbers. 
val l: Long = 48949483L	// similar to Int but is 64 bit signed integer so it can hold a very large number.
												// Note the 'L' suffixed at the end. This tells the compiler its a long. Otherwise, its treated as an int!
val by: Byte = 3					// 8 bit signed integer. No real equivalent in JS. Not often used but useful when trying to save memory. 
```

There are also `Any`, `AnyRef`, `AnyVal`, `Nothing`, `Null`, `null`, and `Unit` but these are better explained in subsequent sections. 

For full details, please see [the official docs](https://docs.scala-lang.org/tour/unified-types.html) as well as the [Java Primitive types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html) (as Scala runs on the JVM and therefore supports the types found in Java)

## Basic Operations

### String Interpolation

In Javascript, you would use the backtick "`" instead of `'` or `"` like so:

```javascript
const num = 3;

const str = `Billy kicked the ball ${num} times`;
```

In Scala, there are 3 ways to interpolate strings:

* the `s` formatter (aka. standard string interplation)
* the `f` formatter (aka. the "printf" style string interpolation with type enforcement)
* the `raw` formatter (aka. the same as the `s` formatter but doesn't escape anything)

```scala
val num = 3

s"Billy kicked the ball\n$num times"       // => "Billy kicked the ball
                                           //     3 times"
f"Billy kicked the ball\n$num%d times"     // => "Billy kicked the ball
                                           //     3 times"
raw"Billy kicked the ball\n$num times"     // => "Billy kicked the ball\n3 times"
```

For more details, see the [official docs](https://docs.scala-lang.org/overviews/core/string-interpolation.html)

### Arrays 

**IMPORTANT**: In Javascript, arrays aren't exactly "just" arrays. In traditional languages like Java or Scala, Arrays are fixed length so adding to the array invovles:

1. creating a new, bigger array,
2. copying all the old values to the new array
3. and then setting the new value in the right index of the new array.

This means creating a new array is traditionally an O(n) operation.

By Java/Scala standards, Javascript cheats by making an Array that is _technically_ ALL of the following:

* a [Linked List](https://en.wikipedia.org/wiki/Linked_list) - because it dynamically grows the array for you when you add to it)
* a [Stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) - due to its `push`/`pop` methods.
* a [Queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) - as you can `push` new items and then `shift()` them from the front,

Each of these individual types is available in Scala as well as the traditional "Array". These notes won't delve into how to use each one and I recommend reading the Scala docs for that.

#### Basic Operations

```scala
val arr: Array = Array(1, 2, 3) // equivalent to "const arr = [1, 2, 3];" in JS
val list: List = List(1, 2, 3)  // equivalent to "const list = [1, 2, 3];" in JS

List.fill(3)("Hello") // equivalent to: List("Hello", "Hello", "Hello")

list.isEmpty      // => false 
list.head         // => 1
list.tail         // => 3

list :+ 11        // => List(1, 2, 3, 11). The result is a new list, the original is unmodified.
                  // Equivalent to [...list, 11] in JS

25 +: list        // => List(25, 1, 2, 3). The result is a new list, the original is unmodified
                  // Equivalent to [25, ...list]  

list.contains(4)  // => false

list(2)           // => 3. Equivalent to list[2]
```

So a few things to note here:

* You might have noticed that **accessing arrays and lists utilize parentheses, NOT brackets**. This is because, just like in the Ruby programming language, _EVERYTHING_ in Scala is an object.  So when you want to access the 3rd item in a list, you use parentheses because you are _literally_ calling a method to access the value.
* [What's the difference between Array and List?](https://stackoverflow.com/questions/2712877/difference-between-array-and-list-in-scala)

#### Mutable Lists through `ArrayBuffer` and `ListBuffer`

`List`s are **IMMUTABLE** in Scala. Methods invoked on a List to modify it will return a new list. The original is always unchanged! 

If we need a mutable list, then an `ArrayBuffer` or `ListBuffer` should be used.
