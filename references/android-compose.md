# Android (Jetpack Compose) Reference

## Project Structure

```
app/src/main/
├── java/com/example/app/
│   ├── MainActivity.kt
│   ├── ui/
│   │   ├── components/           # Reusable composables
│   │   │   ├── AppButton.kt
│   │   │   ├── AppCard.kt
│   │   │   ├── AppTextField.kt
│   │   │   └── ...
│   │   ├── screens/              # Full screens
│   │   │   ├── HomeScreen.kt
│   │   │   ├── DetailScreen.kt
│   │   │   └── ...
│   │   ├── navigation/
│   │   │   └── AppNavigation.kt
│   │   └── theme/
│   │       ├── Color.kt
│   │       ├── Type.kt
│   │       ├── Shape.kt
│   │       ├── Spacing.kt
│   │       └── Theme.kt
│   ├── viewmodels/
│   │   ├── HomeViewModel.kt
│   │   └── ...
│   ├── data/
│   │   ├── models/
│   │   └── repository/
│   └── utils/
└── res/
    ├── drawable/
    ├── font/
    ├── values/
    │   ├── colors.xml
    │   └── strings.xml
    └── ...
```

## Component Templates

### Button Component
```kotlin
// ui/components/AppButton.kt
package com.example.app.ui.components

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import com.example.app.ui.theme.*

enum class ButtonVariant { Primary, Secondary, Ghost }
enum class ButtonSize { Small, Medium, Large }

@Composable
fun AppButton(
    text: String,
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
    variant: ButtonVariant = ButtonVariant.Primary,
    size: ButtonSize = ButtonSize.Medium,
    enabled: Boolean = true
) {
    val height = when (size) {
        ButtonSize.Small -> 36.dp
        ButtonSize.Medium -> 44.dp
        ButtonSize.Large -> 52.dp
    }
    
    val colors = when (variant) {
        ButtonVariant.Primary -> ButtonDefaults.buttonColors(
            containerColor = AppColors.Primary,
            contentColor = AppColors.OnPrimary
        )
        ButtonVariant.Secondary -> ButtonDefaults.buttonColors(
            containerColor = AppColors.Secondary,
            contentColor = AppColors.OnSecondary
        )
        ButtonVariant.Ghost -> ButtonDefaults.textButtonColors(
            contentColor = AppColors.Primary
        )
    }
    
    if (variant == ButtonVariant.Ghost) {
        TextButton(
            onClick = onClick,
            modifier = modifier.height(height),
            enabled = enabled
        ) {
            Text(text)
        }
    } else {
        Button(
            onClick = onClick,
            modifier = modifier.height(height),
            colors = colors,
            enabled = enabled,
            shape = MaterialTheme.shapes.medium
        ) {
            Text(text)
        }
    }
}
```

### Card Component
```kotlin
// ui/components/AppCard.kt
package com.example.app.ui.components

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import com.example.app.ui.theme.*

@Composable
fun AppCard(
    modifier: Modifier = Modifier,
    content: @Composable ColumnScope.() -> Unit
) {
    Card(
        modifier = modifier,
        colors = CardDefaults.cardColors(
            containerColor = AppColors.Surface
        ),
        elevation = CardDefaults.cardElevation(
            defaultElevation = 2.dp
        ),
        shape = MaterialTheme.shapes.large
    ) {
        Column(
            modifier = Modifier.padding(Spacing.md),
            content = content
        )
    }
}
```

## Theme Setup

```kotlin
// ui/theme/Color.kt
package com.example.app.ui.theme

import androidx.compose.ui.graphics.Color

object AppColors {
    val Primary = Color(0xFF4A90E2)
    val OnPrimary = Color.White
    val Secondary = Color(0xFF7B61FF)
    val OnSecondary = Color.White
    val Accent = Color(0xFFFF6B6B)
    
    val Background = Color(0xFFFFFFFF)
    val Surface = Color(0xFFF5F5F5)
    val OnBackground = Color(0xFF111111)
    val OnSurface = Color(0xFF111111)
    
    val TextPrimary = Color(0xFF111111)
    val TextSecondary = Color(0xFF666666)
    val TextMuted = Color(0xFF999999)
    
    // Dark theme variants
    val DarkBackground = Color(0xFF121212)
    val DarkSurface = Color(0xFF1E1E1E)
}

// ui/theme/Type.kt
package com.example.app.ui.theme

import androidx.compose.material3.Typography
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.sp

val AppTypography = Typography(
    displayLarge = TextStyle(
        fontSize = 34.sp,
        fontWeight = FontWeight.Bold,
        lineHeight = 40.sp
    ),
    headlineLarge = TextStyle(
        fontSize = 28.sp,
        fontWeight = FontWeight.Bold,
        lineHeight = 34.sp
    ),
    headlineMedium = TextStyle(
        fontSize = 22.sp,
        fontWeight = FontWeight.SemiBold,
        lineHeight = 28.sp
    ),
    titleLarge = TextStyle(
        fontSize = 20.sp,
        fontWeight = FontWeight.SemiBold,
        lineHeight = 26.sp
    ),
    bodyLarge = TextStyle(
        fontSize = 16.sp,
        fontWeight = FontWeight.Normal,
        lineHeight = 24.sp
    ),
    bodyMedium = TextStyle(
        fontSize = 14.sp,
        fontWeight = FontWeight.Normal,
        lineHeight = 20.sp
    ),
    labelLarge = TextStyle(
        fontSize = 14.sp,
        fontWeight = FontWeight.Medium,
        lineHeight = 20.sp
    )
)

// ui/theme/Spacing.kt
package com.example.app.ui.theme

import androidx.compose.ui.unit.dp

object Spacing {
    val xs = 4.dp
    val sm = 8.dp
    val md = 16.dp
    val lg = 24.dp
    val xl = 32.dp
    val xxl = 48.dp
}

object Radius {
    val sm = 4.dp
    val md = 8.dp
    val lg = 16.dp
    val xl = 24.dp
}

// ui/theme/Theme.kt
package com.example.app.ui.theme

import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.material3.*
import androidx.compose.runtime.Composable

private val LightColorScheme = lightColorScheme(
    primary = AppColors.Primary,
    onPrimary = AppColors.OnPrimary,
    secondary = AppColors.Secondary,
    onSecondary = AppColors.OnSecondary,
    background = AppColors.Background,
    onBackground = AppColors.OnBackground,
    surface = AppColors.Surface,
    onSurface = AppColors.OnSurface
)

private val DarkColorScheme = darkColorScheme(
    primary = AppColors.Primary,
    onPrimary = AppColors.OnPrimary,
    secondary = AppColors.Secondary,
    onSecondary = AppColors.OnSecondary,
    background = AppColors.DarkBackground,
    onBackground = Color.White,
    surface = AppColors.DarkSurface,
    onSurface = Color.White
)

@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) DarkColorScheme else LightColorScheme
    
    MaterialTheme(
        colorScheme = colorScheme,
        typography = AppTypography,
        content = content
    )
}
```

## Navigation

```kotlin
// ui/navigation/AppNavigation.kt
package com.example.app.ui.navigation

import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.navigation.NavHostController
import androidx.navigation.compose.*

sealed class Screen(val route: String, val title: String, val icon: ImageVector) {
    object Home : Screen("home", "Home", Icons.Default.Home)
    object Search : Screen("search", "Search", Icons.Default.Search)
    object Profile : Screen("profile", "Profile", Icons.Default.Person)
}

@Composable
fun AppNavigation(navController: NavHostController = rememberNavController()) {
    val items = listOf(Screen.Home, Screen.Search, Screen.Profile)
    
    Scaffold(
        bottomBar = {
            NavigationBar {
                val navBackStackEntry by navController.currentBackStackEntryAsState()
                val currentRoute = navBackStackEntry?.destination?.route
                
                items.forEach { screen ->
                    NavigationBarItem(
                        icon = { Icon(screen.icon, contentDescription = screen.title) },
                        label = { Text(screen.title) },
                        selected = currentRoute == screen.route,
                        onClick = {
                            navController.navigate(screen.route) {
                                popUpTo(navController.graph.startDestinationId) {
                                    saveState = true
                                }
                                launchSingleTop = true
                                restoreState = true
                            }
                        }
                    )
                }
            }
        }
    ) { innerPadding ->
        NavHost(
            navController = navController,
            startDestination = Screen.Home.route,
            modifier = Modifier.padding(innerPadding)
        ) {
            composable(Screen.Home.route) { HomeScreen(navController) }
            composable(Screen.Search.route) { SearchScreen(navController) }
            composable(Screen.Profile.route) { ProfileScreen(navController) }
        }
    }
}
```

## ViewModel Pattern

```kotlin
// viewmodels/HomeViewModel.kt
package com.example.app.viewmodels

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.*
import kotlinx.coroutines.launch

data class HomeUiState(
    val items: List<Item> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null
)

class HomeViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(HomeUiState())
    val uiState: StateFlow<HomeUiState> = _uiState.asStateFlow()
    
    fun loadItems() {
        viewModelScope.launch {
            _uiState.update { it.copy(isLoading = true) }
            try {
                val items = repository.getItems()
                _uiState.update { it.copy(items = items, isLoading = false) }
            } catch (e: Exception) {
                _uiState.update { it.copy(error = e.message, isLoading = false) }
            }
        }
    }
}

// In Screen:
@Composable
fun HomeScreen(
    navController: NavController,
    viewModel: HomeViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()
    
    LaunchedEffect(Unit) {
        viewModel.loadItems()
    }
    
    // UI...
}
```

## Resource Management

```
res/
├── drawable/
│   ├── ic_logo.xml          # Vector drawables
│   └── bg_gradient.xml
├── drawable-hdpi/           # Density-specific bitmaps
├── drawable-xhdpi/
├── drawable-xxhdpi/
├── font/
│   ├── inter_regular.ttf
│   └── inter_bold.ttf
└── values/
    ├── colors.xml           # Color resources (backup for XML views)
    └── strings.xml          # All user-facing strings
```
