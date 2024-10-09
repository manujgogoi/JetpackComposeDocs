# Interact with UI and State

## Tip Time App

> The Tip Time app contains various UI elements for calculating a tip, teaching about user input, and State in Compose.

1. In Android Studio **New Project** -> **Empty Activity** -> Name field: **Tip Time** -> **Finish**

2. Remove Default Codes (`Greeting` Composable)

3. Add the following string resources to **res** > **values** > **strings.xml**

```xml
<resources>
   <string name="app_name">Tip Time</string>
   <string name="calculate_tip">Calculate Tip</string>
   <string name="bill_amount">Bill Amount</string>
   <string name="tip_amount">Tip Amount: %s</string>
</resources>
```

The `MainActivity.kt` file:

```kt

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        enableEdgeToEdge()
        super.onCreate(savedInstanceState)
        setContent {
            TipTimeTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                ) {
                    TipTimeLayout()
                }
            }
        }
    }
}

@Composable
fun TipTimeLayout() {
    Column(
        modifier = Modifier
            .statusBarsPadding()
            .padding(horizontal = 40.dp)
            .safeDrawingPadding(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            text = stringResource(R.string.calculate_tip),
            modifier = Modifier
                .padding(bottom = 16.dp, top = 40.dp)
                .align(alignment = Alignment.Start)
        )
        Text(
            text = stringResource(R.string.tip_amount, "$0.00"),
            style = MaterialTheme.typography.displaySmall
        )
        Spacer(modifier = Modifier.height(150.dp))
    }
}

/**
 * Calculates the tip based on the user input and format the tip amount
 * according to the local currency.
 * Example would be "$10.00".
 */
private fun calculateTip(amount: Double, tipPercent: Double = 15.0): String {
    val tip = tipPercent / 100 * amount
    return NumberFormat.getCurrencyInstance().format(tip)
}

@Preview(showBackground = true)
@Composable
fun TipTimeLayoutPreview() {
    TipTimeTheme {
        TipTimeLayout()
    }
}
```

### Add the TextField composable to the app:

```kt
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
   TextField(
      value = "",
      onValueChange = {},
      modifier = modifier
   )
}
```

Import `TextField` from:

```kt
import androidx.compose.material3.TextField
```

In the `TipTimeLayout()` composable, call the `EditNumberField()` function

```kt
import androidx.compose.foundation.layout.fillMaxWidth

@Composable
fun TipTimeLayout() {
   Column(
        modifier = Modifier
            .statusBarsPadding()
            .padding(horizontal = 40.dp)
            .verticalScroll(rememberScrollState())
            .safeDrawingPadding(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
   ) {
       Text(
           ...
       )
       EditNumberField(modifier = Modifier.padding(bottom = 32.dp).fillMaxWidth())
       Text(
           ...
       )
       ...
   }
}
```

### Use state in Compose

```kt
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
    val amountInput = "0"
    TextField(
        value = amountInput,
        onValueChange = {},
        modifier = modifier
    )
}
```

### The Composition

- You use the `State` and `MutableState` types in Compose to make state in your app observable, or tracked, by Compose.
- The `State` type is immutable, so you can only read the value in it, while the `MutableState` type is mutable.
- You can use the `mutableStateOf()` function to create an observable `MutableState`.

The value returned by the `mutableStateOf()` function:

- Holds state, which is the bill amount.
- Is mutable, so the value can be changed.
- Is observable, so Compose observes any changes to the value and triggers a recomposition to update the UI.

Change `val amountInput = "0"` to `var amountInput: MutableState<String> = mutableStateOf("0")`

```kt
import androidx.compose.runtime.MutableState
import androidx.compose.runtime.mutableStateOf

// var amountInput: MutableState<String> = mutableStateOf("0")
// or
var amountInput = mutableStateOf("0")
```

Update the `TextField` Composable

```kt
var amountInput = mutableStateOf("0")
...
TextField(
   value = amountInput.value,
   onValueChange = {},
   modifier = modifier
)
```

The `onValueChange` callback is triggered when the text box's input changes. In the lambda expression, the `it` variable contains the new value.

```kt
TextField(
    value = amountInput.value,
    onValueChange = { amountInput.value = it },
    modifier = modifier
)
```

### Use `remember` function to save state

Updated Code

```kt
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
   var amountInput by remember { mutableStateOf("") }
   TextField(
       value = amountInput,
       onValueChange = { amountInput = it },
       modifier = modifier
   )
}
```

### Modify the appearance

- Add `label`, `singleLine` and `keyboardOptions` parameters

```kt
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
    var amountInput by remember { mutableStateOf("") }
    TextField(
        value = amountInput,
        onValueChange = { amountInput = it },
        singleLine = true,
        label = { Text(stringResource(R.string.bill_amount)) },
        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
        modifier = modifier
    )
}
```

### Display the tip amount

Use this `calculateTip` function to calculate the tip amount:

```kt
private fun calculateTip(amount: Double, tipPercent: Double = 15.0): String {
    val tip = tipPercent / 100 * amount
    return NumberFormat.getCurrencyInstance().format(tip)
}
```

```kt
@Composable
fun EditNumberField(modifier: Modifier = Modifier) {
   var amountInput by remember { mutableStateOf("") }

   val amount = amountInput.toDoubleOrNull() ?: 0.0
   val tip = calculateTip(amount)

   TextField(
       value = amountInput,
       onValueChange = { amountInput = it },
       label = { Text(stringResource(R.string.bill_amount)) },
       modifier = Modifier.fillMaxWidth(),
       singleLine = true,
       keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
   )
}
```

### Show the calculated tip amount

To show the calculated `tip` amount to the `Text` composable (Last one) we need to **_hoist_** the state variables to the `TipTimeLayout` composable function

```kt
@Composable
fun TipTimeLayout() {
   var amountInput by remember { mutableStateOf("") }

   val amount = amountInput.toDoubleOrNull() ?: 0.0
   val tip = calculateTip(amount)

   Column(
       //...
   ) {
       //...
   }
}
```

Update the `EditNumberField` composable:

```kt
@Composable
fun EditNumberField(
    value: String,
    onValueChange: (String) -> Unit,
    modifier: Modifier = Modifier
) {
    ...
}
```

Use the Composable

```kt
EditNumberField(
   value = amountInput,
   onValueChange = { amountInput = it },
   modifier = Modifier
       .padding(bottom = 32.dp)
       .fillMaxWidth()
)
```

Update the `TextField`:

```kt
TextField(
   value = value,
   onValueChange = onValueChange,
   // Rest of the code
)
```

### Positional formatting

```xml
// In the res/values/strings.xml file
<string name="tip_amount">Tip Amount: %s</string>
```

```kt
Text(
   text = stringResource(R.string.tip_amount, tip), // Replaced "$0.00" with tip variable
   style = MaterialTheme.typography.displaySmall
)
```
