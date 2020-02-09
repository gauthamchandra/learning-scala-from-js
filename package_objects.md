# Package Objects

This is going to skip introductions on what packages are, how they are organized and how to import them. See [the official docs for a good intro](https://docs.scala-lang.org/tour/packages-and-imports.html)

Package objects are special object classes that sit in a `package.scala` file inside the package.

* There can only be 1 package object per package.
* It must be in a `package.scala` file. 
* Useful for storing constants or methods that are going to be shared in the rest of the package.
* The package object name is the name of the package.

```scala
package some.foo

package object foo {
  // anthing defined here can be referenced in any class in the package without explicit imports.
  val SOME_CONSTANT = 39
}
```

 
