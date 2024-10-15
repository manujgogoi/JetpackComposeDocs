# Create an Art Space App

Import 2 images in drawable directory

1. `eiffel_tower.jpeg`
2. `taj_mahal.jpeg`

File: `app/res/values/strings.xml`

```xml
<resources>
    <string name="app_name">Art Space</string>
    <string name="eiffel_tower">Eiffel Tower</string>
    <string name="taj_mahal">Taj Mahal</string>
    <string name="eiffel_tower_artist">John Doe</string>
    <string name="eiffel_tower_year">2020</string>
    <string name="taj_mahal_artist">Raj Malhotra</string>
    <string name="taj_mahal_year">2021</string>
</resources>
```

File: `MainActivity.kt`

```kt
package com.example.artspace

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.annotation.DrawableRes
import androidx.annotation.StringRes
import androidx.compose.foundation.Image
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.width
import androidx.compose.foundation.rememberScrollState
import androidx.compose.foundation.verticalScroll
import androidx.compose.material3.Button
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableIntStateOf
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.artspace.ui.theme.ArtSpaceTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            ArtSpaceTheme {
                ArtSpaceLayout()
            }
        }
    }
}

@Composable
fun ArtSpaceLayout() {
    var currentArt by remember { mutableIntStateOf(1) }

    Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
        Surface (
            modifier = Modifier.padding(innerPadding)
        ) {
            if (currentArt == 1) {
                ArtSpaceScreen(
                    image = R.drawable.taj_mahal,
                    title = R.string.taj_mahal,
                    artist = R.string.taj_mahal_artist,
                    year = R.string.taj_mahal_year,
                    onPreviousClick = {},
                    onNextClick = {currentArt = 2},
                )
            } else {
                ArtSpaceScreen(
                    image = R.drawable.eiffel_tower,
                    title = R.string.eiffel_tower,
                    artist = R.string.eiffel_tower_artist,
                    year = R.string.eiffel_tower_year,
                    onPreviousClick = {currentArt = 1},
                    onNextClick = {},
                )
            }

        }
    }
}

@Composable
fun ArtSpaceScreen(
    @DrawableRes image: Int,
    @StringRes title: Int,
    @StringRes artist: Int,
    @StringRes year: Int,
    onPreviousClick: () -> Unit,
    onNextClick: () -> Unit,
) {
    Column (
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center,
        modifier = Modifier
            .fillMaxSize()
            .verticalScroll(rememberScrollState()),
    ) {
        Image(
            painter = painterResource(image),
            contentDescription = stringResource(title),
            contentScale = ContentScale.FillWidth,
            modifier = Modifier.width(300.dp)
        )
        Spacer(modifier = Modifier.height(30.dp))
        Text(stringResource(title), style = MaterialTheme.typography.displaySmall)
        Row {
            Text(stringResource(artist), style = MaterialTheme.typography.titleLarge)
            Spacer(modifier = Modifier.width(5.dp))
            Text("(${stringResource(year)})", style = MaterialTheme.typography.titleLarge)
        }
        Spacer(modifier = Modifier.height(10.dp))
        Row (
            horizontalArrangement = Arrangement.SpaceAround,
            modifier = Modifier.fillMaxWidth()
        ) {
            Button(
                onClick = onPreviousClick
            ) {
                Text("Previous")
            }
            Button(
                onClick = onNextClick
            ) {
                Text("Next")
            }
        }

    }
}

@Preview(showBackground = true)
@Composable
fun ArtSpacePreview() {
    ArtSpaceTheme {
        ArtSpaceLayout()
    }
}
```
