# Create A project using the Template

1. Open Android Studio
2. **`New Project`**
3. Make sure the **`Phone and Tablet`** tab is selected.
4. **`Empty Activity`**
5. Click **`Next`**
6. Configure your project as follows:
   - `Name` Field: name of your project
   - Leave the `Package name` field as is
   - Leave the `Save location` field as is.
   - Select **`API 24: Android 7.0 (Nougat)`** from the menu in the `Minimum SDK` field.
7. Click Finish.

## Update the default code

```kt
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            GreetingCardTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    Greeting("Android")
                }
            }
        }
    }
}
```

- The **`onCreate()`** function is the entry point to this Android app and calls other functions to build the user interface.
- The **`setContent()`** function within the **`onCreate()`** function is used to define your layout through **composable functions**.
- All functions marked with the **`@Composable`** annotation can be called from the **`setContent()`** function or from other **Composable functions**.
- **`@Composable`** function names are capitalized.
- **`@Composable`** functions can't return anything.

## Preview

- To enable a preview of a composable, annotate with **`@Composable`** and **`@Preview`**.
- The **`@Preview`** annotation tells Android Studio that this composable should be shown in the design view of this file.

```kt
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    GreetingCardTheme {
        Greeting("Meghan")
    }
}
```

- If `showBackground` parameter is set to `true`, it will add a background to your **composable preview**.
