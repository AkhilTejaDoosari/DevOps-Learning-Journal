# 🐧 Day 08 – User & Group Management

## Table of Contents
- [1. Introduction to Users](#1-introduction-to-users)  
- [2. Key Commands Overview](#2-key-commands-overview)  
- [3. Switching Users (`su` vs `sudo`)](#3-switching-users-su-vs-sudo)  
- [4. User Management Files](#4-user-management-files)  
- [5. UID Ranges](#5-uid-ranges)  
- [6. Adding / Modifying / Deleting Users](#6-adding--modifying--deleting-users)  
- [7. The `sudoers` File](#7-the-sudoers-file)  
- [8. Password Aging & Expiration](#8-password-aging--expiration)  
- [9. `su` vs `su -`](#9-su-vs-su-)  
- [10. Group Management](#10-group-management)

---

<details>
<summary><strong>1. Introduction to Users</strong></summary>

## Theory & Notes
- **UID**: Unique User ID identifies each user.  
- **GID**: Group ID identifies the primary group.  
- When you create a user, a **primary group** with the same name is also created.  
- **Root** (UID 0): super-user with full system access.  
- **Regular** (UID ≥ 1000): day-to-day accounts with limited privileges.

</details>

---

<details>
<summary><strong>2. Key Commands Overview</strong></summary>

## Theory & Notes
These commands help inspect identity and elevate privileges:

| Command | Purpose                                 | Syntax Example   |
| ------- | --------------------------------------- | ---------------- |
| `id`    | Display current user’s UID, GID, groups | `id`             |
| `su`    | Switch to another user                  | `su <username>`  |
| `sudo`  | Execute a command as root temporarily   | `sudo <command>` |

</details>

---

<details>
<summary><strong>3. Switching Users (`su` vs `sudo`)</strong></summary>

## Theory & Notes
- `su <user>`: switch user, keep current environment.  
- `su - <user>`: login shell, load user’s environment.  
- `sudo -i`: start a root login shell.

```bash
# Become root shell:
sudo -i

# Switch to Akhil:
su akhil-teja-doosari

# Verify identity:
id
````

</details>

---

<details>
<summary><strong>4. User Management Files</strong></summary>

## Theory & Notes

* These files control user accounts and credentials.
* Editing `/etc/sudoers` should always be done via `sudo visudo` for safety.

| File           | Purpose                                              | Access    |
| -------------- | ---------------------------------------------------- | --------- |
| `/etc/passwd`  | Stores user details: username, UID, GID, home, shell | read-all  |
| `/etc/shadow`  | Contains hashed passwords & aging settings           | root only |
| `/etc/skel`    | Template files copied to new user’s home             | root only |
| `/etc/sudoers` | Defines sudo permissions                             | root only |

```bash
# List all user accounts
echo "All users:"; cat /etc/passwd
```

</details>

---

<details>
<summary><strong>5. UID Ranges</strong></summary>

## Theory & Notes

Determines account purpose:

| Range     | Purpose                             |
| --------- | ----------------------------------- |
| `0`       | Superuser (root)                    |
| `1–200`   | System accounts (services, daemons) |
| `201–999` | Unprivileged system processes       |
| `1000+`   | Regular user accounts               |

</details>

---

<details>
<summary><strong>6. Adding / Modifying / Deleting Users</strong></summary>

## Theory & Notes

* `useradd`: creates account and default settings.
* `passwd`: sets/updates password in `/etc/shadow`.
* `usermod`: modify properties (`-l` login, `-u` UID, `-s` shell).
* `userdel`: deletes account (`--remove` also removes home & mail).

```bash
# Syntax: add user
sudo useradd <username>
# Example:
sudo useradd navya

# Syntax: set password
sudo passwd <username>
# Example:
sudo passwd navya

# Syntax: list users
cat /etc/passwd

# Syntax: change login name
sudo usermod -l <newname> <oldname>
# Example:
sudo usermod -l atd akhil-teja-doosari

# Syntax: change UID
sudo usermod -u <UID> <username>
# Example:
sudo usermod -u 2000 navya

# Syntax: change shell
sudo usermod -s <shell_path> <username>
# Example:
sudo usermod -s /bin/zsh navya

# Syntax: delete user (keep home)
sudo userdel <username>
# Example:
sudo userdel navya

# Syntax: delete user + home
sudo userdel <username> --remove
# Example:
sudo userdel navya --remove 
```

</details>

---

<details>
<summary><strong>7. The <code>sudoers</code> File</strong></summary>

## Theory & Notes

Controls which users may run commands as root.
Always edit via `sudo visudo` to validate syntax.

```text
# /etc/sudoers entries:
root    ALL=(ALL) ALL
navya   ALL=(ALL) ALL
```

</details>

---

<details>
<summary><strong>8. Password Aging & Expiration</strong></summary>

## Theory & Notes
- Linux tracks each user’s password age and enforces expiration policies via the **`/etc/shadow`** file.  
- The **`chage`** command lets you **view** and **modify** these settings.  
- You can also use **`passwd`** flags to **force expire**, **lock**, or **unlock** an account immediately.

---

### View Current Aging Settings

```bash
# Show password age, expiration, and warning info for user
sudo chage -l <username> (`ch` means change )
# Example:
sudo chage -l navya

# Sample output:
# Last password change                    : Jun 23, 2025
# Password expires                        : never
# Password inactive                       : never
# Account expires                         : never
# Minimum number of days between changes  : 0
# Maximum number of days between changes  : 99999
# Number of days of warning before expiry : 7
````

---

### Force Password Expiration

```bash
# Immediately expire the password so user must change on next login
sudo passwd -e <username>
# Example:
sudo passwd -e navya

# Then verify:
sudo chage -l navya
# "Password expires: password must be changed"
```

**Use case:** Enforce a password reset after policy updates or suspected compromise.

---

### Set Maximum Password Age

```bash
# Set the maximum number of days a password is valid
sudo chage -M <days> <username>
# Example: expire after 30 days
sudo chage -M 30 navya

# Verify:
sudo chage -l navya
# "Password expires: Jul 23, 2025"
```

**Use case:** Enforce regular password rotation (e.g., every 30, 60, or 90 days).

---

### Lock & Unlock an Account

```bash
# Lock user’s password (prevents login via password)
sudo passwd -l <username>
# Example:
sudo passwd -l navya

# Attempted login will fail:
su navya
# su: Authentication failure

# Unlock user’s password
sudo passwd -u <username>
# Example:
sudo passwd -u navya

# User can log in again:
su navya
```

**Use case:** Temporarily disable an account without deleting it (e.g., during extended leave).

</details>

---

<details>
<summary><strong>9. <code>su</code> vs <code>su -</code></strong></summary>

## Theory & Notes

* `su <user>`: switch user, keep current environment.
* `su - <user>`: login shell, full user environment.

| Command     | Behavior                          |
| ----------- | --------------------------------- |
| `su user`   | Switch user, preserve environment |
| `su - user` | Full login shell, new environment |

</details>

---

<details>
<summary><strong>10. Group Management</strong></summary>

## Theory & Notes
- **User group**: Every new user automatically gets a primary group with the same name; files they create default to that group’s ownership.  
- **Supplementary groups**: Additional groups granting shared-permission access; managed independently of the primary group.  
- **Core concepts**:  
  - `addgroup` / `groupadd`: create a new group entry in `/etc/group`  
  - `groupmod`: modify existing group properties (e.g., rename)  
  - `/etc/group`: lists all groups and their members  

---

### Create New Group
```bash
# Syntax:
sudo groupadd <groupname>
# Example:
sudo groupadd avengers
````

---

### Add User to a Group

```bash
# Syntax (-aG appends to existing groups):
sudo usermod -aG <groupname> <username>
# Example:
sudo usermod -aG avengers navya
```

---

### Check User’s Group Membership

```bash
# List all groups and their members:
cat /etc/group

# List groups for a specific user:
groups <username>
# Example:
groups navya
```

---

### Display Users in a Group

```bash
# Using getent:
getent group <groupname>
# Example:
getent group avengers

# Or legacy 'lid' command:
sudo lid -g <groupname>
# Example:
sudo lid -g avengers
```

---

### Remove User from a Group

```bash
# Syntax:
sudo gpasswd -d <username> <groupname>
# Example:
sudo gpasswd -d navya avengers
```

---

### Change Group Name

```bash
# Syntax:
sudo groupmod -n <newname> <oldname>
# Example:
sudo groupmod -n nakama avengers
```

---

### Delete a Group

```bash
# Syntax:
sudo groupdel <groupname>
# Example:
sudo groupdel avengers
```

</details>