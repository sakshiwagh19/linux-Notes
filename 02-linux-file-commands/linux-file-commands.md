## 1. Basic Linux Commands

| Command | Meaning | Example / Use |
|---|---|---|
| `pwd` | Print working directory | Shows the current directory |
| `ls` | List | Shows files and directories |
| `ls -l` | Long listing | Shows file details |
| `ls -a` | All | Shows hidden files |
| `ls -lh` | Human-readable sizes | Shows file sizes in KB, MB, etc. |
| `ls -lt` | Sort by modification time | Shows recently modified items first |
| `ls -ltr` | Reverse time order | Shows oldest modified items first |
| `ls -li` | Shows inode numbers | Displays inode information |
| `clear` | Clear screen | Clears terminal output |
| `whoami` | Who am I | Shows the current username |
| `date` | Date and time | Shows current date and time |

## 2. Directory Commands

| Command | Meaning | Example / Use |
|---|---|---|
| `mkdir Test` | Make directory | Creates a folder named Test |
| `mkdir -v Test` | Verbose | Creates a folder and displays a message |
| `mkdir -p DevOps/Linux/Git` | Parent directories | Creates missing parent folders |
| `mkdir -vp DevOps/Linux/Git` | Verbose + parent directories | Creates missing folders and displays messages |
| `cd DevOps` | Change directory | Enters the DevOps folder |
| `cd ..` | Go one level up | Moves to the parent directory |
| `cd ~` | Home directory | Goes to the user's home directory |
| `cd -` | Previous directory | Returns to the previous working directory |
| `rmdir Test` | Remove directory | Removes an empty folder |
| `tree` | Tree structure | Shows folders and subfolders |

## 3. File Commands

| Command | Meaning | Example / Use |
|---|---|---|
| `touch notes.txt` | Create file | Creates an empty file if it does not exist |
| `cat notes.txt` | Display content | Shows the file content |
| `cp notes.txt backup.txt` | Copy | Copies a file |
| `cp -v notes.txt backup.txt` | Verbose copy | Displays a message while copying |
| `cp -p notes.txt backup.txt` | Preserve attributes | Preserves permissions and timestamps |
| `mv notes.txt new.txt` | Move or rename | Renames or moves a file |
| `rm notes.txt` | Remove | Deletes a file |
| `rm -i notes.txt` | Interactive remove | Asks before deleting |
| `rm -v notes.txt` | Verbose remove | Displays a message while deleting |
| `find . -name "notes.txt"` | Find | Searches for a file by name |

**Warning:** `rm` can permanently delete files. Check the filename before running it.

## 4. Understanding `-v` and `-p`

### `-v` — Verbose

It displays messages about what the command is doing.

**Example:**

```bash
mkdir -v Test

Output:
mkdir: created directory 'Test'


-p — Parent Directories
With mkdir, it creates missing parent directories automatically.

Example:
mkdir -p DevOps/Linux/ShellScript


This creates the directory structure even if the parent folders do not exist.
Using -v and -p Together
mkdir -vp DevOps/Linux/Git


- -v displays messages.
- -p creates missing parent directories.
Note: Options can have different meanings in different commands.
5. The tree Command
The tree command displays a directory structure.

Example:
mkdir -p DevOps/Linux/Git
tree DevOps


Output:
DevOps
└── Linux
    └── Git


Install tree on Ubuntu/Debian
sudo apt update
sudo apt install tree


Install tree on Fedora/RHEL
sudo dnf install tree


6. Package Management Commands
A package contains files needed to install software.
Ubuntu / Debian — APT
Command	Meaning
sudo apt update	Refreshes the package list
sudo apt upgrade	Upgrades installed packages
sudo apt install tree	Installs the tree package
sudo apt remove tree	Removes the tree package
apt search tree	Searches for packages
apt show tree	Shows package information

Fedora / RHEL — DNF
Command	Meaning
sudo dnf install tree	Installs a package
sudo dnf remove tree	Removes a package
sudo dnf update	Updates packages
dnf search tree	Searches for packages
dnf info tree	Shows package information

Older CentOS/RHEL — YUM
sudo yum install tree
sudo yum update
sudo yum remove tree


7. File Permissions
Use ls -l to view file and directory permissions.

Example:
ls -l


Output:
drwxr-xr-x 2 user user 4096 Oct 25 DevOps
-rw-r--r-- 1 user user  100 Oct 25 notes.txt


Permission Symbols
- d — Directory
- - — Regular file
- r — Read permission
- w — Write permission
- x — Execute permission
Change Permissions
chmod 700 notes.txt


This gives the owner read, write, and execute permissions. The group and others receive no permissions.
Create a Directory with Custom Permissions
mkdir -m 700 Python


This creates the Python directory with permission mode 700.
8. User and System Commands
Command	Meaning
sudo	Runs a command with elevated privileges
su	Switches to another user
exit	Exits the current shell or user session
uname -r	Shows the Linux kernel version
df -h	Shows disk space in readable units
free -h	Shows memory usage
ps	Shows running processes
top	Monitors running processes

9. Practice Example
Run these commands one by one in a Linux terminal.
pwd
mkdir -vp DevOps/Linux/ShellScript
cd DevOps
ls -l
cd Linux
touch notes.txt
ls -la
cat notes.txt
cp -v notes.txt backup.txt
mv backup.txt newnotes.txt
ls
cd ..
tree DevOps


What You Will Learn
1. Check the current directory.
2. Create nested directories.
3. Navigate between directories.
4. Create and list files.
5. Copy and rename files.
6. Display the directory structure.
10. Quick Revision of Common Options
Option	General Meaning
-v	Verbose; displays more information
-p	Depends on the command; for mkdir, creates parent directories
-l	Long listing with ls
-a	Includes hidden files with ls
-h	Human-readable sizes with supported commands
-t	Sorts by modification time with ls
-r	Reverse order with supported commands
-i	Interactive confirmation with rm
-m	Sets directory permissions with mkdir

Remember: Linux commands are case-sensitive. For example, DevOps and devops are different directory names.
