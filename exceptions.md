# Exceptions

Throwing exceptions work the same way as in JavaScript:

```javascript
// in JS
throw new Error('Some stuff happened');
```

```scala
// in scala
throw new Exception("Some stuff happened") 
```

## `Exception`s vs `Error`s

In JVM languages (Java, Scala, Kotlin, etc.), all errors that can be thrown in your application implement a `Throwable` Trait/Interface.

* Throwable - represents anything that can be thrown using the `throw` keyword.
  * Exception - Used for when the application should generally try to be handling an error (e.g. when some variable is `null` aka `NullPointerException`)
  * Error - Used for when something in the system went seriously wrong and should/cannot be handled (like if the system ran out of memory aka. `OutofMemoryError`)

## Catching Exceptions

Catching exceptions work in a similar way as in JavaScript but with 1 small catch (no pun intended):

```scala
def someRiskyOperation(number: Int) = {
  if (number == 1) {
    throw new RuntimeException("Stuff!")
  }
}

// as many things in Scala, this try-catch actually returns a value
val tryCatchValue = try {
  someRiskyOperation(1)
} catch {
  // because try catch blocks can catch multiple exceptions, case statements are used!
  case e: RuntimeException => {
    println("Caught exception. Returning 29")
    29
  }
} finally {
  println("Running finally block")
}

println(tryCatchValue)

// Returns:
// => Caught exception. Returning 29
// => Running finally block
// => 29
```

**NOTE**: Whatever is implicitly returned in the `finally` block, doesn't affect the value of the try-catch. In other words, in the code above `tryCatchValue` ignores the result of `finally` (which is implicitly of `Unit` type because of `println`

## Custom Exceptions

This is as simple as saying:

```scala
class MyCustomException extends Exception
```
