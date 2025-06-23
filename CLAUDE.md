# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Flutter package called `infinite_scroll_tab_view` that provides a tab view component that can scroll infinitely. The package enables users to create tab interfaces where both tabs and page content can scroll infinitely in both directions using a cyclic/modulo approach.

## Development Commands

### Testing
```bash
flutter test
```

### Linting
```bash
flutter analyze
```

### Package Development
```bash
# Run the example app for testing
cd example
flutter run
```

### Publishing (when ready)
```bash
flutter pub publish --dry-run
flutter pub publish
```

## Architecture

### Core Components

1. **InfiniteScrollTabView** (`lib/src/infinite_scroll_tab_view.dart`): The main public API widget that users interact with. It's a StatelessWidget that wraps the internal implementation.

2. **InnerInfiniteScrollTabView** (`lib/src/inner_infinite_scroll_tab_view.dart`): The core implementation containing all the complex logic for infinite scrolling, tab synchronization, and animations. This is where the heavy lifting happens.

3. **CycledListView** (`lib/src/cycled_list_view.dart`): A custom ListView implementation that provides infinite scrolling capabilities by using infinite scroll bounds (double.negativeInfinity to double.infinity) and a custom scroll controller.

### Key Implementation Details

- **Infinite Scrolling**: Achieved through `CycledScrollController` and `_InfiniteScrollPosition` which set scroll bounds to infinite values
- **Content Indexing**: Uses modulo arithmetic to map infinite scroll positions to finite content arrays
- **Tab Synchronization**: Complex synchronization between tab scrolling and page scrolling using Tween animations and offset calculations
- **Dynamic Tab Sizing**: Calculates tab widths based on text content and supports both dynamic and fixed-width modes
- **Animation System**: Uses AnimationController for smooth tab transitions and indicator animations

### State Management

- Uses ValueNotifier for reactive state management
- Key state variables:
  - `_selectedIndex`: Currently selected tab index
  - `_isContentChangingByTab`: Prevents infinite loops during tab-initiated page changes
  - `_isTabPositionAligned`: Controls indicator visibility during scrolling
  - `_indicatorSize`: Manages indicator width animations

## Testing

Tests are located in `test/infinite_scroll_tab_view_test.dart` and cover:
- The `calculateMoveIndexDistance` function for cross-boundary navigation
- Tab size calculation logic
- Widget state initialization and text scale factor changes

## Key Files to Understand

- `lib/src/inner_infinite_scroll_tab_view.dart:127-174`: `calculateTabBehaviorElements()` - Core tab sizing and offset calculation
- `lib/src/inner_infinite_scroll_tab_view.dart:239-287`: `_onTapTab()` - Tab selection logic with smooth scrolling
- `lib/src/inner_infinite_scroll_tab_view.dart:397-405`: `calculateMoveIndexDistance()` - Algorithm for shortest path navigation across modulo boundaries
- `lib/src/cycled_list_view.dart:234-256`: `_InfiniteScrollPosition` - Implementation of infinite scroll bounds

## Development Notes

- The package uses Flutter 3.22.3 and Dart SDK >=3.3.2
- No external dependencies beyond Flutter SDK
- Uses `flutter_lints` for code quality
- Package follows Flutter package conventions with proper library exports