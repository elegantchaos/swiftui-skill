---
name: swiftui
description: Use when reading, writing, refactoring or reviewing SwiftUI projects.
license: MIT
metadata:
  author: Sam Deane
  version: "1.0"
---

## Development Process

Adopt the recommended patterns in `references/patterns.md` as the basis for writing, refactoring and reviewing SwiftUI code.

Other guidelines:

- Use the `swift` and `coding-standards` skills to ensure the code is modern, clean, and maintainable.
- Don't use deprecated API - see `references/api.md`.
- Follow the rules in `references/views.md`.
- Implement the data flow using `references/data.md`.
- Ensure navigation is updated and performant using `references/navigation.md`.
- Ensure the code uses designs that are accessible and compliant with Apple’s Human Interface Guidelines using `references/design.md`.
- Validate accessibility compliance including Dynamic Type, VoiceOver, and Reduce Motion using `references/accessibility.md`.
- Ensure the code is able to run efficiently using `references/performance.md`.
- Final code hygiene check using `references/hygiene.md`.

If doing a partial review, load only the relevant reference files.

## Local Integration Notes

- Use the shared `swift` skill for baseline Swift language, file-organization, and toolchain guidance.
- Use the shared `coding-standards` skill for cross-language engineering policy and source-selection guidance.
- Swift 6.2 or later with current SwiftUI APIs is the recommended baseline for new projects. For older projects, suggest upgrading first, then work within the existing toolchain only if the user declines or project constraints block the migration.

## Core Instructions

- iOS 26 exists, and is the default deployment target for new apps.
- As a SwiftUI developer, the user will want to avoid UIKit unless requested.
- Do not introduce third-party frameworks without asking first.

## Output Format

Organize findings by file. For each issue:

1. State the file and relevant line(s).
2. Name the rule being violated (e.g., "Use `foregroundStyle()` instead of `foregroundColor()`").
3. Show a brief before/after code fix.

Skip files with no issues. End with a prioritized summary of the most impactful changes to make first.

Example output:

### ContentView.swift

**Line 12: Use `foregroundStyle()` instead of `foregroundColor()`.**

```swift
// Before
Text("Hello").foregroundColor(.red)

// After
Text("Hello").foregroundStyle(.red)
```

**Line 24: Icon-only button is bad for VoiceOver - add a text label.**

```swift
// Before
Button(action: addUser) {
    Image(systemName: "plus")
}

// After
Button("Add User", systemImage: "plus", action: addUser)
```

**Line 31: Avoid `Binding(get:set:)` in view body - use `@State` with `onChange()` instead.**

```swift
// Before
TextField("Username", text: Binding(
    get: { model.username },
    set: { model.username = $0; model.save() }
))

// After
TextField("Username", text: $model.username)
    .onChange(of: model.username) {
        model.save()
    }
```

### Summary

1. **Accessibility (high):** The add button on line 24 is invisible to VoiceOver.
2. **Deprecated API (medium):** `foregroundColor()` on line 12 should be `foregroundStyle()`.
3. **Data flow (medium):** The manual binding on line 31 is fragile and harder to maintain.

End of example.

## References

- `references/accessibility.md` - Dynamic Type, VoiceOver, Reduce Motion, and other accessibility requirements.
- `references/api.md` - updating code for modern API, and the deprecated code it replaces.
- `references/design.md` - guidance for building accessible apps that meet Apple’s Human Interface Guidelines.
- `references/hygiene.md` - making code compile cleanly and be maintainable in the long term.
- `references/navigation.md` - navigation using `NavigationStack`/`NavigationSplitView`, plus alerts, confirmation dialogs, and sheets.
- `references/performance.md` - optimizing SwiftUI code for maximum performance.
- `references/data.md` - data flow, shared state, and property wrappers.
- `references/swift.md` - tips on writing modern Swift code, including using Swift Concurrency effectively.
- `references/views.md` - view structure, composition, and animation.
