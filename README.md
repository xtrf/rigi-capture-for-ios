# Rigi iOS Capture

[![Platform](https://img.shields.io/badge/platform-macOS-orange.svg)]()
[![Languages](https://img.shields.io/badge/language-Swift-orange.svg)]()
[![License](https://img.shields.io/badge/license-Commercial-green.svg)]()

**Rigi iOS Capture** is a powerful macOS application that streamlines the localization preview workflow for iOS developers. It enables direct capture from iOS Simulators with seamless integration to the Rigi localization platform.

## Table of Contents

1. [Introduction](#introduction)
2. [Key Features](#key-features)
3. [System Requirements](#system-requirements)
4. [Installation](#installation)
5. [Workflow Overview](#workflow-overview)
6. [Step 1: Xcode Project Setup](#step-1-xcode-project-setup)
7. [Step 2: Rigi Server Setup](#step-2-rigi-server-setup)
8. [Step 3: Rigi iOS Capture Setup](#step-3-rigi-ios-capture-setup)
9. [Step 4: Localization Sync](#step-4-localization-sync)
10. [Step 5: Capture and Upload](#step-5-capture-and-upload)
11. [Step 6: Translate with Previews](#step-6-translate-with-previews)
12. [Tips and Best Practices](#tips-and-best-practices)
13. [Troubleshooting](#troubleshooting)
14. [Support](#support)

<br/>

## Introduction

**Rigi iOS Capture** provides a complete solution for capturing localization previews directly from iOS Simulators. The app uses intelligent text scanning technology to detect translatable text and generates visual previews that help translators understand the context of each string.

Find out more about the Rigi Software Localization Tool at [https://xtm.cloud/rigi/](https://xtm.cloud/rigi/)

<br/>

## Key Features

- **Native macOS Application** - Simple drag-and-drop installation
- **Direct Simulator Capture** - Capture screenshots from running iOS Simulators
- **Intelligent Text Scanning** - Automatic text detection and recognition
- **Project Management** - Organize multiple localization projects
- **Seamless Rigi Integration** - Direct upload to Rigi platform
- **Visual Preview Generation** - Automatic preview creation with text highlighting
- **Xcode Localization Catalog Support** - Import/export XLIFF files directly

<br/>

## System Requirements

- macOS 11.0 (Big Sur) or later
- Xcode 13.0 or later with Localization Catalogs
- iOS Simulator
- Rigi server account with API access

**Important**: This tool requires your Xcode project to use Localization Catalogs (Xcode 15+). If your project uses legacy .strings or .stringsdict files, you'll need to migrate first. See [Apple's Localization documentation](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog) for migration instructions.

<br/>

## Installation

Download and install Rigi iOS Capture:

![Installation](docs/assets/20.1-capture-tool-installer.png)

1. Download the installer from [https://github.com/xtrf/rigi-ios-capture](https://github.com/xtrf/rigi-ios-capture)
2. Open the downloaded DMG file
3. Drag **Rigi iOS Capture.app** to your Applications folder
4. The installer includes an uninstaller for easy removal

<br/>

## Workflow Overview

The complete workflow consists of three main setup phases followed by the capture process:

1. **Xcode Setup** - Prepare your iOS project for localization
2. **Rigi Server Setup** - Configure your project on the Rigi platform
3. **Capture Tool Setup** - Connect the macOS app to your project
4. **Sync & Capture** - Export, sync, capture, and translate

<br/>

## Step 1: Xcode Project Setup

### Prerequisites - Localization Catalogs

Rigi iOS Capture requires your project to use Xcode's Localization Catalogs (.xcstrings files). If you're using legacy .strings files, you must migrate first:

1. Follow [Apple's migration guide](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog)
2. In Xcode: Select your .strings files → Editor → Migrate to String Catalog
3. Verify all strings are properly migrated

### Add Pseudo Language

First, add a pseudo language to your Xcode project. This special language will contain marked-up text for the capture process.

![Add Pseudo Language](docs/assets/01-add-speudo.png)

1. Open your project in Xcode
2. Navigate to Project Settings → Info → Localizations
3. Click the + button to add a new language
4. Select **Zulu (zu)** as your pseudo language
5. This language will be used exclusively for capturing previews

### Create a Dedicated Target (Optional)

For better organization, create a separate target for captures:

![Create Target](docs/assets/04-create-target.png)

1. In Xcode, duplicate your existing target
2. Name it appropriately (e.g., "Fruta iOS Capture")
3. This keeps your production builds separate from capture builds

### Configure Target Language

Set the pseudo language for your capture target:

![Set Target Language](docs/assets/05-set-target-language.png)

1. Select your capture scheme
2. Edit Scheme → Run → Options
3. Set App Language to **Zulu**
4. This ensures the app runs with pseudo-localized strings

<br/>

## Step 2: Rigi Server Setup

### Create a Project on Rigi

Set up your project on the Rigi platform:

![Create Rigi Project](docs/assets/11-create-rigi-project.png)

1. Log into your Rigi server
2. Click "Create project"
3. Enter project details:
   - Project name
   - Description
   - Source language
   - **Important!** The language **must exactly** match the language added in Xcode, e.g. English (and not English-US)

### Configure Pseudo Language

Configure the translation manager in your Rigi project:

![Set Rigi Pseudo Language](docs/assets/12-set-rigi-pseudo.png)

1. In the Translation manager step
2. You **do not** have to select a Pseudo language
3. **Important!** Make sure to **unselect** 'Manager files in workspace'

### Create API Access Token

Generate an authentication token for the capture tool:

![Create API Token](docs/assets/14-create-rigi-access-token.png)

1. Navigate to Settings → Access tokens
2. Click "Create access token"
3. Name it appropriately (e.g., "Capture Tool Access")
4. Select required permissions:
   - Update project settings
   - Upload to workspace
   - Download from workspace
   - Tokenize strings
   - Upload HTML Previews
5. Copy and save the generated token securely

<br/>

## Step 3: Rigi iOS Capture Setup

### Create a Capture Project

Launch Rigi iOS Capture and create a new project:

![Create Capture Project](docs/assets/20.2-create-capture-project.png)

1. Open Rigi iOS Capture from Applications
2. Click "Create Project"
3. Enter a project name (e.g., "Fruta Demo")
4. Click "Create Project" to continue

### Configure Project Settings

Complete the three configuration sections:

![Project Settings](docs/assets/21-capture-project-settings.png)

#### Project Folder Configuration
1. Click "Select Project Folder"
2. Navigate to your Xcode project root
3. The tool will automatically detect:
   - Xcode project name
   - Localization status
   - Available languages

#### Rigi Configuration
1. Enter your **Project Server URL**
   - Format: `https://test.rigi.io/projects/16`
2. Select **Source Language** (must match Xcode)
3. Select **Pseudo Language** (must match Xcode)
4. Click **Save Configuration**

#### Server Authentication
1. Paste the API token created earlier
2. Click **Test Connection** to verify
3. Upon successful connection, click **Save Token**

### Settings Storage and Team Sharing

The Rigi iOS Capture app stores configuration in two locations:

**Project Settings** are saved in your project directory:

- Location: `<PROJECT-FOLDER>/RigiSDK/`
- Contains: `rigi.ini` (project settings) and `rigi.meta` (translatable texts)

**Team Sharing**: Add these files to your Git repository to share configuration with your team

**Security Note**: The API access token is stored securely in your macOS Keychain, never in project files. Each team member must configure their own token.

<br/>

## Step 4: Localization Sync

### Export from Xcode

Export your project's localizations:

<img src="docs/assets/22-xcode-export-localization.png" width="300">

1. In Xcode, select Product → Export Localizations...
2. Choose a destination folder
3. Xcode will create a folder containing all localization files

### Upload to Rigi

Import the localizations into Rigi iOS Capture:

![Upload Localizations](docs/assets/23-upload-localizations.png)

1. Switch to the "Sync Localization" tab
2. Drag and drop the exported folder onto the upload area
   - Or click "Browse Files" to select
3. The tool will validate and count all strings
4. Review the file and string counts
5. Click "Upload to Rigi"

### Upload Success

After successful upload:

![Upload Success](docs/assets/24-upload-success.png)

1. The tool confirms successful upload
2. Shows total strings processed
3. Displays the pseudo language code
4. Click **Open Import Folder** to access the processed files
5. The folder contains the pseudo-localized XLIFF file

### Import Back to Xcode

Import the pseudo-localized file:

<img src="docs/assets/26-xcode-import-localization.png" width="300">

1. In Xcode, select Product → Import Localizations...
2. Navigate to the import folder
3. Select the `zu.xliff` file
4. Import it into your project
5. Your strings now have `[# #]` markers for capture

<br/>

## Step 5: Capture and Upload

### Run with Pseudo Language

Launch your app with the pseudo locale:

<img src="docs/assets/30-xcode-run-pseudo-code.png" width="500">

1. Select your capture scheme in Xcode
2. Verify it's set to use Zulu language
3. Build and run in iOS Simulator
4. Your app now displays marked-up text

### Capture Previews

Capture screens from the running simulator:

![Capture Preview](docs/assets/31-capture-preview.png)

1. In Rigi iOS Capture, go to "Capture Previews" tab
2. Select your running simulator from the dropdown
3. Navigate to screens in your app
4. Click **Capture** to take a screenshot
5. The tool will:
   - Scan for text using intelligent text scanning
   - Match text with localization strings
   - Highlight recognized strings in green
   - Highlight unmactched strings in orange
   - Show statistics (scanned vs. translatable)
6. Review the preview
7. Click **Upload to Server** to send to Rigi

<br/>

## Step 6: Translate with Previews

Translators can now work with visual context:

![Translate with Preview](docs/assets/32-translate-with-preview.png)

1. On the Rigi server, translators see:
   - The source text
   - Visual preview showing where text appears
   - Live preview updates as they translate
2. Visual context helps ensure accurate translations
3. Completed translations can be exported back to Xcode

<br/>

## Troubleshooting

### Simulator Issues
- **Not detected**: Ensure iOS Simulator is running
- **Can't capture**: Try restarting the Simulator
- **Poor recognition**: Check text clarity and zoom level

### Connection Problems
- **Test failed**: Verify server URL format
- **Auth error**: Check API token permissions
- **Network issues**: Verify internet connectivity

### Import/Export
- **Missing strings**: Re-export from Xcode
- **Import fails**: Verify file format and language code
- **Markers not showing**: Check pseudo language import

### Localization Catalog Issues
- **No XLIFF export option**: Ensure you're using Localization Catalogs (.xcstrings)
- **Legacy files**: Migrate .strings files to String Catalogs first
- **Missing languages**: Add languages in project settings before export

<br/>

## Support

For additional help:

- **Documentation**: [https://xtm.cloud/rigi/](https://xtm.cloud/rigi/)
- **GitHub**: [https://github.com/xtrf/rigi-ios-capture](https://github.com/xtrf/rigi-ios-capture)
- **Support**: Contact your Rigi administrator

---

Copyright © 2025 Rigi.io powered by XTM. All rights reserved.
