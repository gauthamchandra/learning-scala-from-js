# Generics

For TypeScript developers, Generics, on a basic level, work the same way as in TypeScript.

For regular JS devs who aren't familiar with TypeScript, then here's a basic primer on what generics are.

## Primer on Generics:

Say we have these 3 basic classes like so:

```scala
class Fruit
class Banana extends Fruit
class Mango extends Fruit
```

Pretty straightforward and let's say we want to have a basket that holds fruit

```scala
class Basket {
  def hold(fruit: Fruit) = ???
}

val basket = new Basket
basket.hold(new Fruit)
```

This is all well and good but what if we wanted to be more specific in what the basket holds. What if we wanted to say that the basket can _only_ hold mangos or _only_ hold bananas?

We _could_ do this:

```scala
// BAD. This duplicates code!
class BananaBasket
class MangoBasket
```

Enter Generics!

By using `[]`, we can specify a generic type in the definition using a letter/name:

```scala
class Basket[A] {
  def hold(fruit: A) = ???
}

val fruitBasket = new Basket[Fruit]
val mangoBasket = new Basket[Mango] 
val bananaBasket = new Basket[Banana]
```

This tells the compiler that there's some type (known for now as A), that you will specify the value of later during instance creation. Then, when the `fruitBasket`, `mangoBasket, and `bananaBasket` variables are created, an explicit, concrete type is supplied for `A` (which is `Fruit`, `Mango`, and `Banana` respectively).

This allows us to reuse code without defining multiple versions of the same `Basket` class.

### Generics on Methods

You can also use generics on method definitions (and not just classes):

```scala
object BasketMaker {
  def createBasket[A] = new Basket[A]
}

val fruitBasket: Basket[Fruit] = BasketMaker.createBasket[Fruit]
```

## Variances

Variances are basically ways to tie complex types together using generics.

### Covariance

TBD
