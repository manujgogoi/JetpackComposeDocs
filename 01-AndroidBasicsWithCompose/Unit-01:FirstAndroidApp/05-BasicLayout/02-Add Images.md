# Add images to your Android app

1. In Android Studio, click **View** > **Tool Windows** > **Resource Manager** or click the **Resource Manager** tab next to the **Project** window.
2. Click **+ (Add resources to the module) > Import Drawables**.
3. In the file browser, select the image file that you downloaded and then click **Open**.
4. Android Studio shows you a preview of the image. Select **Density** from the **QUALIFIER TYPE** drop-down list.
5. Select **No Density** from the **VALUE** list.
6. Click **Next**.
7. Click **Import(C)**.

> Android Studio creates a **drawable-nodpi** folder and places your image in it.

## Add an Image composable

### Resources in Jetpack Compose

- Resources are the additional files and static content that your code uses, such as bitmaps, user-interface strings, animation instructions, and more.

### Accessing resources

- Resources can be accessed with resource IDs that are generated in your project's **`R`** class.

```kt
R.drawable.graphic
```

File: `MainActivity.kt`

```kt
//...
import androidx.compose.foundation.Image
//...

@Composable
fun GreetingImage(message: String, from: String, modifier: Modifier = Modifier) {
    val image = painterResource(R.drawable.androidparty)
    Box(modifier) {
        Image(
            painter = image,
            contentDescription = null
        )
        GreetingText(
            message = message,
            from = from,
            modifier = Modifier
                .fillMaxSize()
                .padding(8.dp)
        )
    }
}

```
