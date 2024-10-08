# Chocolatey

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

## References

- https://chocolatey.org/install
- https://community.chocolatey.org/packages/docker-desktop
- https://community.chocolatey.org/packages/hugo
- https://community.chocolatey.org/packages/git.install
- https://chocolatey.org/install
- https://community.chocolatey.org/packages/docker-desktop
- https://community.chocolatey.org/packages/hugo
- https://community.chocolatey.org/packages/git.install
- https://community.chocolatey.org/packages/docker-desktop#files
- https://chocolatey.org/install
- https://community.chocolatey.org/packages/docker-desktop
- https://community.chocolatey.org/packages/hugo
- https://community.chocolatey.org/packages/git.install