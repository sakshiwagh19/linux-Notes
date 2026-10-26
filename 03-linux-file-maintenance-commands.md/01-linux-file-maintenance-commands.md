## 1. touch Command

The `touch` command is used to create an empty file (zero bytes) if the file does not exist.

### Syntax
```bash
touch filename.txt
```

### Example
```bash
touch test.txt
ls
```

**Note:** If the file already exists, `touch` updates its timestamp.

---

## 2. find Command

The `find` command is used to search for files and directories based on specified conditions.

### 1. Find all files in the current directory

```bash
find . -type f
```

- `.` represents the current directory.
- `-type f` searches for regular files.
- It also searches inside subdirectories.

### 2. Find all empty files

```bash
find . -type f -empty
```

- `-type f` means files.
- `-empty` finds empty files.

### 3. Find all empty directories in the current directory

```bash
find . -type d -empty
```

- `-type d` means directories.
- `-empty` finds empty directories.

### 4. Find all empty files in the entire file system

```bash
sudo find / -type f -empty
```

- `/` represents the root directory.
- `sudo` runs the command with elevated privileges.
- It searches for empty files throughout the file system.

### 5. Search for empty files under the logged-in user's home directory

```bash
find ~ -type f -empty
```

- `~` represents the logged-in user's home directory.

### 6. Find hidden files in the current directory

```bash
find . -type f -name ".*"
```

- Hidden filenames in Linux usually start with a dot (`.`).
- This command also searches inside subdirectories.

### 7. Find a particular file under the logged-in user's home directory

```bash
find ~ -name "mithun.sh"
```

- Searches for `mithun.sh` in the user's home directory and its subdirectories.

### 8. Find a particular file under the current directory

```bash
find . -name "mithun.sh"
```

- Searches for `mithun.sh` in the current directory and its subdirectories.

### 9. Search for a file while ignoring case

```bash
find . -iname "mithun.sh"
```

- `-iname` ignores uppercase and lowercase differences.
- It can match `mithun.sh`, `Mithun.sh`, and `MITHUN.SH`.

### 10. Find files with 777 permissions

```bash
find . -type f -perm 0777
```

- `-perm 0777` searches for files with exactly these permissions.
- Permission `777` gives read, write, and execute permissions to everyone.

**Note:** Avoid giving `777` permissions unnecessarily because it can create security risks.

---

## 3. umask Command

`umask` stands for User File-Creation Mask.

It controls which permissions are removed when new files and directories are created.

### Display the current umask

```bash
umask
```

Example values:

- Root user: `0022`
- Normal user: `0002`

These are examples; actual values may vary.

### Default permissions

- Base permissions for files: `666`
- Base permissions for directories: `777`

### Example 1: umask 0022

For files:

```text
Base permissions = 666
umask            = 022
Final permissions = 644
```

For directories:

```text
Base permissions = 777
umask            = 022
Final permissions = 755
```

### Example 2: umask 0002

For files:

```text
Base permissions = 666
umask            = 002
Final permissions = 664
```

For directories:

```text
Base permissions = 777
umask            = 002
Final permissions = 775
```

### Change umask temporarily

```bash
umask 0222
umask -S
```

- `umask 0222` changes the mask for the current shell.
- `umask -S` displays the mask in symbolic form.
- The change is temporary and normally lasts for the current shell session.

### Set umask for one user permanently

Edit the user's shell configuration file, such as `~/.bashrc` or `~/.bash_profile`, and add:

```bash
umask 022
```

The correct file depends on the shell and Linux distribution.

### Set umask system-wide

System-wide umask settings may be configured in files such as `/etc/profile` or other distribution-specific configuration files.

Administrative privileges may be required.

---

## 4. chmod Command

The `chmod` command is used to change file or directory permissions.

### Permission values

| Permission | Symbol | Numeric Value |
|---|---|---:|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

### User categories

- `u` = User (owner)
- `g` = Group
- `o` = Others
- `a` = All users

### Create a file and check permissions

```bash
touch test.txt
ls -l test.txt
```

### Change permissions using numeric values

#### Remove all permissions

```bash
chmod 000 test.txt
```

#### Give read and write permissions to the owner and read permission to others

```bash
chmod 644 test.txt
```

#### Give all permissions to everyone

```bash
chmod 777 test.txt
```

**Note:** Using `777` is generally not recommended for regular files.

### Give read permission to everyone

```bash
chmod 444 test.txt
```

### Change permissions using symbolic mode

#### Add execute permission to the owner

```bash
chmod u+x test.txt
```

#### Remove write permission from the owner

```bash
chmod u-w test.txt
```

#### Give read permission to everyone

```bash
chmod a+r test.txt
```

### Change directory permissions

```bash
chmod 700 mydir
```

Only the owner gets read, write, and execute permissions.

### Change permissions recursively

```bash
chmod -R 777 mydir
```

This applies the permissions recursively to the directory and its contents.

**Warning:** Use recursive permission changes carefully.

---

## 5. chown Command

The `chown` command changes the ownership of a file or directory.

Administrative privileges are generally required.

### Syntax

```bash
sudo chown username filename.txt
```

### Change owner and group

```bash
sudo chown username:groupname filename.txt
```

### Example

```bash
sudo chown ec2-user:ec2-user test.txt
```

This changes the owner and group of `test.txt` to `ec2-user`.

---

## 6. chgrp Command

The `chgrp` command changes the group ownership of a file or directory.

### Syntax

```bash
chgrp groupname filename.txt
```

### Example

```bash
sudo chgrp developers test.txt
```

This changes the group ownership of `test.txt` to `developers`.

**Note:** Changing a file's group may require administrative privileges depending on the user and target group.

---

## 7. Update a File Using vi

The `vi` command is used to create or edit a text file.

### Step 1: Open or create a file

```bash
vi file_name.txt
```

### Step 2: Enter Insert Mode

Press:

```text
i
```

Now you can type the content.

### Step 3: Save the file

Press `Esc` to return to command mode.

Type:

```vim
:w
```

Press `Enter` to save.

### Step 4: Save and quit

```vim
:wq
```

Press `Enter`.

### Useful vi commands

| Command | Purpose |
|---|---|
| `i` | Enter Insert Mode |
| `Esc` | Return to Command Mode |
| `:w` | Save the file |
| `:q` | Quit |
| `:q!` | Quit without saving changes |
| `:wq` | Save and quit |

---

## 8. Display File Contents Using cat

The `cat` command displays the contents of a file.

### Syntax

```bash
cat filename.txt
```

### Example

```bash
cat file_name.txt
```

It displays the text stored inside the file.

---

## 9. Create a File in a Particular Directory

To create a file inside a directory, specify the directory path.

### Example

```bash
touch devops/test.txt
```

This creates `test.txt` inside the `devops` directory, provided that the directory exists.

### Check the file

```bash
ls devops
```

### If the directory does not exist

Create the directory first:

```bash
mkdir -p devops
touch devops/test.txt
```

The `mkdir -p` command creates the directory if it does not already exist.

---

## Important Linux Symbols

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `~` | Logged-in user's home directory |
| `/` | Root directory |
| `$` | Normal user's shell prompt |
| `#` | Root user's shell prompt |

**Important:** `$` and `#` are usually prompt symbols shown in examples. Do not type them as part of the command.

---

## Quick Revision

| Command | Purpose |
|---|---|
| `touch` | Create an empty file |
| `find` | Search for files and directories |
| `umask` | Control default creation permissions |
| `chmod` | Change file or directory permissions |
| `chown` | Change file ownership |
| `chgrp` | Change group ownership |
| `vi` | Create or edit a file |
| `cat` | Display file contents |
| `ls -l` | Display file permissions and ownership |
| `mkdir -p` | Create directories, including parent directories |
