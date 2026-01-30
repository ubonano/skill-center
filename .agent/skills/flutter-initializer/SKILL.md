---
name: flutter-initializer
description: Initialize a new clean Flutter project with specific architecture and configuration.
---

# Flutter Project Initializer

This skill automates the creation of a new Flutter project conformant to specific standards: clean slate (only main.dart), Web support, GetIt dependency, and a specific scalable folder structure.

## When to use this skill

- When the user asks to create a new Flutter project.
- When the user asks to initialize a project with "clean architecture" or standard setup.
- When the user references the "flutter-initializer" skill.

## How to use it

Follow these steps exactly to initialize the project. If any step fails, stop and ask the user.
**IMPORTANT**: The user prefers to be spoken to in **Spanish**. All your output messages and questions to the user should be in Spanish.

### 1. Request Project Name
If the user hasn't provided a project name in their request, ask for it (e.g., "¿Cómo te gustaría llamar al proyecto?").

### 2. Initialize Project
Run the following command to create the project with Web support enabled:
`flutter create --platforms web,android,ios <project_name>`

### 3. Clean and Structure Directory
1.  **Remove Default Files**: Delete the default `lib/main.dart` and `test/widget_test.dart` (and any other files in `lib/`).
2.  **Create Folder Structure**: Ensure the following directory structure exists inside `lib/`:
    ```text
    lib/
    ├── core/
    │   ├── config/
    │   │   ├── secret/
    │   │   └── locator.dart
    │   ├── models/
    │   ├── services/
    │   └── shared/
    ├── features/
    ├── app.dart
    └── main.dart
    ```

### 4. Setup Dependencies
1.  Add `get_it` to `pubspec.yaml`. You can do this by running `flutter pub add get_it` inside the project directory.
2.  Run `flutter pub get`.

### 5. Create Configuration Files

**`lib/core/config/locator.dart`**:
```dart
import 'package:get_it/get_it.dart';

final GetIt locator = GetIt.instance;

void setupLocator() {
  // TODO: Register services and models here
}
```

### 6. Create Entry Points

**`lib/app.dart`**:
```dart
import 'package:flutter/material.dart';

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Material App',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Material App Bar'),
        ),
        body: const Center(
          child: Text('Hola Mundo'),
        ),
      ),
    );
  }
}
```

**`lib/main.dart`**:
```dart
import 'package:flutter/material.dart';
import 'app.dart';
import 'core/config/locator.dart';

void main() {
  setupLocator();
  runApp(const MyApp());
}
```

### 7. Configure Git
1.  Initialize git: `git init` (if not already initialized).
2.  Update `.gitignore` to exclude the secret folder. Append this line to `.gitignore`:
    ```text
    lib/core/config/secret/
    ```

### 8. Add Project Rules
1.  Create the directory `.agent/rules/`: `mkdir -p .agent/rules`
2.  Create the file `.agent/rules/project_rules.md` with the following content:
    ```markdown
    ---
    description: "General project rules, architecture, and code conventions."
    globs: "**/*.dart"
    ---
    # Rules: Flutter Project Standards

    ## 1. Project Context
    - **Framework**: Flutter (Web, Android, iOS)
    - **Language**: Dart
    - **Architecture**: Feature-based Clean Architecture.
        - `lib/core/`: Shared resources, services, and config.
        - `lib/features/`: Isolated features with their own logic/UI.
    - **Dependency Injection**: `get_it` (via `locator.dart`).

    ## 2. Tech Stack & Libraries
    - **GetIt**: For service locator/dependency injection.
    - (Add others as the project evolves)

    ## 3. Code Conventions (CRITICAL)
    - **Clean Code**:
        - Variables and functions must have descriptive, English names.
        - Functions should be small and do one thing well.
    - **DRY (Don't Repeat Yourself)**:
        - Never duplicate logic. Extract to `core/shared` or feature-specific utilities.
    - **Widgets**:
        - Split large build methods into smaller private widgets or separate files.
    - **formatting**:
        - Always use trailing commas for better formatting.

    ## 4. Agent Behavior
    - **Refactoring**: If you see messy code, propose a refactor immediately.
    - **Language**: logic/comments in English. User communication in **Spanish** (as per global user preference).
    ```

### 9. Final Verification
- Notify the user (in Spanish) that the project has been successfully initialized (e.g., "El proyecto '<nombre>' se ha creado correctamente con la estructura solicitada.").
