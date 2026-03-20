# iOS (SwiftUI) Reference

## Project Structure

```
ProjectName/
├── App/
│   ├── ProjectNameApp.swift      # @main entry point
│   └── ContentView.swift         # Root view
├── Views/
│   ├── Components/               # Reusable UI components
│   │   ├── ButtonStyles.swift
│   │   ├── CardView.swift
│   │   ├── InputField.swift
│   │   └── ...
│   ├── Screens/                  # Full screens
│   │   ├── HomeScreen.swift
│   │   ├── DetailScreen.swift
│   │   └── ...
│   └── Navigation/
│       └── MainTabView.swift
├── ViewModels/
│   ├── HomeViewModel.swift
│   └── ...
├── Models/
│   └── ...
├── Theme/
│   ├── Colors.swift
│   ├── Typography.swift
│   ├── Spacing.swift
│   └── Theme.swift
├── Extensions/
│   └── View+Extensions.swift
├── Utilities/
│   └── ...
└── Assets.xcassets/
    ├── Colors/
    ├── Images/
    └── ...
```

## Component Templates

### Button Component
```swift
// Views/Components/AppButton.swift
import SwiftUI

struct AppButton: View {
    enum Variant {
        case primary, secondary, ghost
    }
    
    enum Size {
        case sm, md, lg
        
        var height: CGFloat {
            switch self {
            case .sm: return 36
            case .md: return 44
            case .lg: return 52
            }
        }
        
        var fontSize: CGFloat {
            switch self {
            case .sm: return 14
            case .md: return 16
            case .lg: return 18
            }
        }
    }
    
    let title: String
    var variant: Variant = .primary
    var size: Size = .md
    var action: () -> Void
    
    var body: some View {
        Button(action: action) {
            Text(title)
                .font(.system(size: size.fontSize, weight: .semibold))
                .frame(maxWidth: .infinity)
                .frame(height: size.height)
        }
        .buttonStyle(AppButtonStyle(variant: variant))
    }
}

struct AppButtonStyle: ButtonStyle {
    let variant: AppButton.Variant
    
    func makeBody(configuration: Configuration) -> some View {
        configuration.label
            .background(backgroundColor)
            .foregroundColor(foregroundColor)
            .cornerRadius(AppSpacing.radiusMd)
            .opacity(configuration.isPressed ? 0.8 : 1.0)
    }
    
    private var backgroundColor: Color {
        switch variant {
        case .primary: return AppColors.primary
        case .secondary: return AppColors.secondary
        case .ghost: return .clear
        }
    }
    
    private var foregroundColor: Color {
        switch variant {
        case .primary: return .white
        case .secondary: return AppColors.textPrimary
        case .ghost: return AppColors.primary
        }
    }
}
```

### Card Component
```swift
// Views/Components/CardView.swift
import SwiftUI

struct CardView<Content: View>: View {
    let content: Content
    var padding: CGFloat = AppSpacing.md
    
    init(padding: CGFloat = AppSpacing.md, @ViewBuilder content: () -> Content) {
        self.padding = padding
        self.content = content()
    }
    
    var body: some View {
        content
            .padding(padding)
            .background(AppColors.surface)
            .cornerRadius(AppSpacing.radiusLg)
            .shadow(color: .black.opacity(0.05), radius: 4, y: 2)
    }
}
```

## Theme Setup

```swift
// Theme/Colors.swift
import SwiftUI

struct AppColors {
    static let primary = Color("Primary")           // From Assets
    static let secondary = Color("Secondary")
    static let accent = Color("Accent")
    static let background = Color("Background")
    static let surface = Color("Surface")
    static let textPrimary = Color("TextPrimary")
    static let textSecondary = Color("TextSecondary")
    static let textMuted = Color("TextMuted")
}

// Theme/Typography.swift
import SwiftUI

struct AppTypography {
    static let largeTitle = Font.system(size: 34, weight: .bold)
    static let title = Font.system(size: 28, weight: .bold)
    static let title2 = Font.system(size: 22, weight: .bold)
    static let title3 = Font.system(size: 20, weight: .semibold)
    static let headline = Font.system(size: 17, weight: .semibold)
    static let body = Font.system(size: 17, weight: .regular)
    static let callout = Font.system(size: 16, weight: .regular)
    static let subheadline = Font.system(size: 15, weight: .regular)
    static let footnote = Font.system(size: 13, weight: .regular)
    static let caption = Font.system(size: 12, weight: .regular)
}

// Theme/Spacing.swift
import SwiftUI

struct AppSpacing {
    static let xs: CGFloat = 4
    static let sm: CGFloat = 8
    static let md: CGFloat = 16
    static let lg: CGFloat = 24
    static let xl: CGFloat = 32
    static let xxl: CGFloat = 48
    
    static let radiusSm: CGFloat = 4
    static let radiusMd: CGFloat = 8
    static let radiusLg: CGFloat = 16
    static let radiusXl: CGFloat = 24
}
```

## Navigation Patterns

### Tab Navigation
```swift
// Views/Navigation/MainTabView.swift
import SwiftUI

struct MainTabView: View {
    @State private var selectedTab = 0
    
    var body: some View {
        TabView(selection: $selectedTab) {
            HomeScreen()
                .tabItem {
                    Image(systemName: "house.fill")
                    Text("Home")
                }
                .tag(0)
            
            SearchScreen()
                .tabItem {
                    Image(systemName: "magnifyingglass")
                    Text("Search")
                }
                .tag(1)
            
            ProfileScreen()
                .tabItem {
                    Image(systemName: "person.fill")
                    Text("Profile")
                }
                .tag(2)
        }
        .tint(AppColors.primary)
    }
}
```

### Stack Navigation
```swift
// Views/Screens/HomeScreen.swift
import SwiftUI

struct HomeScreen: View {
    var body: some View {
        NavigationStack {
            List {
                ForEach(items) { item in
                    NavigationLink(value: item) {
                        ItemRow(item: item)
                    }
                }
            }
            .navigationTitle("Home")
            .navigationDestination(for: Item.self) { item in
                DetailScreen(item: item)
            }
        }
    }
}
```

## MVVM Pattern

```swift
// ViewModels/HomeViewModel.swift
import Foundation
import Combine

@MainActor
class HomeViewModel: ObservableObject {
    @Published var items: [Item] = []
    @Published var isLoading = false
    @Published var error: Error?
    
    func loadItems() async {
        isLoading = true
        defer { isLoading = false }
        
        do {
            items = try await fetchItems()
        } catch {
            self.error = error
        }
    }
}

// In View:
struct HomeScreen: View {
    @StateObject private var viewModel = HomeViewModel()
    
    var body: some View {
        // ...
        .task {
            await viewModel.loadItems()
        }
    }
}
```

## Responsive Layouts

```swift
// Adaptive layout based on size class
struct AdaptiveLayout: View {
    @Environment(\.horizontalSizeClass) var sizeClass
    
    var body: some View {
        if sizeClass == .compact {
            // iPhone layout
            VStack { content }
        } else {
            // iPad layout
            HStack { sidebar; content }
        }
    }
}
```

## Asset Catalog Structure

```
Assets.xcassets/
├── AccentColor.colorset/
├── AppIcon.appiconset/
├── Colors/
│   ├── Primary.colorset/
│   ├── Secondary.colorset/
│   └── ...
└── Images/
    ├── logo.imageset/
    └── ...
```

Each `.colorset` supports light/dark mode variants automatically.
