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
y = "Hello" // <= Error because we told the commpiler that y can only be an integer! 
```

In the example above, the declarations for `x` and `y` are functionally equivalent but specifying a type tells the compiler that it can _only_ be of that type. If you do not specify a type, the compiler will try to infer the type (like in TypeScript or Flow).


If you want to ensure it is a constant:

```scala
val x = 3 // equivalent to JS's "const x = 5" 

x = 5 // <= This will throw an error by the compiler as you cannot change the value
```

## The Primitive Types:

They are `String`, `Char`, `Boolean`, `Double`, `Float`, `Int`, `Short`, `Long`, `Byte`. 

There are also `Any`, `AnyRef`, `AnyVal`, `Nothing`, `Null`, `null`, and `Unit` but these are better explained in subsequent sections. 

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

For full details, please see [the official docs](https://docs.scala-lang.org/tour/unified-types.html) as well as the [Java Primitive types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html) (as Scala runs on the JVM and therefore supports the same types as Java)

## String Interpolation

In Javascript, you would use the `\`` instead of `'` or `"` like so:

```javascript
const num = 3;

const str = `Billy kicked the ball ${num} times`;
```

In Scala, there are 3 ways to interpolate strings:

* the `s` formatter (aka. standard string interplation)
* the `f` formatter (aka. the "printf" style string interpolation)
* the `raw` formatter (aka. the same as the `s` formatter but doesn't escape anything)
* The old fashioned way using "+" operator.

```scala
val num = 3

s"Billy kicked the ball\n$num times"       // => "Billy kicked the ball
                                           //     3 times"
f"Billy kicked the ball\n$num%d times"     // => "Billy kicked the ball
                                           //     3 times"
raw"Billy kicked the ball\n$num times"     // => "Billy kicked the ball\n3 times"
```

For more details, see the [official docs](https://docs.scala-lang.org/overviews/core/string-interpolation.html)
