# Installation guide for Aiken
## Below is the installation guide for Aiken to get you started


Welcome to the Aiken installation guide! This tutorial will walk you through the steps to set up Aiken, a powerful tool for your development needs. Follow the instructions below to get started. By any chance, if you're confused about anything. Refer to the official installation guide at [Aiken-Lang](https://aiken-lang.org/installation-instructions)

## Prerequisites

Before installing Aiken, ensure you have the following dependencies:

- **Rust & Cargo**: Aiken's compiler is built in Rust, so Rust and Cargo are required.
- **Aikup**: The version manager and installer for Aiken.
 - **Aiken CLI**: The command-line interface for Aiken.

### Installing Rust and Cargo

To install Rust and Cargo, use `rustup`. Run the following command in your terminal:

```bash 
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

if you have rust already installed in your system you can skip to this part
Install Aikup (installer & version manager) for npm (cross-platform)
```
npm install -g @aiken-lang/aikup
```
#### for Homebrew (macOS)
```
brew install aiken-lang/tap/aikup
```
#### Shell script (macOS & Linux)
```
curl --proto '=https' --tlsv1.2 -LsSf https://install.aiken-lang.org | sh
```
#### PowerShell (Windows)

```powershell
irm https://windows.aiken-lang.org | iex
```
Once installed, simply run:
```
aikup
```
This installs the latest stable Aiken. To install a specific version:

```
aikup 0.0.11
```

Install & verify Aiken CLI
After installing via aikup, you’ll get the aiken CLI in your $PATH. Verify installation:

```
aiken --version
```
You should see output like:
```
aiken 0.x.y
```
### Integrations for IDEs & Shell

Aiken ships with:

-   **Language Server Protocol** (`aiken lsp`)
    
-   **Shell completion**:
    ```    
    aiken completion bash --install` 
    ```
-   **Editor support**:
    
    -   VS Code: `vscode-aiken`
        
    -   Vim/Neovim: `editor-integration-nvim`
        
    -   Zed, Emacs: official plugins [developers.cardano.org+12aiken-lang.org+12emurgo.io+12](https://aiken-lang.org/installation-instructions?utm_source=chatgpt.com)[aiken-lang.org+6github.com+6aiken-lang.org+6](https://github.com/aiken-lang?utm_source=chatgpt.com)[meshjs.dev](https://meshjs.dev/guides/aiken?utm_source=chatgpt.com)[committees.docs.intersectmbo.org](https://committees.docs.intersectmbo.org/intersect-open-source-committee/guides-and-educational-resources/onboarding-guide-for-aiken-learners?utm_source=chatgpt.com)

[previous](index.md) [Next](tutorial.md)
