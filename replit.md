# Fighter Doctors PDF Pro - Professional Study PDF Editor

## Overview

This is a Flutter-based Android application that provides **professional PDF editing for students** with secure encryption and sharing capabilities. The app combines advanced study tools (smart highlighting, flashcards, notes export) with military-grade security using Android Keystore for RSA/AES encryption.

**Important:** This project is designed for Android mobile devices and cannot run as a traditional web application in Replit due to its dependency on native Android features (Android Keystore, platform channels, etc.).

## Current Replit Setup

Since this is an Android-specific application, Replit is configured to serve a **documentation website** that explains the project, its features, and provides build instructions.

### What's Running

- **Documentation Server**: A Python-based web server (port 5000) serving an HTML documentation page
- **Purpose**: Provides project information, features, and Android build instructions
- **Access**: The documentation website is available when you run this Repl

### Documentation

- **Interactive Docs**: View the web documentation at the Replit webview (click Run)
- **Project README**: See [README.md](README.md) for complete project documentation
- **Arabic README**: See [readme_arabic.md](readme_arabic.md) for Arabic documentation

## Project Architecture

### Technology Stack

- **Frontend Framework**: Flutter 3.4.3+
- **Native Code**: Kotlin (Android)
- **Security**: Android Keystore, RSA 2048-bit, AES 256-bit
- **PDF Library**: Syncfusion Flutter PDF Viewer
- **Annotations**: flutter_pdf_annotations
- **State Management**: Provider pattern

### Key Features

**Study Features (Pro):**
1. **Smart Highlighting**: 5-color system (Critical, Important, Review, Reference, Note)
2. **Study Notes**: Organized annotations with page references
3. **Flashcards**: Auto-generate flashcards from highlights
4. **Progress Tracking**: Monitor study progress with statistics
5. **Advanced Search**: Search across all content and annotations
6. **Export Notes**: Export all notes as Markdown for review
7. **Bookmarks**: Quick access to important pages
8. **Backup/Restore**: Save and restore all annotations

**Security Features:**
9. **Encryption**: Military-grade RSA 2048-bit + AES 256-bit
10. **Secure Storage**: Uses Android Keystore for key management
11. **Encrypted Sharing**: Share encrypted PDFs with PEM key files
12. **Screen Protection**: Prevents screenshots and screen recording
13. **One-Time Use**: Optional single-use file encryption

### Native Integrations

The app relies on Android-specific features via platform channels:

- `ensureDeviceKey`: Initialize RSA key pair in Android Keystore
- `encryptPdfForSharing`: Encrypt PDF using AES + RSA key wrapping
- `decryptReceivedPdf`: Decrypt received encrypted PDF files

## Building for Android

### Prerequisites

- Flutter SDK 3.4.3 or higher
- Android Studio with Android SDK (API 26+)
- Java JDK 11 or higher
- Android device or emulator

### Build Commands

```bash
# Install dependencies
flutter pub get

# Run on connected device
flutter run

# Build release APK
flutter build apk --release

# Build split APKs by architecture
flutter build apk --release --split-per-abi

# Run tests
flutter test
```

### File Structure

```
fighter_doctors_pdf/
├── lib/
│   ├── main.dart              # Main application entry
│   └── utils/
│       ├── encryption_utils.dart
│       └── pem_handler.dart
├── android/
│   └── app/src/main/kotlin/
│       └── MainActivity.kt    # Native Android encryption implementation
├── assets/
│   └── images/                # App logos and assets
├── docs/
│   └── index.html             # Documentation website (Replit)
├── pubspec.yaml               # Flutter dependencies
└── server.py                  # Documentation web server (Replit)
```

## Security Architecture

### Encryption Flow

1. Generate random AES-256 key for each PDF
2. Encrypt PDF data with AES-CBC
3. Wrap AES key with RSA-2048 public key (from Android Keystore)
4. Save encrypted PDF with embedded wrapped key
5. Export public key as PEM file for sharing

### Decryption Flow

1. Read encrypted file header and extract wrapped key
2. Unwrap AES key using RSA private key (from Android Keystore)
3. Decrypt PDF data with unwrapped AES key
4. Optionally mark file as consumed (one-time use)

### File Format

```
[HEADER: "ENCPDF01" (8 bytes)]
[VERSION (4 bytes)]
[WRAPPED_KEY_LENGTH (4 bytes)]
[WRAPPED_KEY (variable)]
[CONSUMED_FLAG (1 byte)]
[IV (16 bytes)]
[ENCRYPTED_DATA (variable)]
```

## Recent Changes

- **2025-11-05**: Initial Replit setup with documentation server
  - Created HTML documentation website
  - Configured Python server for port 5000
  - Added project overview and build instructions

## User Preferences

- None configured yet

## Limitations in Replit

- **No Android Runtime**: Cannot run the actual Flutter app due to lack of Android emulator/device
- **No Keystore Access**: Android Keystore APIs are not available in web environment
- **Platform Channels**: Native Kotlin code cannot be executed in web context
- **File System**: Web apps have limited file system access compared to Android

## Next Steps for Development

To develop this application locally:

1. Clone the repository to your local machine
2. Install Flutter SDK and Android Studio
3. Set up Android emulator or connect physical device
4. Run `flutter pub get` to install dependencies
5. Run `flutter run` to launch the app on Android
6. Test encryption/decryption features with real PDF files

## Dependencies

Key Flutter packages (from pubspec.yaml):

- `syncfusion_flutter_pdfviewer`: PDF viewing
- `flutter_pdf_annotations`: PDF annotation capabilities
- `file_picker`: File selection
- `share_plus`: File sharing
- `flutter_windowmanager`: Screen security
- `pointycastle`: Cryptography
- `crypto`: Hash functions
- `asn1lib`: ASN.1 encoding for keys

## Support

For questions or issues:
- Check the README.md for detailed documentation
- Review Android build logs for compilation issues
- Ensure Android SDK and Flutter SDK versions are compatible
