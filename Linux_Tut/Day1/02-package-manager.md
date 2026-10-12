What is a package manager?

A packet manager is a tool that automates the process of installing,updating,configuring,removing software in a linux environment. It ensures that software and its dependencies are managed efficiently.


How does a Packet Manager works?

1. Repositories:

i) A package manager fetches software from official repositories (online storage of packages).
ii) Example: Ubuntu gets packages from archive.ubuntu.cpm

2. Installing Software: When you install a software, the package manager:

i) Downloads the package from the repositories
ii) Resolves Dependencies (installs additional required software).
iii) Installs and configures the software automatically.

3. Updating Software: A single command updates all installed packages to the latest version.

4. Removing Software: The package manager also removes software cleanly without leaving unnecessary files.



## Popular Package Managers in Linux

| Linux Distro | Package Manager | Command Example |
|---|---|---|
| Ubuntu, Debian | apt (Advanced Package Tool) | `sudo apt install nginx` |
| Fedora, RHEL, CentOS | dnf (or yum for older versions) | `sudo dnf install nginx` |
| Arch Linux | pacman | `sudo pacman -S nginx` |
| OpenSUSE | zypper | `sudo zypper install nginx` |


How Package Managers Fetch software from repositories?

A repository is a server that stores software packages. When a package manager installs software:

a) It checks the repository list (e.g. /etc/apt/sources.list in Ubuntu)

b) It downloads the package and its dependencies

c) It installs and configures the software automatically.


Example of an Ubuntu Repository Entry

Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports/
Suites: noble noble-updates noble-backports noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Why Should You Run apt update After Installing Ubuntu?

When you install Ubuntu, the packages included in the ISO image might be outdated. Running:

apt install sudo
sudo apt update

These shows a number of packages of a repository that can be updated

To view which specific packages can be updated:

apt list --upgradable

Then to install the latest version of packages run

sudo apt upgrade -y


## Essential Package Manager Commands in Linux

APT (Debian, Ubuntu)

sudo apt update         # Update package lists
sudo apt upgrade -y     # Upgrade installed packages
sudo apt install nginx  # Install a package
sudo apt remove nginx   # Remove a package
sudo apt autoremove     # Remove unused dependencies
sudo apt search nginx   # Search for a package

DNF (Fedora, RHEL, CentOS)

sudo dnf check-update   # Check for updates
sudo dnf update         # Update all packages
sudo dnf install nginx  # Install a package
sudo dnf remove nginx   # Remove a package

Pacman (Arch Linux)

sudo pacman -Syu        # Sync and update all packages
sudo pacman -S nginx    # Install a package
sudo pacman -R nginx    # Remove a package

Zypper (OpenSUSE)

sudo zypper refresh     # Refresh package list
sudo zypper update      # Update all packages
sudo zypper install nginx  # Install a package
sudo zypper remove nginx   # Remove a package

## Best Practices for Using Package Managers

Always update your package list before installing software:

sudo apt update && sudo apt upgrade -y

Use autoremove to clean up unused dependencies:

sudo apt autoremove

Enable automatic security updates (Ubuntu):

sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades