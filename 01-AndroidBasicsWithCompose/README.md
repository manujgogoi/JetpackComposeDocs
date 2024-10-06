# What is Jetpack Compose?

- Jetpack Compose is a modern toolkit for building Android UIs.
- Compose simplifies and accelerates UI development on Android with less code, powerful tools, and intuitive Kotlin capabilities.

## Composable functions

- Composable functions are the basic building block of a UI in Compose.
- A composable function:
  - Describes some part of your UI.
  - Doesn't return anything.
  - Takes some input and generates what's shown on the screen.

## Annotations

- Annotations are means of attaching extra information to code. This information helps tools like the Jetpack Compose compiler, and other developers understand the app's code.

- An annotation is applied by prefixing its name (the annotation) with the **`@`** character at the beginning of the declaration you are annotating.

```kt
// Example code, do not copy it over

@Json
val imgSrcUrl: String

@Volatile
private var INSTANCE: AppDatabase? = null
```

## Composable function names

The Compose function:

- MUST be a noun: **`DoneButton()`**
- NOT a verb or verb phrase: **`DrawTextField()`**
- NOT a nouned preposition: **`TextFieldWithLink()`**
- NOT an adjective: **`Bright()`**
- NOT an adverb: **`Outside()`**
- Nouns MAY be prefixed by descriptive adjectives: **`RoundIcon()`**
