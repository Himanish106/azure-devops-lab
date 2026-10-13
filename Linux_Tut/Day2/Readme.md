
# Linux File System Structure — DevOps Notes

## 1. Understanding the Linux Terminal Prompt

When you log in to Linux, you may see a prompt like:

```bash
himanish3211@DESKTOP-IET4SNM:~$
```

### Breaking Down the Prompt

| Component | Meaning |
|-----------|---------|
| `himanish3211` | Username of the currently logged-in user |
| `@` | Separator between username and hostname |
| `DESKTOP-IET4SNM` | Hostname of the system |
| `:` | Separator between hostname and current directory |
| `~` | Represents the current user's home directory |
| `$` | Indicates a regular user's shell prompt |

### What Does `~` Mean?

The tilde (`~`) represents the home directory of the current user.

For example:

```bash
/home/himanish3211
```

If you log in as another user, their home directory may be:

```bash
/home/developer1
```

For the root user, the home directory is usually:

```bash
/root
```

### Useful Commands

Check the current directory:

```bash
pwd
```

Go to your home directory:

```bash
cd ~
```

Display your home directory:

```bash
echo $HOME
```

Check the current username:

```bash
whoami
```

---

# 2. Understanding the Linux Directory Structure

Linux follows a hierarchical directory structure.

The root of the entire filesystem is:

```bash
/
```

All other directories exist under this root directory.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

Let's understand each directory.

---

## A. Symbolic Links

A symbolic link is a special type of file that points to another file or directory.

In modern Ubuntu, `/bin` and `/sbin` are commonly symbolic links.

### i. `/sbin -> /usr/sbin`

Traditionally, `/sbin` contains system administration commands.

Examples include utilities for:

- Filesystem management
- Network administration
- System maintenance
- System recovery

On modern Ubuntu:

```text
/sbin -> /usr/sbin
```

**Important:** These commands are not exclusively accessible to administrators. Some can be executed by regular users, while privileged operations require root or sudo permissions.

### ii. `/bin -> /usr/bin`

Traditionally, `/bin` contains essential command-line utilities.

Examples:

```bash
ls
cp
mv
rm
cat
mkdir
```

On modern Ubuntu:

```text
/bin -> /usr/bin
```

This means `/bin` points to `/usr/bin`.

In simple words, `/usr/bin` contains many executable commands that we use regularly.

### Check Symbolic Links

```bash
ls -ld /bin /sbin
```

To check where a symbolic link points:

```bash
readlink -f /bin
```

---

## B. `/usr` — User Utilities and Applications

The `/usr` directory contains a large portion of the operating system's installed software, executable programs, libraries, and shared resources.

Important subdirectories:

| Directory | Purpose |
|-----------|---------|
| `/usr/bin` | General executable programs |
| `/usr/sbin` | System administration utilities |
| `/usr/lib` | Libraries used by applications |
| `/usr/share` | Architecture-independent shared resources |
| `/usr/local` | Software installed locally by administrators |

### Example

When you execute:

```bash
which ls
```

Output:

```text
/usr/bin/ls
```

This means the `ls` executable is located inside `/usr/bin`.

**Note:** `/usr` does not mean that it only contains user-installed software. It also contains software installed and managed by the Linux distribution.

---

## C. `/boot` — Boot Files

The `/boot` directory stores files required for booting the Linux operating system.

It may contain:

- Linux kernel images
- Initial RAM filesystem images (initramfs)
- Bootloader configuration files

Example:

```bash
ls -l /boot
```

### DevOps Note

On traditional Linux servers, `/boot` is important for kernel and bootloader management.



Therefore, `/boot` behaves differently and may contain fewer files.

---

## D. `/srv` — Service Data

`/srv` stands for service.

This directory is intended to store data served by services running on the system.

For example, if we host a website, its content could be stored in:

```text
/srv/www/
```

Or:

```text
/srv/mywebsite/
```

### Important

Installing a web server does not automatically mean its files will be stored in `/srv`.

For example, Nginx may use:

```text
/var/www/html
```

The actual location depends on the application and its configuration.

---

## E. `/opt` — Optional Third-Party Software

The `/opt` directory is conventionally used for installing optional add-on software.

For example:

```text
/opt/myapp/
```

It is commonly used for self-contained applications installed outside the distribution's normal package layout.

### Why Do We Use `/opt`?

Suppose you install an application inside:

```text
/home/himanish3211/myapp
```

Now imagine other users also need to access this application.

To provide access, an administrator might change permissions on the home directory.

If permissions are configured incorrectly, other users might gain access to personal files stored inside:

```text
/home/himanish3211/
```

This could expose:

- Personal documents
- Configuration files
- Sensitive application data
- Other private files

### Security Concern

Installing shared applications inside a user's home directory may create unnecessary security risks if directory permissions are relaxed.

### Better Approach

Install the shared application in:

```text
/opt/myapp
```

Then configure appropriate ownership and permissions.

### Important Points

1. `/opt` helps separate optional applications from personal user data.
2. It makes applications easier to organize and maintain.
3. It reduces the need to modify home-directory permissions.
4. Correct file ownership and permissions are still essential.
5. Installing software outside `/opt` is not automatically insecure.

**Key Takeaway:** `/opt` provides organizational separation, while Linux ownership and permissions provide security.

---

## F. Mount Points

A mount point is a directory where another filesystem is attached and made accessible.

### i. `/mnt` — Temporary or Manual Mount Points

The `/mnt` directory is conventionally used for temporarily mounting filesystems.

Example:

```text
/mnt/data
```

In WSL, Windows drives are commonly mounted under `/mnt`.

For example:

```text
/mnt/c
```

This typically provides access to the Windows C: drive.

### ii. `/media` — Removable Media

The `/media` directory is commonly used for removable storage devices.

Examples:

- USB drives
- External hard disks
- CDs/DVDs

Example:

```text
/media/username/USB
```

### iii. `/data` — Custom Data Directory

`/data` is not a mandatory standard Linux directory.

Administrators often create it to store application data or mount additional disks.

Example:

```text
/data
```

On an Azure Linux VM, an additional managed disk might be mounted at `/data`.

**Note:** `/data` does not automatically represent a Windows drive. The actual filesystem depends on what is mounted there.

### Useful Commands

View mounted filesystems:

```bash
df -h
```

View block devices:

```bash
lsblk
```

View filesystem mounts:

```bash
mount
```

---

## G. `/var` — Variable Data

The `/var` directory stores data that changes during normal system operation.

Examples include:

- Log files
- Cache files
- Application state
- Spool files
- Other variable data

### Important Subdirectories

| Directory | Purpose |
|-----------|---------|
| `/var/log` | System and application logs |
| `/var/cache` | Cached application data |
| `/var/lib` | Persistent application state |
| `/var/spool` | Queued tasks and spool data |
| `/var/tmp` | Temporary files intended to survive reboots |

### Example

To check logs:

```bash
cd /var/log
ls -l
```

Applications may create dedicated log directories.

Example:

```text
/var/log/nginx/
```

However, not every application creates a separate log file here. Some applications send logs to systemd-journald or other logging systems.

### DevOps Use Case

When troubleshooting an application failure, `/var/log` is often one of the first places to investigate.

---

## H. `/home` — User Home Directories

The `/home` directory normally contains personal directories for regular users.

Example:

```text
/home/
├── himanish3211/
├── developer1/
└── developer2/
```

Each user's home directory may contain:

- Personal files
- Downloads
- Shell configuration
- SSH configuration
- Application-specific settings

Example:

```text
/home/himanish3211/.bashrc
/home/himanish3211/.ssh/
```

### Security Note

Home directories should have appropriate permissions to protect private files.

Check permissions:

```bash
ls -ld /home/himanish3211
```

---

## I. `/proc` — Process Information

`/proc` is a virtual filesystem.

It provides information about:

- Running processes
- Kernel settings
- CPU information
- Memory information
- System statistics

The files are generated dynamically by the kernel rather than stored as ordinary files on disk.

### Useful Commands

Check CPU information:

```bash
cat /proc/cpuinfo
```

Check memory information:

```bash
cat /proc/meminfo
```

Check kernel version:

```bash
cat /proc/version
```

Check process information:

```bash
ls /proc
```

### DevOps Use Case

Useful for monitoring, performance troubleshooting, and investigating running processes.

---

## J. `/tmp` — Temporary Files

The `/tmp` directory is used for temporary files and directories.

Applications and users can create temporary data here.

Example:

```bash
mkdir /tmp/testfolder
```

### Important Points

- `/tmp` is generally writable by all users.
- It normally uses the sticky bit to restrict who can delete or rename files.
- Temporary files may be cleaned automatically.
- Cleanup behavior depends on system configuration.
- Files should not be expected to remain permanently.

**Security Note:** Avoid storing sensitive files in `/tmp` without appropriate protections.

---

## K. `/root` — Root User's Home Directory

`/root` is the default home directory of the root user.

The root user is the superuser in Linux.

Unlike normal users whose home directories are generally under `/home`, the root user's home directory is:

```text
/root
```

### Important

`/root` and `/` are different.

| Directory | Meaning |
|-----------|---------|
| `/` | Root of the entire Linux filesystem |
| `/root` | Home directory of the root user |

---

## L. `/run` — Runtime Data

The `/run` directory stores runtime information used by the operating system and services.

Examples include:

- Process ID (PID) files
- Unix domain sockets
- Service runtime state
- Lock-related runtime information

Example:

```bash
ls -l /run
```

### Important

`/run` is typically backed by a temporary filesystem.

Its contents are generally recreated after reboot.

### DevOps Use Case

Useful when troubleshooting service sockets, process state, and runtime files.

---

## M. `/etc` — System Configuration Files

The `/etc` directory contains system-wide configuration files.

These files control how Linux and its services behave.

For example:

- User account configuration
- Authentication
- Networking
- DNS resolution
- SSH configuration
- Filesystem mounts
- Service configuration
- Scheduled tasks

### Important Files and Directories

| File/Directory | Purpose |
|----------------|---------|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and password aging information |
| `/etc/group` | Group definitions |
| `/etc/hostname` | System hostname |
| `/etc/hosts` | Static hostname-to-IP mappings |
| `/etc/resolv.conf` | DNS resolver configuration |
| `/etc/fstab` | Filesystem mount configuration |
| `/etc/ssh/` | SSH client and server configuration |
| `/etc/systemd/` | Systemd configuration |
| `/etc/nginx/` | Nginx configuration, when installed |
| `/etc/cron.d/` | Scheduled task definitions |

### i. `/etc/passwd`

Stores basic user account information.

```bash
cat /etc/passwd
```

Example:

```text
himanish3211:x:1000:1000::/home/himanish3211:/bin/bash
```

The fields contain information such as:

- Username
- Password placeholder
- User ID (UID)
- Group ID (GID)
- User information
- Home directory
- Login shell

### ii. `/etc/shadow`

Stores sensitive password-related information.

Check permissions:

```bash
ls -l /etc/shadow
```

Ordinary users should not have permission to read this file.

### iii. `/etc/hosts`

Used for local hostname-to-IP mappings.

```bash
cat /etc/hosts
```

Example:

```text
127.0.0.1 localhost
10.10.10.20 appserver01
```

### iv. `/etc/resolv.conf`

Contains DNS resolver configuration.

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 10.0.0.4
```

In WSL2, this file may be automatically generated.

### v. `/etc/fstab`

Defines filesystems and their mount configurations.

```bash
cat /etc/fstab
```

This is particularly useful when configuring persistent mounts on Linux servers.

### vi. `/etc/ssh/`

Contains SSH configuration files.

For example:

```text
/etc/ssh/sshd_config
```

This file controls SSH server settings such as:

- SSH listening port
- Root login permissions
- Password authentication
- Public key authentication

### vii. `/etc/systemd/`

Contains configuration used by systemd.

On systems using systemd, administrators can place custom service unit files in:

```text
/etc/systemd/system/
```

### viii. `/etc/nginx/`

Contains Nginx configuration files when Nginx is installed.

Example:

```text
/etc/nginx/nginx.conf
```

### ix. `/etc/cron.d/`

Contains system-level cron task definitions.

These are used to schedule commands and scripts.

---

# 3. Linux Directory Structure — Quick Revision

| Directory | Purpose |
|-----------|---------|
| `/` | Root of the entire filesystem |
| `/bin` | Essential command utilities; commonly linked to `/usr/bin` |
| `/sbin` | Administration utilities; commonly linked to `/usr/sbin` |
| `/usr` | Installed software, utilities, libraries, and shared resources |
| `/boot` | Files required for system boot |
| `/srv` | Data served by system services |
| `/opt` | Optional third-party application packages |
| `/mnt` | Temporary or manual mount points |
| `/media` | Removable media mount points |
| `/var` | Variable data such as logs, caches, and application state |
| `/home` | Regular users' home directories |
| `/proc` | Virtual filesystem containing process and kernel information |
| `/tmp` | Temporary files |
| `/root` | Root user's home directory |
| `/run` | Runtime data |
| `/etc` | System-wide configuration files |

---

# 4. Important Linux Commands for Practice

```bash
# Check current directory
pwd

# Check current user
whoami

# Check hostname
hostname

# List root directory contents
ls /

# List configuration files
ls /etc

# Check home directory
echo $HOME

# Check executable location
which ls

# Check symbolic links
ls -ld /bin /sbin

# Check running kernel
uname -r

# Check mounted filesystems
df -h

# Check CPU information
cat /proc/cpuinfo

# Check memory information
cat /proc/meminfo

# Check DNS configuration
cat /etc/resolv.conf

# Check user accounts
cat /etc/passwd
```

---

# 5. Key Takeaways for DevOps

1. Linux follows a hierarchical filesystem structure starting from `/`.
2. `/usr/bin` contains many commonly used executable commands.
3. `/etc` contains system-wide configuration files.
4. `/var/log` is an important location for troubleshooting logs.
5. `/opt` is conventionally used for optional third-party applications.
6. `/home` stores regular users' personal files and configurations.
7. `/proc` provides live process and kernel information.
8. `/run` contains runtime information for processes and services.
9. `/mnt` and `/media` are commonly used as filesystem mount points.
10. Proper ownership and permissions are essential for securing Linux files and directories.

**Remember:**

- `/etc` = Configuration
- `/var/log` = Logs
- `/opt` = Optional software
- `/home` = User files
- `/usr/bin` = Executable commands
- `/proc` = Process and kernel information
- `/tmp` = Temporary files
- `/root` = Root user's home directory


# Linux Notes: PATH Environment Variable and Command Execution

## 1. Understanding How Linux Executes Commands

In Linux, commands such as `ls`, `cp`, `mkdir`, and `cat` are generally executable programs stored in specific directories.

For example:

```bash
which ls
```

Output:

```text
/usr/bin/ls
```

This indicates that the `ls` executable is located at `/usr/bin/ls`.

However, we can execute the same command in two ways:

```bash
ls
```

Or:

```bash
/usr/bin/ls
```

Both execute the `ls` program.

**Question:** If the executable is stored in `/usr/bin`, why does typing only `ls` work?

**Answer:** Because the shell uses the `PATH` environment variable to locate executable programs.

---

## 2. What Is the PATH Environment Variable?

`PATH` is an environment variable containing a list of directories where the shell searches for executable commands.

Check the current PATH:

```bash
echo $PATH
```

Example output:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### Understanding the Output

- `/usr/local/sbin` - Locally installed administrative commands
- `/usr/local/bin` - Locally installed executable programs
- `/usr/sbin` - System administration commands
- `/usr/bin` - General executable programs
- `/sbin` - System administration utilities
- `/bin` - Essential command-line utilities

**Important:** The directories are separated by colons (`:`).

The shell searches these directories in order when resolving an external command.

---

## 3. How Does Linux Find the `ls` Command?

Suppose we execute:

```bash
ls
```

The process is:

1. Bash receives the command `ls`.
2. Bash checks whether it is an alias, function, or shell builtin.
3. If an external executable is required, Bash resolves it using PATH, potentially using its command hash table.
4. The executable `/usr/bin/ls` is found.
5. Bash executes the program.
6. The program displays the contents of the current directory.

### Execution Flow

```text
User types: ls
       |
       v
Bash receives command
       |
       v
Checks command resolution
       |
       v
Searches PATH for executable
       |
       v
Finds /usr/bin/ls
       |
       v
Executes ls
       |
       v
Displays directory contents
```

### Important Observation

When executing:

```bash
/usr/bin/ls
```

Bash does not need to search PATH because the executable location is explicitly provided.

---

## 4. Difference Between Absolute Path and Command Name

### Command Using PATH

```bash
ls
```

The shell resolves the executable through its command lookup mechanism.

### Command Using Absolute Path

```bash
/usr/bin/ls
```

The shell executes the program using its explicitly specified location.

### Comparison

| Command | PATH Search Required? |
|---------|-----------------------|
| `ls` | Yes, for external command lookup |
| `/usr/bin/ls` | No |
| `cat` | Yes, for external command lookup |
| `/usr/bin/cat` | No |
| `./script.sh` | No |

**Note:** `./script.sh` uses a relative path pointing to a script in the current directory.

---

## 5. How to Check Where a Command Is Located

### Method 1: Using `which`

```bash
which ls
```

Output:

```text
/usr/bin/ls
```

This displays the executable found through PATH.

### Method 2: Using `type`

```bash
type ls
```

Example output:

```text
ls is aliased to 'ls --color=auto'
```

To display all known resolutions:

```bash
type -a ls
```

Example:

```text
ls is aliased to 'ls --color=auto'
ls is /usr/bin/ls
ls is /bin/ls
```

`type` is especially useful because it can identify aliases, shell functions, builtins, and executable paths.

---

## 6. How PATH Works With Third-Party Software

Suppose Terraform is installed manually in:

```text
/opt/terraform/terraform
```

If `/opt/terraform` is not included in PATH, executing:

```bash
terraform
```

May produce:

```text
terraform: command not found
```

However, this works:

```bash
/opt/terraform/terraform
```

### Solution: Add the Directory to PATH

```bash
export PATH="$PATH:/opt/terraform"
```

Now execute:

```bash
terraform
```

The shell can locate the executable.

### Important Notes

- `export` makes the variable available to child processes.
- This modification applies to the current shell session and processes launched from it.
- It does not permanently modify PATH.
- New terminal sessions will not automatically inherit this modification.

### Making PATH Persistent

For an interactive Bash environment, add the export statement to the appropriate Bash startup file, such as `~/.bashrc`:

```bash
export PATH="$PATH:/opt/terraform"
```

Then reload it:

```bash
source ~/.bashrc
```

**Security best practice:** Avoid adding untrusted or world-writable directories to PATH, especially before trusted system directories.

---

## 7. Difference Between Current Directory and PATH

Consider the terminal:

```text
himanish3211@DESKTOP-IET4SNM:/$
```

The `/` indicates the current working directory.

Check it using:

```bash
pwd
```

Output:

```text
/
```

Now execute:

```bash
ls
```

The command lists files in `/`.

Next:

```bash
cd /etc
ls
```

The command now lists files in `/etc`.

### Why?

Because:

- PATH determines where the shell searches for executables.
- The current working directory determines where relative file paths are resolved.
- Running `ls` without arguments lists the current directory.

Even after changing directories, the executable can still be `/usr/bin/ls`.

---

## 8. Important Commands to Remember

| Command | Purpose |
|---------|---------|
| `pwd` | Display current working directory |
| `echo $PATH` | Display PATH variable |
| `which ls` | Find executable using PATH |
| `type ls` | Identify how Bash resolves a command |
| `type -a ls` | Show all known command resolutions |
| `command -v ls` | Show how the shell resolves a command |
| `export PATH=...` | Modify PATH for current shell environment |
| `source ~/.bashrc` | Execute Bash configuration in current shell |
| `readlink -f /bin` | Resolve symbolic link to its final path |


## 9. Key Takeaways

1. Linux executable programs are stored in directories such as `/usr/bin`.
2. PATH contains directories used to locate external commands.
3. A command such as `ls` can work without its full path because the shell resolves its location.
4. Absolute executable paths bypass PATH lookup.
5. `type -a` is useful for understanding command resolution.
6. `export PATH=...` can add custom executable directories.
7. PATH configuration is important when installing DevOps tools.
8. The current working directory and PATH serve different purposes.
9. On modern Ubuntu, `/bin` is commonly linked to `/usr/bin`.
10. Avoid placing untrusted writable directories early in PATH.

**Final Concept:**

`PATH` tells the shell where to look for commands, while `pwd` tells you where you are currently working.
