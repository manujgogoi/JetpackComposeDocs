# Calculate a Custom Tip

## Add necessary string resources

1. In the Project tab, click **res** > **values** > **strings.xml**.
   In between the **strings.xml** file's `<resources>` tags, add these string resources:

```xml
<string name="how_was_the_service">Tip Percentage</string>
<string name="round_up_tip">Round up tip?</string>
```

## Add a tip-percentage text field

### Make the EditNumberField() composable function reusable:

1. In the `MainActivity.kt` file in the `EditNumberField()` composable function's parameters, add a `label` string resource of `Int` type:

```kt
@Composable
fun EditNumberField(
    label: Int,
    value: String,
    onValueChanged: (String) -> Unit,
    modifier: Modifier = Modifier
)
```

> You should add `Modifier` as the first optional parameter after all required parameters.

2. In the function body, replace the hardcoded string resource ID with the `label` parameter:

```kt
@Composable
fun EditNumberField(
    //...
) {
     TextField(
         //...
         label = { Text(stringResource(label)) },
         //...
     )
}
```

3. To denote that the `label` parameter is expected to be a string resource reference, annotate the function parameter with the `@StringRes` annotation:

```kt
@Composable
fun EditNumberField(
    @StringRes label: Int,
    value: String,
    onValueChanged: (String) -> Unit,
    modifier: Modifier = Modifier
)
```

4. In the `TipTimeLayout()` composable function's `EditNumberField()` function call, set the label parameter to the `R.string.bill_amount` string resource:

```kt
EditNumberField(
    label = R.string.bill_amount,
    value = amountInput,
    onValueChanged = { amountInput = it },
    modifier = Modifier.padding(bottom = 32.dp).fillMaxWidth()
)
```

5. In the `TipTimeLayout()` composable function after the `EditNumberField()` function call, add another text field for the custom tip percentage. Make a call to the `EditNumberField()` composable function with these parameters:

```kt
EditNumberField(
    label = R.string.how_was_the_service,
    value = "",
    onValueChanged = { },
    modifier = Modifier.padding(bottom = 32.dp).fillMaxWidth()
)
```

6. At the top of the `TipTimeLayout()` composable function, add a var property called `tipInput` for the added text field's state variable.

```kt
var tipInput by remember { mutableStateOf("") }
```

7. In the new `EditNumberField()` function call, set the value named parameter to the `tipInput` variable and then update the `tipInput` variable in the `onValueChanged` lambda expression:

```kt
EditNumberField(
    label = R.string.how_was_the_service,
    value = tipInput,
    onValueChanged = { tipInput = it },
    modifier = Modifier.padding(bottom = 32.dp).fillMaxWidth()
)
```

8. Define `tipPercent` and `tip` variables:

```kt
val tipPercent = tipInput.toDoubleOrNull() ?: 0.0
val tip = calculateTip(amount, tipPercent)
```

9. Run the app

### Set an action button

See the `keyboardOptions` parameter:

```kt
TextField(
        value = value,
        onValueChange = onValueChange,
        singleLine = true,
        label = { Text(stringResource(label)) },
        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
        modifier = modifier
    )
```

```kt
import androidx.compose.ui.text.input.ImeAction


@Composable
fun EditNumberField(
    //...
) {
    TextField(
        //...
        keyboardOptions = KeyboardOptions.Default.copy(
            keyboardType = KeyboardType.Number,
            imeAction = ImeAction.Next
        )
    )
}
```

imeAction options:

1. `ImeAction.Search` - Used when the user wants to execute a search.
2. `ImeAction.Send` - Used when the user wants to send the text in the input field.
3. `ImeAction.Go` - Used when the user wants to navigate to the target of the text in the input.
4. `ImeAction.Next` - The user is done with the current input and wants to move to the next text box.
5. `ImeAction.Done` - The user finished providing input.

6. Examine the `EditNumberField()` function.

```kt
// No need to copy, just examine the code.
fun EditNumberField(
    @StringRes label: Int,
    value: String,
    onValueChanged: (String) -> Unit,
    modifier: Modifier = Modifier
) {
    TextField(
        //...
        keyboardOptions = KeyboardOptions.Default.copy(
           keyboardType = KeyboardType.Number,
           imeAction = ImeAction.Next
        )
    )
}
```

2. In the `EditNumberField()` function definition, add a `keyboardOptions` parameter of type `KeyboardOptions` type. In the function body, assign it to the `TextField()` function's `keyboardOptions` named parameter:

```kt
@Composable
fun EditNumberField(
    @StringRes label: Int,
    keyboardOptions: KeyboardOptions,
    // ...
){
    TextField(
        //...
        keyboardOptions = keyboardOptions
    )
}
```

Update the first `EditNumberField()` function call:

```kt
EditNumberField(
    label = R.string.bill_amount,
    keyboardOptions = KeyboardOptions.Default.copy(
        keyboardType = KeyboardType.Number,
        imeAction = ImeAction.Next
    ),
    // ...
)
```

Update the second `EditNumberField()` function call:

```kt
EditNumberField(
    label = R.string.how_was_the_service,
    keyboardOptions = KeyboardOptions.Default.copy(
        keyboardType = KeyboardType.Number,
        imeAction = ImeAction.Done
    ),
    // ...
)
```

### Add a switch

1. After the `EditNumberField()` function, add a `RoundTheTipRow()` composable function:

```kt
@Composable
fun RoundTheTipRow(modifier: Modifier = Modifier) {
}
```

2. Add a `Row` layout composable

```kt
Row(
   modifier = modifier
       .fillMaxWidth()
       .size(48.dp),
   verticalAlignment = Alignment.CenterVertically
) {
}
```

3. In the `Row` layout composable's lambda block, add a `Text` composable

```kt
Text(text = stringResource(R.string.round_up_tip))
```

4. After the `Text` composable, add a `Switch` composable, and pass a `checked` named parameter set it to `roundUp` and an `onCheckedChange` named parameter set it to `onRoundUpChanged`.

```kt
Switch(
    checked = roundUp,
    onCheckedChange = onRoundUpChanged,
)
```

5. In the `RoundTheTipRow()` function, add a `roundUp` parameter of `Boolean` type and an `onRoundUpChanged` lambda function that takes a `Boolean` and returns nothing:

````kt
@Composable
fun RoundTheTipRow(
    roundUp: Boolean,
    onRoundUpChanged: (Boolean) -> Unit,
    modifier: Modifier = Modifier
)

6. In the `Switch` composable:

```kt
Switch(
    modifier = Modifier
        .fillMaxWidth()
        .wrapContentWidth(Alignment.End),
    //...
)
````

7. In the `TipTimeLayout()` function, add a `var` variable for the `Switch` composable's state.

```kt
fun TipTimeLayout() {
    //...
    var roundUp by remember { mutableStateOf(false) }

    //...
    Column(
        ...
    ) {
      //...
   }
}
```

8. In the `TipTimeLayout()` function's Column block after the **Tip Percentage** text field. Call the `RoundTheTipRow()` function:

```kt
@Composable
fun TipTimeLayout() {
    //...

    Column(
        ...
    ) {
        Text(
            ...
        )
        Spacer(...)
        EditNumberField(
            ...
        )
        EditNumberField(
            ...
        )
        RoundTheTipRow(
             roundUp = roundUp,
             onRoundUpChanged = { roundUp = it },
             modifier = Modifier.padding(bottom = 32.dp)
         )
        Text(
            ...
        )
    }
}
```

9. Run the app.

10. Modify the `calculateTip()` function to accept a `Boolean` variable to **_round up_** the `tip` to the nearest integer:

```kt
private fun calculateTip(amount: Double, tipPercent: Double = 15.0, roundUp: Boolean): String {
    var tip = tipPercent / 100 * amount
    if (roundUp) {
        tip = kotlin.math.ceil(tip)
    }
    return NumberFormat.getCurrencyInstance().format(tip)
}
```

11. In the `TipTimeLayout()` function, update the `calculateTip()` function call and then pass in a `roundUp` parameter:

```kt
val tip = calculateTip(amount, tipPercent, roundUp)
```

### Add support for landscape orientation

1. Add `.verticalScroll(rememberScrollState())` to the `modifier` to enable the column to scroll vertically. The `rememberScrollState()` creates and automatically remembers the scroll state.

```kt
@Composable
fun TipTimeLayout() {
    // ...
    Column(
        modifier = Modifier
            .padding(40.dp)
            .verticalScroll(rememberScrollState()),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        //...
    }
}
```

### Add leading icon to text fields (optional)

1. Add another parameter to the `EditNumberField()` composable called `leadingIcon` of the type `Int`. Annotate it with `@DrawableRes`.

```kt
@Composable
fun EditNumberField(
    @StringRes label: Int,
    @DrawableRes leadingIcon: Int,
    keyboardOptions: KeyboardOptions,
    value: String,
    onValueChanged: (String) -> Unit,
    modifier: Modifier = Modifier
)
```

2. Import the following:

```kt
import androidx.annotation.DrawableRes
import androidx.compose.material3.Icon
```

3. Add the leading icon to the text field. The `leadingIcon` takes a composable, you will pass in the following `Icon` composable.

```kt
TextField(
    value = value,
    leadingIcon = { Icon(painter = painterResource(id = leadingIcon), null) },
    //...
)
```

4. Pass in the leading icon to the text fields.

```kt
EditNumberField(
    label = R.string.bill_amount,
    leadingIcon = R.drawable.money,
    // Other arguments
)
EditNumberField(
    label = R.string.how_was_the_service,
    leadingIcon = R.drawable.percent,
    // Other arguments
)
```

5. Run the app.
