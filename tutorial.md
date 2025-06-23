# Installation guide for Aiken
## Below is the installation guide for Aiken to get you started

Installing Aiken
To start using Aiken, you’ll need:

Rust & Cargo (since Aiken compiler is built in Rust)

Aikup, the version manager/installer for Aiken

The Aiken CLI itself

Prerequisites
You must have Rust and Cargo installed. You can install them via rustup:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

if you have rust already installed in your system you can skip to this part
 Install Aikup (installer & version manager)
for npm (cross-platform)
```
npm install -g @aiken-lang/aikup
```
for Homebrew (macOS)
```
brew install aiken-lang/tap/aikup
```
Shell script (macOS & Linux)
```
curl --proto '=https' --tlsv1.2 -LsSf https://install.aiken-lang.org | sh
```
PowerShell (Windows)
powershell
```
irm https://windows.aiken-lang.org | iex
```
Once installed, simply run:
```
aikup
```
This installs the latest stable Aiken. To install a specific version:

aikup 0.0.11


Install & verify Aiken CLI
After installing via aikup, you’ll get the aiken CLI in your $PATH. Verify installation:

```
aiken --version
```
You should see output like:
```
aiken 0.x.y
```
