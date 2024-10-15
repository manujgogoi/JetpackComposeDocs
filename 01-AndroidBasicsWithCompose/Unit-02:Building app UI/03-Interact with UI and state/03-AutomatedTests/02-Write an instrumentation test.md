# Write an instrumentation test

## Create the instrumentation directory

1. Right click on the **src** directory and select **New > Directory**.
2. In the **New Directory** window select `androidTest/java`.
3. Right click on the **androidTest/java** folder and select **New > Package**.
4. In the **New Package** window, type `com.example.tiptime`.

## Create the test class

1. Navigate to the `androidTest` directory in the project pane.
2. Right-click the `tiptime` directory and then select **New > Kotlin Class/File**.
3. Enter `TipUITests` as the class name.

## Write the test

> Create a `composeTestRule` variable set to the result of the `createComposeRule()` method and annotate it with the `Rule` annotation:

```kt
package com.example.tiptime

import androidx.compose.ui.test.junit4.createComposeRule
import androidx.compose.ui.test.onNodeWithText
import androidx.compose.ui.test.performTextInput
import com.example.tiptime.ui.theme.TipTimeTheme
import org.junit.Rule
import org.junit.Test
import java.text.NumberFormat

@Test
fun calculate_20_percent_tip() {
    composeTestRule.setContent {
        TipTimeTheme {
            Surface (modifier = Modifier.fillMaxSize()){
                TipTimeLayout()
            }
        }
    }
   composeTestRule.onNodeWithText("Bill Amount")
      .performTextInput("10")
   composeTestRule.onNodeWithText("Tip Percentage").performTextInput("20")
   val expectedTip = NumberFormat.getCurrencyInstance().format(2)
   composeTestRule.onNodeWithText("Tip Amount: $expectedTip").assertExists(
      "No node with this text was found."
   )
}
```
