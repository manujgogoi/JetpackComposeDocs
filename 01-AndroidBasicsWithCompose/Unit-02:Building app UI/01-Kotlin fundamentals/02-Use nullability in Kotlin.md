# Use nullability in Kotlin

- Nullable types are variables that can hold `null`.
- Non-null types are variables that can't hold `null`.

```kt
fun main() {
    var favoriteActor: String = "Sandra Oh"
    favoriteActor = null // Error: Null can not be a value of a non-null type String
}
```

- A type is only nullable if you explicitly let it hold `null`. As the error message says, the String data type is a non-nullable type, so you can't reassign the variable to `null`.

```kt
fun main() {
    var favoriteActor: String? = "Sandra Oh" // Nullable type
    favoriteActor = null
}
```

Nullable Integer

```kt
fun main() {
    var number: Int? = 10
    println(number)

    number = null
}
```

## Handle nullable variables

```kt
fun main() {
    var favoriteActor: String = "Sandra Oh"
    println(favoriteActor.length) // Access non-nullable property
}
```

```kt
fun main() {
    var favoriteActor: String? = "Sandra Oh"
    println(favoriteActor.length) // Access nullable property [ Compile Error]
}
```

### Use the `?.` safe call operator

- You can use the ?. safe call operator to access methods or properties of nullable variables.

```kt
fun main() {
    var favoriteActor: String? = "Sandra Oh"
    println(favoriteActor?.length)
}
```

Output:

```
9
```

```kt
fun main() {
    var favoriteActor: String? = null
    println(favoriteActor?.length)
}
```

Output:

```
null
```

### Use the `!!` not-null assertion operator

```kt
fun main() {
    var favoriteActor: String? = "Sandra Oh"
    println(favoriteActor!!.length)
}
```

Output

```
9
```

```kt
fun main() {
    var favoriteActor: String? = null
    println(favoriteActor!!.length)
}
```

_You get a `NullPointerException` error:_

### Use the `?:` Elvis operator

- The `?:` Elvis operator is an operator that you can use together with the `?.` safe-call operator.
- With the `?:` Elvis operator, you can add a default value when the `?.` safe-call operator returns `null`.

```kt
fun main() {
    var favoriteActor: String? = "Sandra Oh"

    val lengthOfName = favoriteActor?.length ?: 0

    println("The number of characters in your favorite actor's name is $lengthOfName.")
}
```
