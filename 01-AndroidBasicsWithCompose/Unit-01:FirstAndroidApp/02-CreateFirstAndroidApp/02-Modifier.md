# Modifier

- A **`Modifier`** is used to augment or decorate a composable
- One modifier you can use is the **padding** modifier

```kt
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = Color.Cyan) {
        Text(
            text = "Hi, my name is $name!",
            modifier = modifier.padding(24.dp)
        )
    }
}
```

- Add these imports to the import statement section.

```kt
import androidx.compose.ui.unit.dp
import androidx.compose.foundation.layout.padding
```
