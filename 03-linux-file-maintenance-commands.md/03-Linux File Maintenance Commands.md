## 1. File Maintenance Commands

- `chown` – Changes the owner of a file or directory.
- `chgrp` – Changes the group owner of a file or directory.
- `cp` – Copies files or directories.
- `mv` – Moves or renames files or directories.
- `file` – Identifies the type of a file.
- `wc` – Counts lines, words, bytes, or characters in a file.
- `ln` – Creates hard links.
- `ln -s` – Creates soft (symbolic) links.

---

## 2. `chown` Command

**Meaning:** Changes the owner of a file or directory.

**Syntax:**

```bash
sudo chown username filename
```

**Example:**

```bash
[ec2-user@server ~]$ ls -l devops.txt
-rw-r--r-- 1 sakshi developers 147 Nov 1 10:00 devops.txt

[ec2-user@server ~]$ sudo chown ec2-user devops.txt

[ec2-user@server ~]$ ls -l devops.txt
-rw-r--r-- 1 ec2-user developers 147 Nov 1 10:00 devops.txt
```

**Explanation:** The owner of `devops.txt` changes from `sakshi` to `ec2-user`. The group remains unchanged.

To change both owner and group:

```bash
sudo chown ec2-user:developers devops.txt
```

Changing ownership usually requires administrator privileges.

---

## 3. `chgrp` Command

**Meaning:** Changes the group owner of a file or directory.

**Syntax:**

```bash
sudo chgrp groupname filename
```

**Example:**

```bash
[ec2-user@server ~]$ ls -l devops.txt
-rw-r--r-- 1 ec2-user ec2-user 147 Nov 1 10:00 devops.txt

[ec2-user@server ~]$ sudo chgrp developers devops.txt

[ec2-user@server ~]$ ls -l devops.txt
-rw-r--r-- 1 ec2-user developers 147 Nov 1 10:00 devops.txt
```

**Explanation:** The group owner changes from `ec2-user` to `developers`.

**Difference:**
- `chown` changes the owner and can also change the group.
- `chgrp` changes only the group owner.

---

## 4. `cp` Command

**Meaning:** Copies files or directories from one location to another.

**Syntax:**

```bash
cp source destination
```

### Example 1: Copy a file

```bash
[ec2-user@server ~]$ ls
devops.txt

[ec2-user@server ~]$ cp devops.txt backup.txt

[ec2-user@server ~]$ ls
backup.txt  devops.txt
```

**Explanation:** A new file named `backup.txt` is created. The original `devops.txt` remains unchanged.

### Example 2: Copy a file into another directory

```bash
[ec2-user@server ~]$ cp devops.txt /home/ec2-user/

[ec2-user@server ~]$ ls /home/ec2-user/
backup.txt  devops.txt
```

**Explanation:** The file is copied into `/home/ec2-user/`.

### Example 3: Copy a directory

```bash
[ec2-user@server ~]$ ls
project

[ec2-user@server ~]$ cp -r project backup-project

[ec2-user@server ~]$ ls
backup-project  project
```

**Explanation:** The `-r` option copies the directory and all its contents recursively.

---

## 5. `mv` Command

**Meaning:** Moves or renames files and directories.

**Syntax:**

```bash
mv source destination
```

### Example 1: Rename a file

```bash
[ec2-user@server ~]$ ls
devops.txt

[ec2-user@server ~]$ mv devops.txt python.txt

[ec2-user@server ~]$ ls
python.txt
```

**Explanation:** The filename changes from `devops.txt` to `python.txt`.

### Example 2: Move a file

```bash
[ec2-user@server ~]$ ls
python.txt

[ec2-user@server ~]$ mv python.txt /home/ec2-user/project/

[ec2-user@server ~]$ ls /home/ec2-user/project/
python.txt
```

**Explanation:** The file moves into the `project` directory.

**Difference between `cp` and `mv`:**
- `cp` creates a copy and keeps the original.
- `mv` moves or renames the original.

---

## 6. `file` Command

**Meaning:** Identifies the type of a file.

**Syntax:**

```bash
file filename
```

**Example:**

```bash
[ec2-user@server ~]$ echo "Welcome to Linux" > devops.txt

[ec2-user@server ~]$ file devops.txt
devops.txt: ASCII text
```

**Explanation:** Linux identifies `devops.txt` as a text file.

---

## 7. `wc` Command

**Meaning:** Counts lines, words, bytes, or characters in a file.

**Example:**

```bash
[ec2-user@server ~]$ echo "Linux is easy to learn" > devops.txt

[ec2-user@server ~]$ wc devops.txt
1 5 23 devops.txt
```

**Explanation:**
- `1` – Number of lines.
- `5` – Number of words.
- `23` – Number of bytes.

### Different options of wc

**Count lines:**

```bash
[ec2-user@server ~]$ wc -l devops.txt
1 devops.txt
```

**Count words:**

```bash
[ec2-user@server ~]$ wc -w devops.txt
5 devops.txt
```

**Count bytes:**

```bash
[ec2-user@server ~]$ wc -c devops.txt
23 devops.txt
```

**Count characters:**

```bash
[ec2-user@server ~]$ wc -m devops.txt
23 devops.txt
```

**Options:**
- `-l` – Counts lines.
- `-w` – Counts words.
- `-c` – Counts bytes.
- `-m` – Counts characters.

---

## 8. `sudo su` and `sudo su -`

### A. sudo su

**Meaning:** Switches to the root user without performing a full login-shell setup.

```bash
[ec2-user@server ~]$ sudo su

[root@server ec2-user]# whoami
root

[root@server ec2-user]# pwd
/home/ec2-user
```

**Explanation:** The user changes to `root`, but the current working directory normally remains the same.

### B. sudo su -

**Meaning:** Switches to root with a login-shell environment.

```bash
[ec2-user@server ~]$ sudo su -

[root@server ~]# whoami
root

[root@server ~]# pwd
/root
```

**Explanation:** The user changes to `root`, and the working directory normally changes to `/root`.

---

## 9. Hard Link and Soft Link

A link provides another name or path through which a file can be accessed.

### A. Hard Link

**Meaning:** A hard link is another directory entry referring to the same underlying file data.

**Syntax:**

```bash
ln original_file hard_link
```

**Example:**

```bash
[ec2-user@server ~]$ echo "Linux practice" > devops.txt

[ec2-user@server ~]$ ln devops.txt devops-hard.txt

[ec2-user@server ~]$ ls
devops-hard.txt  devops.txt

[ec2-user@server ~]$ ls -li
12345 -rw-r--r-- 2 ec2-user ec2-user 15 Nov 1 10:00 devops.txt
12345 -rw-r--r-- 2 ec2-user ec2-user 15 Nov 1 10:00 devops-hard.txt
```

**Explanation:**
- Both filenames have the same inode number `12345`.
- Both refer to the same underlying file data.
- `2` indicates two hard links.

**Check content:**

```bash
[ec2-user@server ~]$ cat devops-hard.txt
Linux practice
```

**Features:**
- Changes to file content are visible through both names.
- Normally cannot be created for directories by ordinary users.
- Cannot cross filesystem boundaries.
- Can continue working after the original filename is deleted.

### B. Soft Link (Symbolic Link)

**Meaning:** A soft link stores the path to another file or directory.

**Syntax:**

```bash
ln -s target_file soft_link
```

**Example:**

```bash
[ec2-user@server ~]$ ln -s devops.txt devops-soft.txt

[ec2-user@server ~]$ ls -l
-rw-r--r-- 2 ec2-user ec2-user 15 Nov 1 10:00 devops.txt
lrwxrwxrwx 1 ec2-user ec2-user 10 Nov 1 10:05 devops-soft.txt -> devops.txt
```

**Explanation:**
- `l` indicates a symbolic link.
- `devops-soft.txt` is the soft link.
- `-> devops.txt` indicates the target file.

**Check content:**

```bash
[ec2-user@server ~]$ cat devops-soft.txt
Linux practice
```

**Features:**
- Can point to files and directories.
- Can cross filesystem boundaries.
- May stop working if the target is moved or deleted.
- Does not create another copy of the file.

### Difference Between Hard Link and Soft Link

| Hard Link | Soft Link |
|---|---|
| Created using `ln`. | Created using `ln -s`. |
| Refers to the same underlying file data. | Stores the target path. |
| Normally cannot link directories. | Can link directories. |
| Cannot cross filesystem boundaries. | Can cross filesystem boundaries. |
| Works after original filename is deleted. | May break if the target path no longer exists. |

---

## 10. Understanding `ls -l` Output

**Command:**

```bash
[ec2-user@server ~]$ ls -l

-rw-r--r-- 1 ec2-user developers 147 Nov 1 10:00 devops.txt
drwxr-xr-x 2 ec2-user developers 4096 Nov 1 10:00 project
lrwxrwxrwx 1 ec2-user developers 10 Nov 1 10:00 soft-link -> devops.txt
```

**Explanation:**

1. `-rw-r--r--` – File type and permissions.
2. `1` – Number of hard links.
3. `ec2-user` – File owner.
4. `developers` – Group owner.
5. `147` – File size in bytes.
6. `Nov 1 10:00` – Last modification time.
7. `devops.txt` – Filename.

**File type symbols:**
- `-` – Regular file.
- `d` – Directory.
- `l` – Symbolic link.

---

## 11. `vi` Text Editor Commands

**Meaning:** The `vi` command opens a text editor in the terminal.

**Open a file:**

```bash
[ec2-user@server ~]$ vi python.txt
```

Inside the editor:

1. Press `i` to enter insert mode.
2. Type the following text:

```text
Welcome to Linux
Learning file maintenance commands
```

3. Press `Esc` to return to normal mode.
4. Type `:wq`.
5. Press `Enter` to save and exit.

**Check saved content:**

```bash
[ec2-user@server ~]$ cat python.txt
Welcome to Linux
Learning file maintenance commands
```

**Important commands:**

| Action | Command |
|---|---|
| Enter insert mode | Press `i` |
| Return to normal mode | Press `Esc` |
| Save and exit | `:wq` |
| Save without exiting | `:w` |
| Exit without saving | `:q!` |

---

## 12. Quick Revision

| Command | Purpose |
|---|---|
| `chown` | Change file owner |
| `chgrp` | Change group owner |
| `cp` | Copy files |
| `mv` | Move or rename files |
| `file` | Identify file type |
| `wc` | Count lines, words, bytes, or characters |
| `ln` | Create a hard link |
| `ln -s` | Create a soft link |
| `sudo su` | Switch to root user |
| `sudo su -` | Switch to root with login-shell environment |
| `ls -l` | Display file details |
| `vi` | Open terminal text editor |

---

