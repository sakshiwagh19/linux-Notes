# Linux User and Group Administration

### Important Terms

- **User:** An account used by a person or service to access Linux.
- **Group:** A collection of users. Groups make it easier to manage permissions for multiple users.
- **Root User:** The administrator account. Its UID (User ID) is `0`.
- **UID:** A numeric ID assigned to a user.
- **GID:** A numeric ID assigned to a group.
- **Primary Group:** The main group associated with a user.
- **Supplementary Groups:** Additional groups to which a user belongs.

Most user and group administration commands require root privileges. Run them as root or use `sudo`.

### sudo Command

For example:

```bash
sudo useradd bhaskar
```

---

## 2. Important User and Group Files

Linux stores local account information in important files under `/etc`.

### 2.1 `/etc/passwd` – User Account Information

View user account entries:

```bash
cat /etc/passwd
```

Example entry:

```text
bhaskar:x:1001:1001:Bhaskar:/home/bhaskar:/bin/bash
```

The fields are separated by colons (`:`).

| Field | Example | Meaning |
|---|---|---|
| Username | bhaskar | Login name |
| Password marker | x | Password hash is stored in `/etc/shadow` |
| UID | 1001 | User ID |
| GID | 1001 | Primary group ID |
| Comment | Bhaskar | User description / GECOS field |
| Home directory | /home/bhaskar | User's home directory |
| Login shell | /bin/bash | Shell started at login |

**Important:** The `x` does not mean the password is literally `x`. It indicates that password information is stored separately in `/etc/shadow`.

### 2.2 `/etc/shadow` – Password and Password-Aging Information

View it as root:

```bash
sudo cat /etc/shadow
```

This file stores password hashes and password-aging fields.

**Note:** Do not share its contents. Ordinary users normally cannot read it.

### 2.3 `/etc/group` – Group Information

View group entries:

```bash
cat /etc/group
```

Example:

```text
devops:x:1002:bhaskar
```

Fields:

- Group name – `devops`
- Password marker – `x`
- GID – `1002`
- Supplementary member list – `bhaskar`

A user's primary-group membership is recorded through the GID in `/etc/passwd`. The member list in `/etc/group` is mainly used to list supplementary members.

### 2.4 UID and GID

- Root always has UID `0`.
- On many modern RHEL-based systems, regular user IDs commonly start at `1000`.
- Older RHEL releases commonly used `500` as the starting point for regular users.
- The exact next UID/GID depends on the distribution configuration and IDs already in use.

Check a user's IDs:

```bash
id bhaskar
```

---

## 3. User Management Commands

### 3.1 `useradd` – Create a User

**Syntax:**

```bash
sudo useradd username
```

Example:

```bash
sudo useradd bhaskar
```

To explicitly create the user's home directory:

```bash
sudo useradd -m bhaskar
```

To specify a login shell:

```bash
sudo useradd -m -s /bin/bash bhaskar
```

To specify a UID:

```bash
sudo useradd -u 1500 -m bhaskar
```

Check whether the user was created:

```bash
id bhaskar
grep '^bhaskar:' /etc/passwd
ls -ld /home/bhaskar
```

**Remember:** `-m` asks `useradd` to create the home directory. Whether a home directory is created by default depends on system configuration.

### 3.2 `usermod` – Modify an Existing User

`usermod` changes an existing user's account attributes.

**Change the user's login shell:**

```bash
sudo usermod -s /bin/bash bhaskar
```

**Change the user's home directory path:**

```bash
sudo usermod -d /home/bhaskar bhaskar
```

**Move existing home-directory contents while changing the home path:**

```bash
sudo usermod -d /newhome/bhaskar -m bhaskar
```

**Add a user to a supplementary group:**

```bash
sudo usermod -aG devops bhaskar
```

**Change the primary group:**

```bash
sudo usermod -g devops bhaskar
```

**Lock and unlock password-based login:**

```bash
sudo usermod -L bhaskar
sudo usermod -U bhaskar
```

- `-L` locks the user's password by adding a lock marker to the password field.
- `-U` unlocks it.

These options affect password authentication; they do not necessarily disable every possible way of accessing an account.

#### Important Difference Between `-G` and `-aG`

```bash
sudo usermod -G devops bhaskar
```

`-G` sets the supplementary-group list. Groups not included in the new list may be removed.

```bash
sudo usermod -aG devops bhaskar
```

`-aG` appends the user to the supplementary group and keeps existing supplementary memberships.

**Usually use `-aG` when you only want to add one group.**

### 3.3 `id` – Display User IDs and Groups

```bash
id
id bhaskar
```

Example output:

```text
uid=1001(bhaskar) gid=1001(bhaskar) groups=1001(bhaskar),1002(devops)
```

Meaning:

- `uid=1001`: User's UID.
- `gid=1001`: User's primary GID.
- `groups=...`: Primary and supplementary group memberships.

### 3.4 `groups` – Display Group Membership

```bash
groups
groups bhaskar
```

This displays the groups associated with the current user or the specified user.

### 3.5 `lid` – List User/Group Information (If Installed)

On systems that provide the libuser utilities, `lid` can display users belonging to a group or groups associated with a user.

Examples:

```bash
sudo lid -g devops
sudo lid -u bhaskar
```

The command may not be installed on every Linux distribution.

Alternatives:

```bash
id bhaskar
getent group devops
```

### 3.6 `getent` – Read Account Information

```bash
getent passwd bhaskar
getent group devops
```

`getent` looks up account information through the system's configured name-service sources.

It is often more suitable than reading only `/etc/passwd` or `/etc/group`, especially when accounts come from a network directory service.

---

## 4. Password Management

### 4.1 `passwd` – Set or Change a Password

A normal user changes their own password:

```bash
passwd
```

The system asks for the current password and then the new password (usually twice for confirmation).

A root user or an administrator with suitable privileges can set another user's password:

```bash
sudo passwd bhaskar
```

In this case, the administrator is prompted to enter a new password for `bhaskar`. The user's old password is not requested.

**Set or change the root password:**

```bash
sudo passwd root
```

**Check password status:**

```bash
sudo passwd -S bhaskar
```

The status output indicates whether the password is locked, has a usable password, or has no password set.

**Important:** Passwords are not stored as plain text in `/etc/shadow`. Linux stores password hashes there.

---

## 5. Password Aging with `chage`

### 5.1 What is `chage`?

`chage` means change age. It is used to view or configure password-aging rules, such as:

- Minimum number of days before a password can be changed again.
- Maximum number of days a password can be used.
- Number of warning days before password expiry.
- Number of inactive days after password expiry before the account is disabled.
- Account expiration date.

**View a user's password-aging details:**

```bash
sudo chage -l bhaskar
```

**Open interactive password-aging settings:**

```bash
sudo chage bhaskar
```

### 5.2 Important `chage` Options

| Option | Meaning |
|---|---|
| `-m` | Minimum number of days between password changes |
| `-M` | Maximum number of days a password can be used |
| `-W` | Warning days before password expiry |
| `-I` | Inactive days after password expiry before the password is disabled |
| `-E` | Account expiration date |
| `-l` | List current password-aging information |

**Example:**

```bash
sudo chage -m 0 -M 90 -W 14 bhaskar
```

Meaning:

- `-m 0`: The user does not have to wait before changing the password.
- `-M 90`: The password expires after 90 days.
- `-W 14`: The user receives a warning 14 days before expiry.

**Set an account expiration date:**

```bash
sudo chage -E 2027-12-31 bhaskar
```

**Remove the account-expiration date:**

```bash
sudo chage -E -1 bhaskar
```

**Disable the post-expiry inactivity period:**

```bash
sudo chage -I -1 bhaskar
```

### 5.3 Understand `-1`

For options that accept `-1` as a special value, it generally means that the corresponding limit is disabled or unset.

For example:

```bash
sudo chage -E -1 bhaskar
```

This removes the account-expiration date. It does not mean that the account is immediately disabled.

**Do not confuse these two settings:**

- **Password expiration:** The password reaches its maximum allowed age.
- **Account expiration:** The account itself reaches its configured expiration date.

---

## 6. Group Management

Groups are used to organize users and manage access permissions.

### 6.1 `groupadd` – Create a Group

**Syntax:**

```bash
sudo groupadd groupname
```

Example:

```bash
sudo groupadd devops
```

Check the group:

```bash
getent group devops
```

### 6.2 `usermod -aG` – Add a User to a Supplementary Group

```bash
sudo usermod -aG devops bhaskar
```

This adds `bhaskar` to the supplementary group `devops` without replacing the user's other supplementary groups.

Verify:

```bash
id bhaskar
groups bhaskar
getent group devops
```

A user may need to log out and log in again to receive the updated group membership in a new login session.

### 6.3 Primary Group vs Supplementary Group

**Primary Group:**

The default group associated with the user's files and processes when no other group is specified.

**Supplementary Group:**

An additional group that gives the user access to resources assigned to that group.

**Change the primary group:**

```bash
sudo usermod -g devops bhaskar
```

**Set the supplementary-group list (replaces the previous supplementary list):**

```bash
sudo usermod -G devops bhaskar
```

**Append a supplementary group (recommended when adding one group):**

```bash
sudo usermod -aG devops bhaskar
```

### 6.4 `gpasswd` – Manage Group Membership

**Add a user to a supplementary group:**

```bash
sudo gpasswd -a bhaskar devops
```

**Remove a user from a supplementary group:**

```bash
sudo gpasswd -d bhaskar devops
```

### 6.5 `groupdel` – Delete a Group

```bash
sudo groupdel devops
```

A group cannot be deleted while it is the primary group of an existing user.

Change that user's primary group first, or remove the user if appropriate.

---

## 7. Switching Users and Administrative Access

### 7.1 `su` – Switch User

**Switch to another user:**

```bash
su bhaskar
```

**Start a login shell as another user:**

```bash
su - bhaskar
```

**Switch to root:**

```bash
su -
```

The `-` requests a login environment, including the target user's environment settings and working directory.

**Return to the previous shell:**

```bash
exit
```

The password prompt depends on the account and command.

For example, switching to another normal user usually requires that target user's password. Switching to root with `su -` normally requires the root password.

### 7.2 `sudo` – Execute a Command with Another User's Privileges

**Run one command with administrator privileges:**

```bash
sudo useradd bhaskar
```

**Run a command as a specific user:**

```bash
sudo -u bhaskar whoami
```

**Open a root login shell:**

```bash
sudo -i
```

`sudo` normally asks for the password of the user running sudo, not the root password.

The exact behavior depends on the sudoers configuration.

### 7.3 `/etc/sudoers` and `visudo`

The sudoers file controls who can use sudo and which commands they can run.

Use:

```bash
sudo visudo
```

`visudo` checks the file for syntax errors before saving. This is safer than editing `/etc/sudoers` directly.

On many RHEL-based systems, administrator access is also configured through files in:

```text
/etc/sudoers.d/
```

**Note:** Do not give a user administrator privileges unless they need them.

---

## 8. Deleting Users and Groups

### 8.1 `userdel` – Delete a User Account

**Delete only the account:**

```bash
sudo userdel bhaskar
```

**Delete the account and its home directory and mail spool:**

```bash
sudo userdel -r bhaskar
```

**Difference:**

- `userdel bhaskar`: Deletes the user account but normally leaves the home directory and the user's files.
- `userdel -r bhaskar`: Also removes the user's home directory and mail spool.

Before deleting a user, check whether the user is logged in or has running processes:

```bash
who
ps -u bhaskar
```

If `userdel` reports that the user is currently used by a process, stop the relevant work safely and log the user out before retrying.

Do not terminate processes without checking what they are doing.

### 8.2 Why Can `userdel` Leave a Group?

Some Linux systems create a private group with the same name as each user.

Whether that group is automatically removed depends on the distribution's user/group configuration and whether the group is still in use.

Check the group:

```bash
getent group bhaskar
```

If the group remains and is no longer needed, it can be deleted separately:

```bash
sudo groupdel bhaskar
```

This will fail if the group is still the primary group of another existing user.

### 8.3 Why Can `groupdel` Fail?

Example error:

```text
groupdel: cannot remove the primary group of user 'ec2-user'
```

**Meaning:**

`ec2-user` is still using that group as its primary group.

Linux does not allow the primary group of an existing user to be deleted.

Check the user's primary group:

```bash
id ec2-user
```

If appropriate, change the user's primary group to another existing group before deleting the old group.

Do not change system-account groups unless you understand the effect.

---

## 9. Complete Practical Example

The following example creates a user, creates a group, adds the user to that group, and verifies the configuration.

Run commands with administrator privileges.

### Step 1: Create a User

```bash
sudo useradd -m -s /bin/bash bhaskar
```

### Step 2: Set the User's Password

```bash
sudo passwd bhaskar
```

### Step 3: Create a Group

```bash
sudo groupadd devops
```

### Step 4: Add the User to the Supplementary Group

```bash
sudo usermod -aG devops bhaskar
```

### Step 5: Verify the User and Group Information

```bash
id bhaskar
groups bhaskar
getent passwd bhaskar
getent group devops
```

### Step 6: Configure Password Aging

```bash
sudo chage -m 0 -M 90 -W 14 bhaskar
```

### Step 7: View Password-Aging Information

```bash
sudo chage -l bhaskar
```

### Step 8: Switch to the User

```bash
su - bhaskar
```

Check the current username:

```bash
whoami
```

Return to the previous shell:

```bash
exit
```

### Step 9: Delete the Test User

Only when you no longer need it.

First make sure the user is logged out and has no relevant running processes.

```bash
sudo userdel -r bhaskar
```

If the `devops` group is no longer needed and no user has it as their primary group:

```bash
sudo groupdel devops
```

---

## 10. Quick Command Revision

| Command | Purpose |
|---|---|
| `useradd` | Create a user |
| `usermod` | Modify a user |
| `userdel` | Delete a user |
| `passwd` | Set or change a password |
| `passwd -S` | Display password status |
| `chage -l` | Display password-aging details |
| `groupadd` | Create a group |
| `groupdel` | Delete a group |
| `groupmod` | Modify a group |
| `gpasswd` | Manage group membership |
| `id` | Display UID, GID, and group memberships |
| `groups` | Display a user's groups |
| `lid` | List user/group information, if installed |
| `getent` | Look up user or group records |
| `su` | Switch user |
| `sudo` | Run a command with permitted privileges |
| `visudo` | Safely edit sudoers configuration |
| `who` | Show logged-in users |
| `whoami` | Show the current username |
| `ps -u username` | Show processes owned by a user |
| `cat /etc/passwd` | View local user account records |
| `cat /etc/group` | View local group records |
| `sudo cat /etc/shadow` | View protected password-aging records |

---

## Important Points to Remember

1. Root has UID `0`.
2. `/etc/passwd` stores user account details.
3. `/etc/shadow` stores password hashes and aging information.
4. `/etc/group` stores group records.
5. `id username` shows the user's UID, primary GID, and group memberships.
6. `usermod -g` changes the primary group.
7. `usermod -aG` appends a supplementary group.
8. `usermod -G` replaces the supplementary-group list.
9. `passwd` changes passwords; `chage` manages password-aging rules.
10. `userdel -r` removes the account and its home directory/mail spool.
11. A group cannot be deleted while it is the primary group of an existing user.
12. Use `visudo` to edit sudoers safely.
13. Always verify account and group changes with `id`, `getent`, and `groups`.
