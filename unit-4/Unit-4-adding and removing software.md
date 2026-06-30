# Unit 4: Adding & Removing Software (APT)

## Introduction to APT
APT (**Advanced Package Tool**) is the default package manager for Debian-based Linux distributions such as Ubuntu and Kali Linux.

### Features
- Install software packages
- Update package lists
- Upgrade installed software
- Remove software packages
- Resolve dependencies automatically
- Search repositories efficiently

---

# Package Management Architecture

```text
User
  ↓
APT Commands
  ↓
Package Database
  ↓
Repositories
  ↓
Software Installation
```

---

# Searching for Packages

## Syntax

```bash
apt search <package-name>
apt-cache search <package-name>
```

## Examples

```bash
apt search nmap
apt-cache search snort
apt search scanner
apt search python
```

---

# Viewing Package Information

```bash
apt show <package-name>
```

Example:

```bash
apt show nmap
```

Displays:
- Package version
- Dependencies
- Description
- Maintainer information
- Package size

---

# Installing Software

## Syntax

```bash
sudo apt install <package-name>
apt-get install <package-name>
```

## Example

```bash
sudo apt install nmap
```

### Installation Workflow

1. Search package
2. Download package
3. Resolve dependencies
4. Install package
5. Verify installation

---

# Removing Software

## Remove Package

```bash
sudo apt remove <package-name>
```

Removes the package but keeps configuration files.

## Purge Package

```bash
sudo apt purge <package-name>
```

Removes the package and configuration files.

## List Installed Packages

```bash
apt --list installed
```

## Remove Unused Dependencies

```bash
apt autoremove
```

## Clean Package Cache

```bash
apt clean
```

---

# Updating Packages

Updates repository information.

```bash
sudo apt update
apt-get update
```

---

# Upgrading Packages

Upgrades installed software packages.

```bash
sudo apt upgrade
sudo apt full-upgrade
apt-get upgrade
```

---

# Common Maintenance Commands

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt update && sudo apt full-upgrade -y
```

```bash
sudo apt autoremove && sudo apt clean
```

---

# Repositories

A repository is an online software storage location that contains packages and metadata.

Repository information is stored in:

```text
/etc/apt/sources.list
```

---

# Repository Categories

## Main
Officially supported open-source software.

## Universe
Community-maintained open-source software.

## Multiverse
Software restricted by copyright or legal issues.

## Restricted
Proprietary drivers and software.

## Backports
Packages from newer releases.

---

# GUI Package Managers

## Synaptic Package Manager
Graphical interface for package management.

## GDebi
GUI tool used to install `.deb` packages.

---

# Installing Software from Git

Clone a repository:

```bash
git clone <repository-url>
```

After cloning, follow the project's installation instructions.

---

# APT vs GUI Tools

| APT | GUI Tools |
|------|----------|
| Terminal-based | Graphical interface |
| Fast and scriptable | Easy for beginners |
| Preferred by administrators | Preferred by new users |

---

# Best Practices

- Run `sudo apt update` before installing packages.
- Use official repositories whenever possible.
- Remove unused packages regularly.
- Keep the system updated.
- Verify package names before installation.

# Summary

APT is a powerful package management system used in Debian-based Linux distributions. It simplifies software installation, updating, upgrading, removal, repository management, and system maintenance.
