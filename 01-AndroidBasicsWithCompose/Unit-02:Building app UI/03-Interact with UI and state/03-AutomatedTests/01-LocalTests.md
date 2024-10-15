# Write a Local Test

## Prepare the app code

- In the `MainActivity.kt` file on the line before the `calculateTip()` method, add the `@VisibleForTesting` annotation and change the visibility from `private` to `internal`:

```kt
@VisibleForTesting
internal fun calculateTip(amount: Double, tipPercent: Double = 15.0, roundUp: Boolean): String {
    var tip = tipPercent / 100 * amount
    if (roundUp) {
        tip = kotlin.math.ceil(tip)
    }
    return NumberFormat.getCurrencyInstance().format(tip)
}
```

This makes the method public, but indicates to others that it's only public for testing purposes.

## Create the test directory

1. In the **Project** Tab, change teh view to Project.
2. Right click on the **src** directory.
3. **New** -> **Directory**.
4. In the **New Directory** window, select `test/java`. (If directory not exist)

> The **test** directory requires a package structure identical to that of the `main` directory where your app code lives.
> In other words, just as your app code is written in the `main > java > com > example > tiptime` package, your local tests
> will be written in `test > java > com > example > tiptime`.

5. Right click on the **test/java** directory and select **New > Package**.
6. In the **New Package** window, type `com.example.tiptime`.

## Create the test class

1. Right click the `com.example.tiptime` directory and then select **New > Kotlin Class/File**.
2. Enter `TipCalculatorTests` as the class name.

## Write the test

```kt
import org.junit.Assert.assertEquals
import org.junit.Test
import java.text.NumberFormat

class TipCalculatorTests {

    @Test
    fun calculateTip_20PercentNoRoundup() {
        val amount = 10.00
        val tipPercent = 20.00
        val expectedTip = NumberFormat.getCurrencyInstance().format(2)
        val actualTip = calculateTip(amount = amount, tipPercent = tipPercent, false)
        assertEquals(expectedTip, actualTip)
    }
}
```
