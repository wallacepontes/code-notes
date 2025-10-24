# Chocolatey

## Table of Contents
- [Chocolatey](#chocolatey)
  - [Table of Contents](#table-of-contents)
  - [Basic](#basic)
    - [`choco install`](#choco-install)
    - [Syntax:](#syntax)
    - [Example:](#example)
    - [Why it's useful:](#why-its-useful)
    - [Other notable commands:](#other-notable-commands)
  - [How update chocolatey version?](#how-update-chocolatey-version)
    - [🚀 How to Update Chocolatey](#-how-to-update-chocolatey)
      - [Breakdown of the Command](#breakdown-of-the-command)
      - [Recommended Usage (with confirmation)](#recommended-usage-with-confirmation)
    - [📦 Related Update Commands](#-related-update-commands)
  - [Re-install using Chocolatey](#re-install-using-chocolatey)
    - [How to Get Chocolatey to Manage Node.js and npm](#how-to-get-chocolatey-to-manage-nodejs-and-npm)
      - [Option 1: Re-install using Chocolatey (Recommended)](#option-1-re-install-using-chocolatey-recommended)
      - [Option 2: Use the `choco upgrade` command to take over](#option-2-use-the-choco-upgrade-command-to-take-over)
  - [Breakdown some packages](#breakdown-some-packages)
    - [1. KB Packages (Windows Updates)](#1-kb-packages-windows-updates)
    - [2. VCRedist Packages (Visual C++ Redistributables)](#2-vcredist-packages-visual-c-redistributables)
  - [Install plsql](#install-plsql)
    - [1. Install the Full PostgreSQL Server](#1-install-the-full-postgresql-server)
      - [Installation Notes:](#installation-notes)
    - [2. Install Only the Client Tools](#2-install-only-the-client-tools)
  - [Chocolatey to install everything](#chocolatey-to-install-everything)
    - [👍 Benefits of Using Chocolatey for New PC Setup](#-benefits-of-using-chocolatey-for-new-pc-setup)
    - [⚠️ Important Considerations](#️-important-considerations)
      - [1. Not Everything is on Chocolatey](#1-not-everything-is-on-chocolatey)
      - [2. Manual Installation Takeover](#2-manual-installation-takeover)
      - [3. Package Management Philosophy](#3-package-management-philosophy)
  - [New empty computer using Chocolatey first](#new-empty-computer-using-chocolatey-first)
    - [Full-Stack Development Installation Command 🚀](#full-stack-development-installation-command-)
    - [Breakdown of Installed Packages](#breakdown-of-installed-packages)
    - [Optional Additions (If Applicable)](#optional-additions-if-applicable)
  - [The best way to install Chocolatey](#the-best-way-to-install-chocolatey)
    - [⚙️ How to Install Chocolatey](#️-how-to-install-chocolatey)
      - [Step 1: Open PowerShell as Administrator](#step-1-open-powershell-as-administrator)
      - [Step 2: Set Execution Policy and Install](#step-2-set-execution-policy-and-install)
        - [A. Set Execution Policy](#a-set-execution-policy)
        - [B. Run the Installation Script](#b-run-the-installation-script)
      - [Step 3: Verify Installation](#step-3-verify-installation)
  - [When You typically see packages like 7zip and 7zip.install](#when-you-typically-see-packages-like-7zip-and-7zipinstall)
    - [📦 7zip (Metapackage)](#-7zip-metapackage)
    - [📦 7zip.install (Installer Package)](#-7zipinstall-installer-package)
  - [choco install javaruntime or jre8 ?](#choco-install-javaruntime-or-jre8-)
    - [⚙️ Recommended Command](#️-recommended-command)
    - [🆚 Package Differences](#-package-differences)
    - [🔑 Key Takeaway](#-key-takeaway)
    - [1. Install the Modern JDK (JRE Included)](#1-install-the-modern-jdk-jre-included)
    - [2. Install Legacy JRE (If Absolutely Required)](#2-install-legacy-jre-if-absolutely-required)
      - [OpenJDK 8 Availability on Chocolatey](#openjdk-8-availability-on-chocolatey)
      - [Important Context on OpenJDK 8](#important-context-on-openjdk-8)
  - [choco list](#choco-list)
    - [1. Check if Chocolatey Manages the Package](#1-check-if-chocolatey-manages-the-package)
      - [Check by Package Name](#check-by-package-name)
    - [2. Check if the Program Executable is on PATH](#2-check-if-the-program-executable-is-on-path)
    - [3. Check for GUI Applications](#3-check-for-gui-applications)
  - [Copy \& Past](#copy--past)
  - [References](#references)

## Basic
**Chocolatey** is a Windows package manager designed to simplify the process of installing, updating, and managing software on a Windows machine, much like package managers in Linux (e.g., apt or yum). It automates the process of downloading and installing software from trusted sources, ensuring that users always get the latest versions with minimal manual effort.

The most useful command in Chocolatey is often considered to be:

### `choco install`
This command is the backbone of Chocolatey and allows users to install any software package available in the Chocolatey repository with a simple command. Here's how it works:

### Syntax:
```bash
choco install <package_name> -y
```
- `<package_name>` is the name of the software or package you want to install.
- The `-y` flag automatically agrees to any prompts, making the installation fully automated.

### Example:
```bash
choco install googlechrome -y
```
This would install Google Chrome without any manual intervention.

### Why it's useful:
- **Efficiency:** Instead of manually searching, downloading, and running installers, you can install software in seconds.
- **Automation:** With scripts, you can automate the installation of multiple programs (e.g., setting up a new machine).
- **Package updates:** It keeps installed software up to date with the `upgrade` command.

### Other notable commands:
- `choco upgrade <package_name>`: Updates a specific package.
- `choco list --local-only`: Lists installed packages managed by Chocolatey.
- `choco uninstall <package_name>`: Uninstalls a package.

## How update chocolatey version?

To update the **Chocolatey CLI** (the package manager itself), you use the standard `choco upgrade` command, treating Chocolatey as one of its own packages.

You must run this command from an elevated (Administrator) Command Prompt or PowerShell session.

-----

### 🚀 How to Update Chocolatey

The command to update Chocolatey is:

```bash
choco upgrade chocolatey
```

#### Breakdown of the Command

  * **`choco upgrade`**: This is the Chocolatey command for upgrading installed packages.
  * **`chocolatey`**: This is the package ID for the Chocolatey command-line program itself.

#### Recommended Usage (with confirmation)

For automated or more efficient use, it is common to add the `-y` or `--yes` switch to automatically confirm any prompts:

```bash
choco upgrade chocolatey -y
```

This tells Chocolatey to proceed with the upgrade without asking for confirmation.

-----

### 📦 Related Update Commands

Once you've updated the Chocolatey CLI, here are other related commands you might find useful:

| Action | Command | Description |
| :--- | :--- | :--- |
| **Upgrade All Packages** | `choco upgrade all -y` | Upgrades all installed packages to their latest available versions. |
| **Check for Outdated** | `choco outdated` | Lists all packages that have a newer version available, but **does not** perform the upgrade. |
| **Upgrade Specific Package** | `choco upgrade <package-name>` | Upgrades only the specified package (e.g., `choco upgrade git`). |

-----

If you are looking for a video guide on how to install and update software on Windows using Chocolatey, you can watch [Como instalar e atualizar vários softwares no Windows COM O CHOCOLATEY\!](https://www.youtube.com/watch?v=MrxMBQHJyeI).
http://googleusercontent.com/youtube_content/1

## Re-install using Chocolatey

That's common\! When you see that **Node.js** and **npm** are not listed as installed by Chocolatey, but **Python** is, it means that:

1.  **Python** was likely installed using the command `choco install python`.
2.  **Node.js** and **npm** were installed directly from the official website, via a manual installer, or using a different package manager (like Winget or NVM).

Chocolatey **only manages software that it installs itself**. If a package isn't in its internal list, it cannot track, upgrade, or manage it, even if the software is on your computer.

-----

### How to Get Chocolatey to Manage Node.js and npm

You have two main options to bring Node.js and npm under Chocolatey's management:

#### Option 1: Re-install using Chocolatey (Recommended)

The cleanest way is to **uninstall** your current Node.js installation (via Windows' "Add or remove programs") and then reinstall it using Chocolatey. This ensures Chocolatey sets up the correct package manifest and update scripts.

1.  **Uninstall Manually:** Go to Windows **Settings** or **Control Panel** and uninstall **Node.js**.

2.  **Install via Choco:** Run the Chocolatey install command for Node.js (which includes npm):

    ```bash
    choco install nodejs -y
    ```

    (Or `choco install nodejs-lts -y` for the Long-Term Support version.)

#### Option 2: Use the `choco upgrade` command to take over

The `choco upgrade` command can sometimes detect and take over management of software that was installed manually, as long as the installed version is recognized by the Chocolatey package.

Run the upgrade command for the Node.js package:

```bash
choco upgrade nodejs -y
```

If the package exists on your system, Chocolatey will attempt to install its package metadata and start managing the application. If it succeeds, you'll see Node.js and npm appear in the `choco list --local-only` output and be manageable via `choco upgrade` moving forward.

## Breakdown some packages

Those packages are not applications you typically use directly. They are **fundamental dependencies** (under-the-hood components) required by various other software on your system.

Here is a breakdown of what each group of packages represents:

***

### 1. KB Packages (Windows Updates)

The packages prefixed with **KB** (which stands for Knowledge Base) are packages that install critical **Windows Updates** and components often required by development tools, security features, or newer runtimes.

| Package ID | Purpose |
| :--- | :--- |
| **KB2919355** | A large cumulative update for Windows 8.1 and Windows Server 2012 R2. This is a foundational update required before installing many other Windows components or service packs. |
| **KB2919442** | A prerequisite/servicing stack update that must be installed *before* $\text{KB2919355}$. |
| **KB2999226** | **Update for Universal C Runtime (CRT) in Windows.** This update is crucial for many applications developed with newer versions of Visual Studio (like Visual Studio 2015 and later). It ensures those programs can run on older Windows versions. |
| **KB3033929** | An update that adds **SHA-2 code signing support** to Windows 7 and Windows Server 2008 R2. This is a security-related update required for installing software with modern digital signatures. |
| **KB3035131** | A **security update** for the Windows kernel. |

**Why Chocolatey installed them:** Chocolatey often installs these $\text{KB}$ packages as **dependencies** when you install development-related software (like Python, Git, or a compiler). If the required Windows component is missing, Chocolatey uses the $\text{KB}$ package to install it and satisfy the dependency.

***

### 2. VCRedist Packages (Visual C++ Redistributables)

These packages install the necessary runtime components for programs written in C/C++ using Microsoft's Visual Studio compiler.

| Package ID | Purpose |
| :--- | :--- |
| **vcredist140** | Installs the **Microsoft Visual C++ Redistributable for Visual Studio 2015-2022**. This is a current, single runtime package that covers multiple Visual Studio versions and is required by almost all modern Windows software (like Steam games, many open-source tools, etc.). The version **14.38** indicates a very recent update. |
| **vcredist2015** | Installs the specific **Microsoft Visual C++ Redistributable for Visual Studio 2015**. While $\text{vcredist140}$ supersedes it, $\text{vcredist2015}$ might be installed because a specific older package you installed still explicitly lists $\text{vcredist2015}$ as a dependency. |

**In short:** These packages are crucial libraries. Without them, a large portion of the software on your Windows machine (installed via Chocolatey or otherwise) would not be able to run.

**Conclusion:** You should generally **leave these packages alone**. They are essential runtime components and dependencies that Chocolatey is managing to ensure your other software can function correctly and securely.

## Install plsql

If you don't have $\text{psql}$ (the interactive terminal for **PostgreSQL**) installed, you can easily install it using Chocolatey.

You have a few choices depending on whether you want the full database server or just the command-line client tools.

-----

### 1\. Install the Full PostgreSQL Server

This option installs the complete PostgreSQL database server, the command-line utilities (including $\text{psql}$), and usually a graphical management tool like pgAdmin.

Run the following command in an **elevated (Administrator) terminal**:

```bash
choco install postgresql -y
```

#### Installation Notes:

  * This will download and run the official PostgreSQL installer, which typically installs the server, the command-line tools, and creates a Windows service.
  * **You may be prompted** by the installer to set the password for the default superuser ($\text{postgres}$).
  * $\text{psql}$ will be added to your system's $\text{PATH}$ and accessible from any terminal window after the installation is complete (you may need to close and reopen your terminal).

-----

### 2\. Install Only the Client Tools

If you already have a PostgreSQL server running elsewhere (e.g., on a cloud server or a Docker container) and just need the $\text{psql}$ command-line client and other client utilities, you can install the **PostgreSQL Client** package:

Run the following command in an **elevated (Administrator) terminal**:

```bash
choco install postgresql-client -y
```

This package installs the necessary tools like $\text{psql}$, $\text{pg\_dump}$, and $\text{pg\_restore}$ without the heavy database server component.

## Chocolatey to install everything

That's an excellent idea and a common practice! Using **Chocolatey** to install everything you need on a new Windows computer is highly recommended because it transforms a time-consuming, manual process into a fast, repeatable, automated script.

Here is a breakdown of the benefits and some considerations:

***

### 👍 Benefits of Using Chocolatey for New PC Setup

1.  **Automation and Speed:** Instead of manually downloading and clicking through dozens of installers, you can install all your essential applications (browsers, editors, development tools, utilities) with a single command or script.
    * **Example:** `choco install googlechrome vscode spotify vlc -y`
2.  **Consistency:** Chocolatey ensures you get the official, stable packages every time, reducing the risk of installing bundled crapware or outdated versions.
3.  **Dependency Management:** As you noticed with the $\text{KB}$ and $\text{vcredist}$ packages, Chocolatey handles necessary prerequisites automatically. When you install Python, for example, it ensures all required Microsoft runtimes are installed first.
4.  **Easy Updates:** Once everything is installed, keeping it updated is effortless. A single command (`choco upgrade all -y`) keeps all managed software current, which is crucial for security.
5.  **Simplified Reinstallation:** If you ever need to wipe your machine or set up another one, you can simply save your list of packages (a `packages.config` file) and run one command to recreate your entire software environment.

***

### ⚠️ Important Considerations

#### 1. Not Everything is on Chocolatey

While the repository is huge, some highly niche or brand-new applications may not have a package. For a full new setup, you will likely still need to install one or two items manually.

#### 2. Manual Installation Takeover

As discussed with Node.js, if you manually install an application **after** setting up Chocolatey, the package manager won't know it exists and won't update it. Be disciplined about using Chocolatey for all future installations.

#### 3. Package Management Philosophy

Chocolatey manages software at the **package level**. This means if you install the `visualstudio` package, for instance, you'll still interact with the Visual Studio Installer GUI for heavy component customization. Chocolatey streamlines the initial launch of the official installer.

**Overall:** For a new PC, using Chocolatey is the best way to move to a **DevOps-style workflow** for your own machine, saving you hours of setup time and making maintenance much simpler.

## New empty computer using Chocolatey first

That's the perfect scenario for Chocolatey\! Since the new computer is empty, you can use a single, powerful command to install all the most popular, essential tools for full-stack development.

Here is a complete command tailored for a typical modern full-stack environment, combining common tools for web, backend, and general utilities:

### Full-Stack Development Installation Command 🚀

Run this command in an **elevated (Administrator) PowerShell or Command Prompt** window:

```bash
choco install git vscode 7zip googlechrome nodejs-lts python3 -y
```

-----

### Breakdown of Installed Packages

This single command installs the core components needed for a vast range of full-stack projects:

| Package ID | Tool Name | Why It's Needed (Full-Stack Role) |
| :--- | :--- | :--- |
| **`git`** | Git | **Version Control.** Essential for all collaborative and professional software development. |
| **`vscode`** | Visual Studio Code | **Code Editor.** The most popular, lightweight, and versatile IDE for web and scripting. |
| **`nodejs-lts`** | Node.js (LTS) | **Backend Runtime.** Enables JavaScript backend development (with frameworks like Express) and provides **npm** (Node Package Manager), which is crucial for frontend (React, Vue, Angular) build tools and dependencies. |
| **`python3`** | Python | **Backend/Scripting.** Used for data processing, machine learning, and many backend services (like Django or Flask). Installs the latest version of Python 3. |
| **`googlechrome`** | Google Chrome | **Primary Browser.** Standard browser for development, testing, and debugging web applications. |
| **`7zip`** | 7-Zip | **Utility.** A standard tool for file compression and decompression, often needed for package handling. |

-----

### Optional Additions (If Applicable)

Depending on your specific needs, you might want to extend the command with these highly popular tools:

| Additional Tool | Package ID | Purpose |
| :--- | :--- | :--- |
| **Database Client** | `dbeaver` | A universal database tool for working with PostgreSQL, MySQL, SQLite, etc. |
| **PowerShell Terminal** | `microsoft-windows-terminal` | A modern, multi-tab terminal application from Microsoft. |
| **Docker** | `docker-desktop` | Required for working with containers (highly common in modern deployment). |
| **PostgreSQL** | `postgresql` | To install a local PostgreSQL server (if needed for the backend). |

**To include everything:**

```bash
choco install git vscode 7zip googlechrome nodejs-lts python3 dbeaver microsoft-windows-terminal docker-desktop postgresql -y
```
## The best way to install Chocolatey

The best way to install **Chocolatey** on a new computer is using an **elevated PowerShell window**, as it provides the most streamlined installation process.

Here are the complete steps:

-----

### ⚙️ How to Install Chocolatey

#### Step 1: Open PowerShell as Administrator

1.  Press the **Windows Key** and search for `PowerShell`.
2.  Right-click on **Windows PowerShell** (or Terminal) and select **Run as administrator**.
3.  Confirm the User Account Control (UAC) prompt.

#### Step 2: Set Execution Policy and Install

Paste and run the following two lines of code **sequentially**.

##### A. Set Execution Policy

This command ensures PowerShell allows the installation script to run (it's often restricted by default).

```powershell
Set-ExecutionPolicy AllSigned -Scope Process
```

##### B. Run the Installation Script

This command downloads and executes the official Chocolatey installation script.

```powershell
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Once the script finishes, you will see a message confirming the installation.

#### Step 3: Verify Installation

Close and reopen the PowerShell or Command Prompt window (this is crucial to ensure the Chocolatey folder is added to your system's $\text{PATH}$).

Run this command to confirm Chocolatey is ready:

```bash
choco
```

If the installation was successful, you will see the Chocolatey version information. You are now ready to run your bulk installation command\!

## When You typically see packages like 7zip and 7zip.install

That's a great question that points to a key distinction in how the Chocolatey community packages software.

You typically see packages like **`7zip`** and **`7zip.install`** because of how package maintainers manage updates and different installation methods. They are different ways to package the same application.

Here's the difference:

-----

### 📦 7zip (Metapackage)

The package simply named **`7zip`** (without the `.install` suffix) is typically a **metapackage**.

  * **What it does:** It acts as a permanent reference that points to the most current version of the installer package (e.g., `7zip.install`).
  * **Best for:** General installation and upgrades. When you run `choco upgrade all`, this metapackage ensures you always get the update from the underlying installer package.
  * **Chocolatey's recommendation:** Always use the simpler, non-suffixed package (`7zip`) unless you have a specific reason not to.

-----

### 📦 7zip.install (Installer Package)

The package named with the `.install` suffix, like **`7zip.install`**, directly contains or calls the logic to run the actual installer executable (the `.exe` or `.msi`) for the application.

  * **Why it exists:** It is primarily used to separate the actual installation logic from the general package name.
  * **Common Scenario:** Sometimes packages are structured this way so that a user could install the program using the official installer package (`7zip.install`), but then also install a separate **configuration package** (`7zip.config`) if needed.
  * **The Difference in Practice:** Both `choco install 7zip` and `choco install 7zip.install` will install the same program. The difference mostly matters to the package maintainers who manage the logic and versioning.

**In summary:** When installing or upgrading, you should generally just use the base name:

```bash
choco install 7zip -y
```

## choco install javaruntime or jre8 ?

When choosing between the Chocolatey packages to install the Java Runtime Environment (JRE), the best option for most users is **`javaruntime`**.

Here is a breakdown of why and the distinction between the two:

-----

### ⚙️ Recommended Command

Use the general package name for the most modern, automatically updated experience:

```bash
choco install javaruntime -y
```

-----

### 🆚 Package Differences

| Package Name | Purpose & Best Practice | Why You See It |
| :--- | :--- | :--- |
| **`javaruntime`** | **Recommended.** This package acts as an **alias** or **metapackage**. It usually points to the latest supported stable version of the JRE (e.g., JRE 17 or JRE 21), or allows you to specify a version if needed. It's the most flexible and future-proof option. | It ensures you get the current, most secure JRE without having to constantly update your install script's package ID. |
| **`jre8`** | **Specific Version.** This package is explicitly tied to **Java 8 (JRE 8)**. While Java 8 is still used for legacy applications, it is an older version. | It is primarily used when you have a legacy application that *specifically requires* Java 8 and will not run on a newer version. |

-----

### 🔑 Key Takeaway

Unless you know for a fact that your software requires the older **Java 8**, always use **`javaruntime`**.

Using `javaruntime` gives you the latest, most secure version of the JRE available through Chocolatey, which is best for general use and for compatibility with modern applications.

-----

If you need **both** the Java Development Kit (JDK) and the Java Runtime Environment (JRE) for different purposes, the best approach using Chocolatey is to install the appropriate **JDK** package.

Here's the plan and the recommended commands:

-----

### 1\. Install the Modern JDK (JRE Included)

The most efficient way to get both the runtime and the development tools is to install a modern **JDK** (Java Development Kit). The JDK package will automatically include the necessary **JRE** runtime component.

The best practice is to install the latest **LTS (Long-Term Support) version** of the JDK. OpenJDK is the community-driven, non-commercial implementation.

Run this in an **elevated (Administrator) terminal**:

```bash
choco install openjdk --version 21 -y
# OR, for the very latest LTS version:
choco install openjdk -y
```

| Package ID | What it Installs | Why it's Best |
| :--- | :--- | :--- |
| **`openjdk`** | The complete modern **JDK** (which includes the JRE/Runtime). | It gives you the latest Java tools for *developing* code and the *runtime* for executing Java applications. |

-----

### 2\. Install Legacy JRE (If Absolutely Required)

If you have a very specific, older application that *only* runs on the outdated **Java 8 JRE**, you can install that specific version separately, but this is generally only done for compatibility reasons.

Run this only if you have a legacy requirement:

```bash
choco install jre8 -y
```

**Note:** If you install both the modern `openjdk` and the older `jre8`, you may need to manage your **JAVA\_HOME** environment variable manually to switch between the two environments for compiling and running different applications.

-----

Yes, **OpenJDK version 8** absolutely exists and is still maintained with regular updates, primarily for applications that require compatibility with that specific, older version of the Java Platform.

#### OpenJDK 8 Availability on Chocolatey

For installation using Chocolatey, you can find and install OpenJDK 8 using specific package names:

1.  **To install the Java Development Kit (JDK) 8:**

    ```bash
    choco install openjdk8 -y
    ```

    *(Note: This package name is often tied to a specific maintainer or distribution like Adoptium (formerly AdoptOpenJDK), but it targets the OpenJDK 8 release.)*

2.  **To install just the Java Runtime Environment (JRE) 8:**

    ```bash
    choco install openjdk8jre -y
    # OR, as previously discussed:
    choco install jre8 -y
    ```

-----

#### Important Context on OpenJDK 8

  * **Legacy Requirement:** You should generally only install OpenJDK 8 if you have a legacy application or development environment that strictly requires the Java 8 version (e.g., specific versions of Minecraft, older enterprise applications, or build systems that haven't been updated).
  * **Maintenance:** OpenJDK 8 is a Long-Term Support (LTS) version and continues to receive security and bug fix updates from various providers (like Adoptium, Red Hat, Microsoft, etc.) under the OpenJDK project.
  * **Modern Alternative:** For all new development, it is highly recommended to use a newer LTS version like **OpenJDK 17** or **OpenJDK 21** as they include performance improvements and modern language features.

The process of installing older versions of Java using Chocolatey is demonstrated in this video. [How to Install Java JDK with Chocolatey](https://www.youtube.com/watch?v=KWXTqABiVQs) is relevant as it walks through using Chocolatey commands to install various Java JDK packages, which is useful when needing a specific version like OpenJDK 8.
http://googleusercontent.com/youtube_content/2

## choco list

That's a smart idea\! Before trying to install something with Chocolatey, you should always check if it's already on your system, or if Chocolatey already manages it.

Here are the commands you can use in your terminal (PowerShell or Command Prompt) to check if a program is installed:

-----

### 1\. Check if Chocolatey Manages the Package

The most reliable way to check if Chocolatey has managed the installation (or if the package is available) is using the `choco list` command:

#### Check by Package Name

Run this command, replacing `[package_name]` with the tool you're interested in (e.g., `nodejs-lts`, `git`, `yarn`, `openssl`):

```bash
choco list [package_name]
```

**Examples:**

| Command | Output if Installed | Output if NOT Installed |
| :--- | :--- | :--- |
| `choco list nodejs-lts` | `nodejs-lts 20.10.0` (Shows package name and version) | `Chocolatey v... 0 packages installed.` |
| `choco list yarn` | `yarn 1.22.21` (Shows package name and version) | `Chocolatey v... 0 packages installed.` |

-----

### 2\. Check if the Program Executable is on PATH

For command-line tools like `git`, `yarn`, `node`, `psql`, and `choco` itself, you can just run the command name. If the program is installed correctly and added to your system's $\text{PATH}$, it will execute.

| Tool | Check Command | Successful Output |
| :--- | :--- | :--- |
| **Node.js** | `node -v` | `v20.10.0` (or similar version number) |
| **Yarn** | `yarn -v` | `1.22.21` (or similar version number) |
| **Git** | `git --version` | `git version 2.43.0.windows.1` (or similar) |
| **Python** | `python --version` | `Python 3.12.0` (or similar) |
| **Psql** | `psql --version` | `psql (PostgreSQL) 16.0` (or similar) |
| **OpenSSL** | `openssl version` | `OpenSSL 3.0.12 24 Oct 2023` (or similar) |

If the program is **not installed**, the terminal will return an error like:

> 'node' is not recognized as an internal or external command, operable program or batch file.

-----

### 3\. Check for GUI Applications

For graphical applications like **VS Code** or **Power BI Desktop**, you can simply use the Windows Start menu search:

1.  Press the **Windows Key**.
2.  Start typing the application name (e.g., "**Visual Studio Code**" or "**Power BI**").
3.  If the app appears in the search results and you can open it, it's installed.

## Copy & Past

- Install chocolatey

```bash
$ Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

- Install all

```bash
# 1
$ choco install git vscode 7zip googlechrome nodejs-lts python3 -y
# 2
$ choco install adobereader firefox brave dbeaver microsoft-windows-terminal docker-desktop postgresql -y
# 3
$ choco install notepadplusplus vlc winrar putty libreoffice-fresh filezilla curl winscp jq vim nodejs-lts terraform -y
# 4
$ choco install postman openssl yarn opera anydesk maven openjdk8 openjdk tortoisegit -y
# 5
choco install pgadmin4 openoffice nvm sqlite soapui rabbitmq mobaxterm mongodb -y 
# 6
choco install forticlientvpn sublimetext3 openssl powerbi -y
```

## References

- https://chocolatey.org/install
- https://community.chocolatey.org/packages/