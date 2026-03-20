# 🎨 Design ZIP to App Generator

> Convert Stitch design exports into production-ready ui components for Web, iOS, Android, and Flutter — with aggressive component extraction, design system generation, and auto-navigation.

[![Claude Skill](https://img.shields.io/badge/Claude-Skill-blueviolet?style=for-the-badge&logo=anthropic)](https://claude.ai)
[![Platforms](https://img.shields.io/badge/Platforms-React%20%7C%20SwiftUI%20%7C%20Compose%20%7C%20Flutter-blue?style=for-the-badge)]()
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)]()

---

## 🚀 What It Does

This Claude skill transforms your **Stitch design exports** (HTML/CSS/assets ZIP files) into fully functional, scalable applications:

| Input | Output |
|-------|--------|
| Stitch ZIP (HTML + CSS + assets) | Production-ready codebase |
| Raw design export | Extracted design system |
| Static mockups | Functional UI with navigation |

### Supported Platforms

| Platform | Framework | Architecture | Styling |
|----------|-----------|--------------|---------|
| 🌐 Web | React / Next.js | Component-based | Tailwind CSS |
| 🍎 iOS | SwiftUI | MVVM | Native SwiftUI |
| 🤖 Android | Jetpack Compose | MVVM + ViewModel | Material3 |
| 🐦 Flutter | Flutter/Dart | Clean Architecture | ThemeData |

---

## ✨ Key Features

### 🧱 Aggressive Component Extraction
Every reusable pattern gets extracted — buttons, cards, inputs, lists, headers, modals, and more. No duplicated UI blocks.

```
Raw HTML:
<button class="blue-btn">Login</button>
<button class="blue-btn">Signup</button>

Generated SwiftUI:
Button("Login", variant: .primary)
Button("Signup", variant: .primary)
```

### 🎨 Design System Generation
Automatically extracts design tokens from your CSS:
- **Colors** — Primary, secondary, accent, backgrounds, text hierarchy
- **Typography** — Font families, sizes, weights, line heights
- **Spacing** — Padding/margin scale, border radius, shadows

### 🧭 Auto-Navigation
Detects and generates appropriate navigation patterns:
- Tab bars → `TabView` / `BottomNavigationBar`
- Stack navigation → `NavigationStack` / `NavHost`
- Modals → Sheet presentations

### 🔍 Smart Clarifications
When input is ambiguous, the skill asks before assuming:
- Missing fonts? → "Should I substitute with a system font?"
- Unclear layout? → "Is this tabs or horizontal scroll?"
- Missing assets? → "Do you have this image separately?"

---

## 📦 Installation

### Option 1: Download & Double-Click
1. Download `design-zip-to-app.skill` from [Releases](../../releases)
2. Double-click to install in Claude Code

### Option 2: Manual Installation
```bash
# Clone this repo
git clone https://github.com/YOUR_USERNAME/design-zip-to-app.git

# Copy to Claude skills directory
cp -r design-zip-to-app ~/.claude/skills/

# Or for project-specific installation
cp -r design-zip-to-app your-project/.claude/skills/
```

### Option 3: Direct Download
```bash
# Download and extract
curl -L https://github.com/YOUR_USERNAME/design-zip-to-app/releases/latest/download/design-zip-to-app.skill -o design-zip-to-app.skill
```

---

## 🛠️ Usage

### Basic Usage
1. Upload your Stitch export ZIP to Claude
2. Tell Claude which platform you want:
   ```
   Convert this design to SwiftUI
   ```
3. Specify where to place the output:
   ```
   Put the generated code in ~/Projects/MyApp/
   ```

### Example Prompts

```
"Convert this Stitch ZIP to a React/Next.js app"

"Generate SwiftUI code from my design export, use MVVM architecture"

"Turn this into a Flutter app with Riverpod state management"

"Create an Android Compose app from this design, include dark mode"
```

### Advanced Options

| Option | Description | Example |
|--------|-------------|---------|
| Architecture | Override default architecture | "Use Clean Architecture" |
| State Management | Specify state solution | "Use Zustand for React" |
| Dark Mode | Enable dark mode support | "Include dark mode" |
| Animations | Add animation system | "Add Framer Motion animations" |

---

## 📁 Output Structure

The skill generates a complete, organized codebase:

```
YourProject/
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
│   └── ThemeProvider
├── navigation/          # Navigation setup
├── assets/              # Processed images, fonts
└── utils/               # Helpers
```

---

## 🎯 Design Token Example

The skill extracts tokens like this from your CSS:

```json
{
  "colors": {
    "primary": "#4A90E2",
    "secondary": "#7B61FF",
    "accent": "#FF6B6B",
    "background": "#FFFFFF",
    "surface": "#F5F5F5",
    "textPrimary": "#111111",
    "textSecondary": "#666666"
  },
  "typography": {
    "fontFamily": {
      "primary": "Inter",
      "secondary": "Roboto Mono"
    },
    "fontSize": {
      "xs": 12, "sm": 14, "base": 16, "lg": 18, "xl": 24
    }
  },
  "spacing": [4, 8, 12, 16, 24, 32, 48],
  "radius": {
    "sm": 4, "md": 8, "lg": 16, "full": 9999
  }
}
```

---

## 🧩 Component Detection

The skill detects and extracts these component patterns:

| Pattern | Extracted Component |
|---------|---------------------|
| `<button>`, `.btn`, clickable elements | `Button` |
| Card containers with image + text | `Card` |
| `<input>`, `<textarea>`, form fields | `Input`, `TextArea` |
| Repeated list items | `ListItem`, `List` |
| Top navigation bars | `Header`, `NavBar` |
| Bottom navigation | `TabBar`, `BottomNav` |
| Overlay containers | `Modal`, `Sheet` |
| Icon + text combinations | `IconLabel` |
| Badge/tag elements | `Badge`, `Tag` |
| Avatar/profile images | `Avatar` |

---

## ⚠️ Constraints

The skill enforces these quality standards:

| ✅ Do | ❌ Don't |
|-------|---------|
| Extract all reusable patterns | Duplicate UI blocks |
| Use theme system | Inline styles |
| Follow platform conventions | Raw HTML copy-paste |
| Generate navigation | Leave static mockups |
| Modular file structure | Magic numbers |

---

## 📚 Included References

The skill includes platform-specific best practices:

| File | Contents |
|------|----------|
| `references/web-react.md` | React patterns, Tailwind, Zustand, Framer Motion |
| `references/ios-swiftui.md` | SwiftUI patterns, MVVM, Combine, Navigation |
| `references/android-compose.md` | Compose patterns, Material3, ViewModel, StateFlow |
| `references/flutter.md` | Widget patterns, Riverpod, go_router, ThemeData |

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes to `SKILL.md` or reference files
4. Test with various Stitch exports
5. Submit a pull request

### Ideas for Contribution
- Add support for more design tools (Figma HTML export, Webflow, Framer)
- Improve component detection patterns
- Add more platform references (Kotlin Multiplatform, React Native)
- Enhance dark mode generation

---

## 📄 License

MIT License — feel free to use, modify, and distribute.

---

## 🙏 Credits

Built for use with [Claude](https://claude.ai) by Anthropic.

Designed for [Stitch](https://stitch.ai) design exports.

---

## 💬 Support

- **Issues**: [GitHub Issues](../../issues)
- **Discussions**: [GitHub Discussions](../../discussions)

---

<p align="center">
  <b>Made with 💜 for designers who code and developers who design</b>
</p>
