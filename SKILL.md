---
name: design-zip-to-app
description: Convert Stitch design export ZIPs (HTML/CSS/assets) into production-ready applications. Use this skill whenever the user uploads a design ZIP, mentions "Stitch export", "design to code", "convert design to app", "HTML to React/SwiftUI/Compose/Flutter", or wants to turn exported design files into a functional application. Also trigger when user mentions "design system extraction", "component generation from design", or has a ZIP containing HTML/CSS they want converted to native platform code. This skill handles Web (React/Next.js), iOS (SwiftUI), Android (Jetpack Compose), and Flutter — ask which platform if not specified.
---

# Design ZIP to App Generator

Convert Stitch design export ZIPs into fully functional, scalable applications with extracted design systems and reusable components.

## Supported Platforms

| Platform | Framework | Architecture | Styling |
|----------|-----------|--------------|---------|
| Web | React / Next.js | Component-based | Tailwind CSS |
| iOS | SwiftUI | MVVM | Native SwiftUI |
| Android | Jetpack Compose | MVVM + ViewModel | Material3 |
| Flutter | Flutter/Dart | Clean Architecture | ThemeData |

## Workflow

### Step 1: Gather Input

**Required:**
```
"Please upload your Stitch design export ZIP file (containing HTML, CSS, images, fonts)."
```

**Always ask:**
```
"Which platform do you want to generate for?"
- Web (React / Next.js)
- iOS (SwiftUI)  
- Android (Jetpack Compose)
- Flutter
```

```
"Where is your existing project located? (I'll place generated code there)"
```

**Optional preferences to ask about:**
- Architecture preference (if non-default)
- Dark mode support (Yes/No)
- Animation system (Framer Motion / Lottie / Native)
- Backend integration needs

### Step 2: Parse the ZIP

1. **Extract and analyze:**
   ```bash
   unzip -o <upload_path> -d /home/claude/design-parse/
   ```

2. **Inventory the contents:**
   - HTML files → Document structure
   - CSS files → Styles (global + component-level)
   - Assets → Images, SVGs, fonts

3. **Build mental model:**
   - Layout tree (DOM → abstract UI tree)
   - Style map (class → properties)
   - Asset manifest

4. **If anything is ambiguous or missing, ASK before proceeding:**
   - Missing fonts → "I found references to `CustomFont` but no font files. Should I substitute with a system font?"
   - Unclear hierarchy → "This layout could be interpreted as tabs or a horizontal scroll. Which do you intend?"
   - Broken references → "Image `hero.png` is referenced but not found. Do you have it separately?"

### Step 3: Extract Design Tokens

Generate a comprehensive design system from the parsed CSS:

**Colors:**
```json
{
  "colors": {
    "primary": "#4A90E2",
    "secondary": "#7B61FF",
    "accent": "#FF6B6B",
    "background": "#FFFFFF",
    "surface": "#F5F5F5",
    "textPrimary": "#111111",
    "textSecondary": "#666666",
    "textMuted": "#999999"
  }
}
```

**Typography:**
```json
{
  "typography": {
    "fontFamily": {
      "primary": "Inter",
      "secondary": "Roboto Mono"
    },
    "fontSize": {
      "xs": 12, "sm": 14, "base": 16, "lg": 18, "xl": 24, "2xl": 32
    },
    "fontWeight": {
      "regular": 400, "medium": 500, "semibold": 600, "bold": 700
    },
    "lineHeight": {
      "tight": 1.2, "normal": 1.5, "relaxed": 1.75
    }
  }
}
```

**Spacing & Radius:**
```json
{
  "spacing": [4, 8, 12, 16, 24, 32, 48, 64],
  "radius": {
    "sm": 4, "md": 8, "lg": 16, "xl": 24, "full": 9999
  },
  "shadows": {
    "sm": "0 1px 2px rgba(0,0,0,0.05)",
    "md": "0 4px 6px rgba(0,0,0,0.1)",
    "lg": "0 10px 15px rgba(0,0,0,0.15)"
  }
}
```

### Step 4: Component Extraction (AGGRESSIVE)

**Extract ALL reusable patterns.** This is the core value of the skill.

**Detection rules:**
| Pattern | Component |
|---------|-----------|
| `<button>`, `.btn`, clickable styled elements | `Button` |
| Card-like containers with image + text | `Card` |
| `<input>`, `<textarea>`, form fields | `Input`, `TextArea` |
| Repeated list items | `ListItem` + `List` |
| Top bars with logo/nav | `Header`, `NavBar` |
| Bottom navigation | `TabBar`, `BottomNav` |
| Overlay containers | `Modal`, `Sheet` |
| Image + caption groups | `MediaBlock` |
| Icon + text combinations | `IconLabel` |
| Badge/tag elements | `Badge`, `Tag` |
| Avatar/profile images | `Avatar` |
| Dividers, spacers | `Divider`, `Spacer` |

**Reusability rules (STRICT):**
- ❌ NEVER duplicate UI blocks
- ✅ Extract ANY repeated pattern (even 2 occurrences)
- ✅ Props-based configuration for variants
- ✅ Composition over duplication

**Example transformation:**

Raw HTML:
```html
<button class="blue-btn large">Login</button>
<button class="blue-btn large">Sign Up</button>
<button class="gray-btn small">Cancel</button>
```

Generated (React):
```tsx
<Button variant="primary" size="lg">Login</Button>
<Button variant="primary" size="lg">Sign Up</Button>
<Button variant="secondary" size="sm">Cancel</Button>
```

Generated (SwiftUI):
```swift
Button("Login", variant: .primary, size: .large)
Button("Sign Up", variant: .primary, size: .large)
Button("Cancel", variant: .secondary, size: .small)
```

### Step 5: Layout Translation

Map CSS layouts to platform-native equivalents:

| CSS | React | SwiftUI | Compose | Flutter |
|-----|-------|---------|---------|---------|
| `display: flex; flex-direction: row` | `<div className="flex">` | `HStack` | `Row` | `Row()` |
| `display: flex; flex-direction: column` | `<div className="flex flex-col">` | `VStack` | `Column` | `Column()` |
| `display: grid` | CSS Grid / `grid` classes | `LazyVGrid` | `LazyVerticalGrid` | `GridView` |
| `position: absolute` | `absolute` positioning | `ZStack` + `.offset` | `Box` + `Modifier.offset` | `Stack` + `Positioned` |
| `position: fixed` | `fixed` class | `.overlay` | N/A (use Scaffold) | `Overlay` |
| `overflow: scroll` | `overflow-auto` | `ScrollView` | `LazyColumn` | `SingleChildScrollView` |

### Step 6: Generate Navigation

**Always generate navigation structure:**

1. **Detect navigation pattern from design:**
   - Tab bar at bottom → Tab-based navigation
   - Hamburger menu → Drawer navigation
   - Header with links → Stack navigation with header
   - Cards/buttons leading to detail → Push navigation

2. **Generate platform-appropriate navigation:**

   **React/Next.js:**
   ```tsx
   // app/layout.tsx with navigation
   // pages structure or app router
   ```

   **SwiftUI:**
   ```swift
   TabView or NavigationStack with NavigationLink
   ```

   **Compose:**
   ```kotlin
   NavHost with composable destinations
   Scaffold with BottomNavigation
   ```

   **Flutter:**
   ```dart
   MaterialApp with routes
   BottomNavigationBar or Drawer
   ```

### Step 7: Generate Code

**Output structure for each platform:**

```
<user_project>/
├── components/          # Extracted reusable components
│   ├── Button/
│   ├── Card/
│   ├── Input/
│   └── ...
├── screens/             # Full screens/pages
│   ├── HomeScreen
│   ├── DetailScreen
│   └── ...
├── theme/               # Design system
│   ├── colors
│   ├── typography
│   ├── spacing
│   └── ThemeProvider (optional)
├── navigation/          # Navigation setup
├── assets/              # Processed images, fonts
└── utils/               # Helpers
```

**Platform-specific naming:**

| Concept | React | SwiftUI | Compose | Flutter |
|---------|-------|---------|---------|---------|
| Components | `components/` | `Views/` | `ui/components/` | `lib/widgets/` |
| Screens | `pages/` or `app/` | `Screens/` | `ui/screens/` | `lib/screens/` |
| Theme | `theme/` | `Theme/` | `ui/theme/` | `lib/theme/` |
| ViewModels | N/A | `ViewModels/` | `viewmodels/` | `lib/providers/` |

### Step 8: Asset Management

1. **Deduplicate** identical assets
2. **Optimize** images (note any that seem oversized)
3. **Map to platform asset systems:**

| Platform | Asset Location | Config |
|----------|---------------|--------|
| Web | `/public/images/`, `/public/fonts/` | Import directly |
| iOS | `Assets.xcassets` | Asset catalog structure |
| Android | `res/drawable/`, `res/font/` | Resource XML |
| Flutter | `assets/images/`, `assets/fonts/` | `pubspec.yaml` |

### Step 9: Functional Layer

Convert static UI to functional:

- **State binding:** Connect inputs to state variables
- **Navigation:** Wire up buttons to navigate
- **Validation:** Add basic form validation
- **Interactions:** onClick, onPress, onChange handlers
- **Loading states:** Add loading indicators where appropriate

### Step 10: Output & Handoff

1. **Generate all files** into the user's specified project location
2. **Provide a component map** explaining:
   - What components were created
   - Where each is used
   - Props/configuration options
3. **List any manual steps needed:**
   - Font installation
   - Asset optimization recommendations
   - Missing pieces that need user input

---

## Code Quality Standards

**Always enforce:**
- ✅ No inline styles (use theme system)
- ✅ Consistent naming conventions
- ✅ Modular file structure
- ✅ Platform best practices
- ✅ Accessibility basics (labels, contrast)
- ✅ Responsive/adaptive layouts

**Never generate:**
- ❌ Raw HTML copy-paste
- ❌ Duplicated UI blocks
- ❌ Magic numbers (use spacing scale)
- ❌ Hardcoded strings (extract to constants)

---

## Platform-Specific References

For detailed platform implementation patterns, read these reference files:

- **Web (React/Next.js):** `references/web-react.md`
- **iOS (SwiftUI):** `references/ios-swiftui.md`
- **Android (Compose):** `references/android-compose.md`
- **Flutter:** `references/flutter.md`

---

## Error Handling

**When encountering issues, always ASK before assuming:**

| Issue | Question to Ask |
|-------|-----------------|
| Missing font files | "Font `X` is referenced but missing. Use system font or do you have the file?" |
| Ambiguous layout | "This could be tabs or horizontal scroll. Which did you intend?" |
| Unknown component pattern | "I see a custom element. Is this a `[guess]` or something else?" |
| Missing assets | "Image `X` not found in ZIP. Do you have it separately?" |
| Multiple HTML files | "I found multiple pages. Generate all, or start with one?" |

---

## Quick Reference

**Trigger phrases:**
- "Convert this design to..."
- "Turn this Stitch export into..."
- "Generate [React/SwiftUI/Compose/Flutter] from this ZIP"
- "Design to code"
- "Extract components from this design"

**Required inputs:**
1. ZIP file upload
2. Target platform
3. Project location

**Default behaviors:**
- Aggressive component extraction
- Auto-generate navigation
- Include design token files
- Theme provider is optional (ask user)
