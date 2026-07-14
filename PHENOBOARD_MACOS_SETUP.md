# Phenoboard macOS Setup Guide

This guide explains how to install or build GA4GH Phenoboard on macOS.

Phenoboard is a Tauri desktop application with an Angular frontend and a Rust
backend. It is used to curate individuals and cohorts with Human Phenotype
Ontology terms and the GA4GH Phenopacket Schema.

Project links:

- Documentation: <https://p2gx.github.io/phenoboard/>
- Installation docs: <https://p2gx.github.io/phenoboard/help/installation.html>
- GitHub repository: <https://github.com/P2GX/phenoboard>
- Releases: <https://github.com/P2GX/phenoboard/releases>

There are two setup paths:

1. Install the ready-made macOS app. This is best for most users.
2. Build and run Phenoboard from source. This is best for developers.

## Option 1: Install the Ready-Made App

This is the recommended path if you only want to use Phenoboard.

### 1. Check Your Mac Type

The current Phenoboard macOS installer is built for Apple Silicon Macs.

1. Click the Apple menu in the top-left corner of your screen.
2. Click **About This Mac**.
3. Look for **Chip** or **Processor**.

If your Mac says Apple M1, M2, M3, or M4, download the file named:

```bash
phenoboard_<version>_aarch64.dmg
```

### 2. Download Phenoboard

1. Open <https://github.com/P2GX/phenoboard/releases>.
2. Find the newest release at the top of the page.
3. Open the **Assets** section.
4. Download the macOS file ending in `_aarch64.dmg`.

As of July 14, 2026, the latest published release is `v0.5.152`, and the macOS
Apple Silicon installer is:

```bash
phenoboard_0.5.152_aarch64.dmg
```

You can download it from Terminal with macOS's built-in `curl`:

```bash
mkdir -p ~/Downloads/phenoboard
cd ~/Downloads/phenoboard
curl -L -O https://github.com/P2GX/phenoboard/releases/download/v0.5.152/phenoboard_0.5.152_aarch64.dmg
```

For a newer release, replace every `0.5.152` with the version shown on the
current GitHub release.

### 3. Verify the Download

GitHub release assets include a SHA-256 digest. For `v0.5.152`, the digest for
`phenoboard_0.5.152_aarch64.dmg` is:

```text
53ee18296ee494bea1e7b59d32c7a7517ee4c3fc049602a4e7c4bc14020204ab
```

Calculate the checksum on your Mac:

```bash
shasum -a 256 phenoboard_0.5.152_aarch64.dmg
```

The first value printed by `shasum` must match the release digest exactly. If
it does not match, delete the `.dmg` and download it again.

For future releases, use the SHA-256 digest shown for the matching asset on the
GitHub release page or through the GitHub releases API. Do not compare against
a checksum you generated yourself as the expected value.

### 4. Install Phenoboard

1. Open the downloaded `.dmg` file.
2. Drag the Phenoboard app into the **Applications** folder.
3. Open **Applications**.
4. Double-click Phenoboard.

### 5. Approve the App if macOS Warns You

The current Phenoboard documentation notes that the app is open source,
distributed for free, and not signed or notarized by Apple. macOS may warn you
the first time you open it.

If macOS asks whether you are sure you want to open Phenoboard, confirm that
you want to open it.

If macOS blocks the app:

1. Open **System Settings**.
2. Go to **Privacy & Security**.
3. Find the message saying Phenoboard was blocked.
4. Click **Open Anyway**.
5. Confirm that you want to open it.

After the first approval, Phenoboard should open like any other application,
including from Spotlight.

## Option 2: Build and Run Phenoboard From Source

Use this path if you want to inspect, modify, or develop Phenoboard.

## Prerequisites

The current Phenoboard docs list these source-build prerequisites:

- Node.js at least version 18.
- npm at least version 9.
- Rust and Cargo.
- Git, unless you download the source archive manually.

On macOS, the easiest beginner setup is to use Apple's Command Line Tools,
Homebrew, Node.js, Git, and rustup.

### 1. Install Apple's Command Line Tools

Open Terminal and run:

```bash
xcode-select --install
```

If macOS says the tools are already installed, continue.

### 2. Install Homebrew

Check whether Homebrew is already installed:

```bash
brew --version
```

If that prints a version number, continue. If it says `brew: command not
found`, install Homebrew from:

<https://brew.sh/>

After installation, close Terminal, open it again, and confirm:

```bash
brew --version
```

### 3. Install Git

Check whether Git is installed:

```bash
git --version
```

If Git is missing, install it:

```bash
brew install git
```

### 4. Install Node.js and npm

Install Node.js through Homebrew:

```bash
brew install node
```

Check the versions:

```bash
node -v
npm -v
```

Phenoboard currently uses Angular 21 and Nx 22 in the upstream source. If a
build command reports that Node.js is too old, update Node.js:

```bash
brew upgrade node
```

### 5. Install Rust and Cargo

Install Rust with rustup:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Choose the default installation when prompted. Then close Terminal, open it
again, and check:

```bash
rustc --version
cargo --version
rustup --version
```

Update Rust later with:

```bash
rustup update
```

### 6. Download the Phenoboard Source Code

Choose a folder for code projects. This example uses `~/Developer`:

```bash
mkdir -p ~/Developer
cd ~/Developer
git clone https://github.com/P2GX/phenoboard.git
cd phenoboard
```

### 7. Install JavaScript Dependencies

From inside the `phenoboard` directory, run:

```bash
npm install
```

This installs Angular, Nx, the Tauri CLI, and the rest of the JavaScript
dependencies listed in `package.json`.

### 8. Run Phenoboard in Development Mode

Run:

```bash
npm run tauri dev
```

The upstream `package.json` also provides this equivalent shortcut:

```bash
npm run tauri:dev
```

This starts the Angular development server on port `1420` and opens Phenoboard
as a desktop application. Leave Terminal open while using the development app.

To stop the development app:

1. Close the Phenoboard window.
2. Return to Terminal.
3. Press `Control-C`.

### 9. Build a Production App

To build an installable app for your current operating system, run:

```bash
npm run tauri build
```

The generated app and installer files are placed under:

```bash
src-tauri/target/release/bundle/
```

On Apple Silicon macOS, the DMG will be under:

```bash
src-tauri/target/release/bundle/dmg/
```

## Useful Developer Commands

Run these commands from inside the Phenoboard source directory.

Start only the Angular frontend development server:

```bash
npm run start
```

The frontend is served at:

```bash
http://localhost:1420
```

Build only the Angular frontend:

```bash
npm run build
```

Run the Tauri desktop app in development mode:

```bash
npm run tauri dev
```

or:

```bash
npm run tauri:dev
```

Remove generated build files and caches:

```bash
npm run clean
```

Clean generated files and then start a fresh Tauri development run:

```bash
npm run tauri:dev:fresh
```

## Troubleshooting

### `command not found: brew`

Install Homebrew from:

<https://brew.sh/>

Then close Terminal, reopen it, and run:

```bash
brew --version
```

### `command not found: git`

Install Apple's Command Line Tools:

```bash
xcode-select --install
```

If Git is still missing, install it with Homebrew:

```bash
brew install git
```

### `command not found: node` or `command not found: npm`

Install Node.js:

```bash
brew install node
```

Then check:

```bash
node -v
npm -v
```

### Angular or Nx Says Node.js Is Unsupported

Check your Node.js version:

```bash
node -v
```

If it is too old, update it:

```bash
brew upgrade node
```

Then rerun:

```bash
npm install
```

### `command not found: rustc` or `command not found: cargo`

Rust is missing, or Terminal has not loaded the Rust environment.

Install Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

After installation, close Terminal completely and open it again. Then check:

```bash
rustc --version
cargo --version
```

If those commands still do not work, load the Rust environment manually:

```bash
source "$HOME/.cargo/env"
```

### Rust Build Fails During Development

The first Tauri build downloads and compiles Rust dependencies. This can take a
while. If the build fails, first update Rust:

```bash
rustup update
```

Then run the development command again:

```bash
npm run tauri dev
```

If it still fails after a partial build, try a fresh run:

```bash
npm run tauri:dev:fresh
```

### `npm install` Fails

First check that Node.js and npm are available:

```bash
node -v
npm -v
```

Then try a clean install:

```bash
rm -rf node_modules
npm install
```

### Port 1420 Is Already in Use

Phenoboard's development server uses port `1420`.

Find the process using the port:

```bash
lsof -i :1420
```

If you recognize the process and know it is safe to stop, end it:

```bash
kill -9 <PID>
```

Then start Phenoboard again:

```bash
npm run tauri dev
```

### Spotlight Opens an Old Phenoboard Build

The current developer docs mention that macOS Spotlight can sometimes find an
old local Phenoboard app while you are testing a newer release.

To find the app Spotlight is opening:

1. Open **Activity Monitor**.
2. Select the running Phenoboard process.
3. Click the info button.
4. Open **Open Files and Ports**.
5. Look for the executable path.

You can reveal that app in Finder with:

```bash
open -R /path/to/Phenoboard.app
```

Then remove the old app if you no longer need it.

## Important Files in the Phenoboard Project

- `package.json`: JavaScript dependencies and npm commands.
- `src/`: Angular frontend source code.
- `src-tauri/`: Rust and Tauri desktop app source code.
- `src-tauri/tauri.conf.json`: Tauri app configuration.
- `src-tauri/Cargo.toml`: Rust package metadata and dependencies.
- `book/`: Phenoboard documentation source.
- `dist/`: generated Angular build output.
- `src-tauri/target/`: generated Rust and Tauri build output.

## Quick Checklist

For installing the released app:

- Download `phenoboard_<version>_aarch64.dmg` from the GitHub releases page.
- Verify the `.dmg` with the published SHA-256 digest.
- Drag Phenoboard into **Applications**.
- Approve the app in **Privacy & Security** if macOS blocks it.

For building from source:

- Install Apple's Command Line Tools.
- Install Homebrew.
- Install Git.
- Install Node.js and npm.
- Install Rust and Cargo.
- Clone <https://github.com/P2GX/phenoboard>.
- Run `npm install`.
- Run `npm run tauri dev`.