# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

HXPHPicker is an iOS photo/video picker library (Swift) supporting iOS 12.0+. Key features include:
- Photo/video selection with multi-select, sliding selection, LivePhoto, GIF
- Photo/video editing (graffiti, stickers, text, crop, mosaic, filter)
- In-app camera capture
- iCloud resource download
- Light/dark/custom UI themes
- Internationalization (Chinese, English, Japanese, Korean, Thai, Indonesian, Vietnamese, Russian, German, French, Arabic)

## Build Commands

```bash
# Install dependencies via CocoaPods (primary method)
pod install

# Build via Xcode command line
xcodebuild -workspace HXPHPicker.xcworkspace -scheme HXPHPicker_Example build

# Run SwiftLint
swiftlint
```

## Architecture

```
Sources/HXPHPicker/
├── Core/           # Shared base: configs, base controllers, extensions, models, utilities
├── Picker/         # Photo/video selection UI and logic
├── Editor/         # Photo/video editing (graffiti, stickers, text, crop, mosaic, filter)
├── Editor+View/    # Editor UI components (Lite/KF variants with Kingfisher)
├── Camera/         # In-app camera capture
├── Camera+Location/# Camera with location support
└── Resources/      # Asset bundles (HXPHPicker.bundle)
```

### Key Patterns

- **Main API entry point**: `Photo` enum with static methods for picker/editor/camera
- **File naming**: `<Module>+<Feature>.swift` for extensions (e.g., `PhotoPickerController+Transitioning.swift`)
- **Conditional compilation**: Feature flags (`HXPICKER_ENABLE_PICKER`, `HXPICKER_ENABLE_EDITOR`, `HXPICKER_ENABLE_CAMERA`, etc.)
- **Delegate pattern**: `PhotoPickerControllerDelegate`, `EditorViewControllerDelegate`
- **Compatibility pattern**: `HXPickerCompatible` protocol with `hx` property extension

### Feature Modules

| Module | Purpose | Feature Flag |
|--------|---------|--------------|
| Picker | Photo/video selection | `HXPICKER_ENABLE_PICKER` |
| Editor | Photo/video editing | `HXPICKER_ENABLE_EDITOR` |
| Camera | Camera capture | `HXPICKER_ENABLE_CAMERA` |
| Editor+View | Editor UI with Kingfisher | `HXPICKER_ENABLE_EDITOR_VIEW` |
| Camera+Location | Camera with location | `HXPICKER_ENABLE_CAMERA_LOCATION` |

### Dependency Configuration

- **Kingfisher**: ~> 7.0 (optional, for image loading/caching)
- **Lite variants**: Exclude Kingfisher dependency
- **KF variants**: Include Kingfisher for enhanced image handling

## Code Style

SwiftLint is configured with custom rules (see `.swiftlint.yml`):
- Disabled: `trailing_whitespace`, `identifier_name`, `force_cast`, etc.
- Function body length: 120 (warning), 150 (error)
- File length: 1000 (warning), 1500 (error)
- Cyclomatic complexity: 40 max

## Important Notes

- No test infrastructure exists; this is a pure library project
- The example app is in `HXPHPicker_Example/` workspace
- Resources are bundled in `Sources/HXPHPicker/Resources/`
