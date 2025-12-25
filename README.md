# Flutter WhisperKit

[![Fork](https://img.shields.io/badge/fork-moinsen--dev-blue)](https://github.com/moinsen-dev/flutter_whisper_kit)

> **Note**: This is a fork of [r0227n/flutter_whisper_kit](https://github.com/r0227n/flutter_whisper_kit) with fixes for iOS/macOS platform compatibility.

A monorepo for Flutter WhisperKit packages that enable on-device speech recognition in Flutter applications using WhisperKit.

## Fork Changes

This fork includes the following improvements over the original:

### Podspec Platform Fix (Issue #148)
The original podspec had a bug where setting `s.platform` twice would cause the second platform declaration to override the first, breaking multi-platform support. This fork fixes the issue by using platform-specific deployment targets and dependencies:

```ruby
# Fixed: Support both iOS and macOS platforms
s.ios.deployment_target = '16.0'
s.osx.deployment_target = '13.0'

# Platform-specific dependencies
s.ios.dependency 'Flutter'
s.osx.dependency 'FlutterMacOS'
```

### Progress Stream Variant Identifier (Issue #108)
Fixed progress stream conflicts during simultaneous downloads. The `Progress` model now includes an optional `variant` field that identifies which model the progress event belongs to, allowing clients to filter progress by variant when downloading multiple models concurrently:

```dart
// Progress events now include the variant
modelProgressStream.listen((progress) {
  if (progress.variant == 'openai_whisper-large-v3') {
    // Handle progress for this specific model
  }
});
```

### Translation Control (Issue #152)
The `task` parameter in `DecodingOptions` is now properly respected. You can control whether WhisperKit transcribes audio in the original language or translates it to English:

```dart
// Transcribe in original language (default)
await transcribeFromFile(path, options: DecodingOptions(
  task: DecodingTask.transcribe,
  language: 'de',  // German
));

// Translate to English
await transcribeFromFile(path, options: DecodingOptions(
  task: DecodingTask.translate,
  language: 'de',  // German audio, English output
));
```

## Overview

Flutter WhisperKit is a wrapper package that enables the use of WhisperKit within Flutter applications. [WhisperKit] is an on-device speech recognition framework developed by Argmax that provides high-quality transcription capabilities.

## Repository Structure

This repository is organized as a monorepo containing the following packages:

- **flutter_whisper_kit**: The main package that provides a platform-agnostic API for using WhisperKit in Flutter applications.
- **flutter_whisper_kit_apple**: The platform implementation for Apple devices (iOS and macOS).

## Features

- On-device speech recognition with no data sent to external servers
- Support for multiple model sizes (tiny to large)
- File-based audio transcription
- Real-time microphone transcription
- Configurable model storage options
- Progress tracking for model downloads

## Demo

https://github.com/user-attachments/assets/4a405460-2eb9-4485-a467-24b8ebf6bee7

## Getting Started

To use Flutter WhisperKit in your Flutter application, add the following dependency to your `pubspec.yaml`:

```yaml
dependencies:
  flutter_whisper_kit: latest
```

For detailed usage instructions, refer to the documentation in the individual package directories:

- [flutter_whisper_kit]
- [flutter_whisper_kit_apple]

## Acknowledgments

- [Original flutter_whisper_kit](https://github.com/r0227n/flutter_whisper_kit) - The original Flutter WhisperKit implementation by r0227n
- [WhisperKit] - The underlying speech recognition framework by Argmax
- [Flutter] - The UI toolkit for building natively compiled applications

## License

This project is licensed under the MIT License - see the LICENSE file for details.

<!-- URLs -->

[WhisperKit]: https://github.com/argmaxinc/WhisperKit
[flutter_whisper_kit]: packages/flutter_whisper_kit/README.md
[flutter_whisper_kit_apple]: packages/flutter_whisper_kit_apple/README.md
[Flutter]: https://flutter.dev/
