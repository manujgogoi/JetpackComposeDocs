# Translation

- Hardcoded strings make it difficult to translate your app into other languages and harder to reuse strings in different places in your app.
- You can extract strings into a resource file to resolve these issues.

1. In `MainActivity.kt` file select any hardcoded text string.
2. Click the bulb on the left side of the screen.
3. Select **Extract string resource**.
4. In **Extract Resource** dialog enter the `Resource name` and the `Resource value` fields.
5. Click **OK**.

```kt
GreetingImage(
    // Replaced the hardcoded string
    message = stringResource(R.string.happy_birthday_text),
    from = stringResource(R.string.signature_text)
    modifier = Modifier.padding(8.dp)
)
```
