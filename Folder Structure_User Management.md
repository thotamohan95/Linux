# Linux Folder Structure and User Management

---

## Part 1 — Understanding the Folder Structure

### Symbolic links (common)
- `/sbin -> /usr/sbin` — System binaries for administrative commands (linked to `/usr/sbin`).
- `/bin -> /usr/bin` — Essential user binaries (linked to `/usr/bin`).
- `/lib -> /usr/lib` — Shared libraries and kernel modules (linked to `/usr/lib`).

### Important system directories
| Directory | Description |
|---|---|
| `/boot` | Files needed for booting the system (not relevant in containers). |
| `/usr` | Contains most user-installed applications and libraries. |
| `/var` | Stores logs, caches, and frequently changing temporary files. |
| `/etc` | System configuration files. |

### User & application-specific directories
| Directory | Description |
|---|---|
| `/home` | Default location for user home directories. |
| `/opt` | Optional third-party software. |
| `/srv` | Data for services like web servers (rarely used in containers). |
| `/root` | Home directory for the root user. |

### Temporary & virtual filesystems
| Directory | Description |
|---|---|
| `/tmp` | Temporary files (may be cleared on reboot). |
| `/run` | Runtime data for processes. |
| `/proc` | Virtual filesystem for process and system information. |
| `/sys` | Virtual filesystem for hardware and kernel information. |
| `/dev` | Device files (e.g., `/dev/null`, `/dev/sda`). |

### Mount points
| Directory | Description |
|---|---|
| `/mnt` | Temporary mount point for external filesystems. |
| `/media` | Mount point for removable media (USB, CDs). |
| `/data` | Possibly a mounted volume from Windows (e.g., `C:/ubuntu-data`). |

---

## Part 2 — User Management in Linux

Linux is a multi-user OS. Proper user management ensures security, controlled access, and system integrity.

### Key files involved in user management
- `/etc/passwd` — User account details (one line per account).
- `/etc/shadow` — Encrypted user passwords and password metadata.
- `/etc/group` — Group information.
- `/etc/gshadow` — Secure group administration data.

### Creating users

- Using `useradd` (available on most distributions)
  - Create user without a home directory:
    ```bash
    useradd username
    ```
  - Create user with a home directory:
    ```bash
    useradd -m username
    ```
  - Create user with a specific shell:
    ```bash
    useradd -s /bin/bash username
    ```

- Using `adduser` (Debian/Ubuntu friendly; interactive)
  ```bash
  adduser username
  ```

### Managing passwords

- Set or change password:
  ```bash
  passwd username
  ```

- Enforce password policies
  - Set maximum password age (expire after N days):
    ```bash
    chage -M 90 username
    ```
  - Lock an account:
    ```bash
    passwd -l username
    ```
  - Unlock an account:
    ```bash
    passwd -u username
    ```

### Modifying users (`usermod`)
- Change username:
  ```bash
  usermod -l new_username old_username
  ```
- Change home directory and move contents:
  ```bash
  usermod -d /new/home/directory -m username
  ```
- Change default shell:
  ```bash
  usermod -s /bin/zsh username
  ```

### Deleting users (`userdel`)
- Remove user but keep home directory:
  ```bash
  userdel username
  ```
- Remove user and their home directory:
  ```bash
  userdel -r username
  ```

### Working with groups
- Create a group:
  ```bash
  groupadd groupname
  ```
- Add a user to a supplementary group (append):
  ```bash
  usermod -aG groupname username
  ```
- View a user's groups:
  ```bash
  groups username
  ```
- Change a user's primary group:
  ```bash
  usermod -g new_primary_group username
  ```

### Sudo access and privilege escalation
- Add user to sudo group (Debian/Ubuntu):
  ```bash
  usermod -aG sudo username
  ```
- Add user to wheel group (RHEL/Fedora/CentOS):
  ```bash
  usermod -aG wheel username
  ```

- Grant specific commands without a password:
  1. Edit the sudoers file safely:
     ```bash
     visudo
     ```
  2. Add a line such as:
     ```
     username ALL=(ALL) NOPASSWD: /path/to/command
     ```

---

If you'd like, I can:
- Commit this formatted file back to the repository (I will need confirmation of the target repo and branch), or
- Further adjust formatting (for example, add examples for /etc/passwd fields, or include best-practices for locking inactive accounts).
