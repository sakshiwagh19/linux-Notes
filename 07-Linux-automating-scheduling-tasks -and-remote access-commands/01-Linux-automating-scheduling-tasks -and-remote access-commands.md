# Linux Notes: Automating Tasks Using Cron, Shell Scripting, SSH and SCP

## 1. Automating / Scheduling Tasks Using Cron

### 1.1 What is Cron?

Cron is a background service (daemon) in Linux.

- It executes commands or scripts automatically at scheduled times.
- It is useful when we want a task to run repeatedly without executing it manually.

**Example:**

Suppose you want to run a backup command every day at 12:00 AM (midnight). Instead of running it manually every day, you can schedule it using cron.

### 1.2 What is Crontab?

Crontab is the command-line program used to manage scheduled cron jobs.

We use it to:

- Add scheduled jobs.
- Edit existing jobs.
- List scheduled jobs.
- Remove scheduled jobs.

The cron daemon reads the configured schedule and runs commands at the specified time.

### 1.3 Cron Time Format

A crontab entry contains five time fields followed by the command.

```text
┌──────── minute (0–59)
│ ┌────── hour (0–23)
│ │ ┌──── day of month (1–31)
│ │ │ ┌── month (1–12)
│ │ │ │ ┌ day of week (0–7)
│ │ │ │ │
* * * * * command
```

**Cron Fields:**

| Field | Meaning | Allowed Values |
|---|---|---|
| Minute | Minute of the hour | 0–59 |
| Hour | Hour of the day | 0–23 |
| Day of Month | Date of the month | 1–31 |
| Month | Month | 1–12 |
| Day of Week | Day of the week | 0–7 |

**Common Cron Symbols:**

| Symbol | Meaning | Example |
|---|---|---|
| `*` | Every possible value | `* * * * *` |
| `,` | Multiple selected values | `0 9,18 * * *` |
| `-` | Range of values | `0 9-17 * * *` |
| `/` | Step or interval | `*/5 * * * *` |

**Cron Examples:**

| Cron Expression | Meaning |
|---|---|
| `* * * * *` | Runs every minute |
| `*/5 * * * *` | Runs every 5 minutes |
| `0 12 * * *` | Runs every day at 12:00 PM (noon) |
| `0 0 * * *` | Runs every day at 12:00 AM (midnight) |
| `30 8 * * 1-5` | Runs at 8:30 AM, Monday to Friday |

**Important:** Cron uses the server's configured time zone. Check the server time zone if a job runs at an unexpected time.

### 1.4 Why Do We Need Crontab?

If you want to execute a regular task every day or every minute, you do not need to run the command manually each time.

You can schedule it once in crontab.

**Example:** Run a shell script every minute to generate a log entry automatically.

### 1.5 Important Crontab Commands

| Command | Purpose |
|---|---|
| `crontab -e` | Edit the current user's crontab |
| `crontab -l` | List the current user's cron jobs |
| `crontab -r` | Remove all cron jobs of the current user |
| `crontab -u username -l` | List cron jobs belonging to a particular user |
| `crontab -u username -e` | Edit cron jobs belonging to a particular user |

**Important:** `crontab -r` removes the current user's entire crontab. Use it carefully.

### 1.6 Cron Service Commands

Cron jobs are executed by the cron service/daemon.

On many Linux distributions that use systemd:

**Check service status:**

```bash
sudo systemctl status crond
```

On some Debian/Ubuntu systems:

```bash
sudo systemctl status cron
```

**Start the service:**

```bash
sudo systemctl start crond
```

**Restart the service:**

```bash
sudo systemctl restart crond
```

**Enable the service at boot:**

```bash
sudo systemctl enable crond
```

**Note:** Use `cron` instead of `crond` on systems where the service has that name.

---

## 2. Creating and Running a Shell Script

### 2.1 What is a Shell Script?

A shell script is a file containing a sequence of Linux commands. The shell executes those commands in order.

- Shell script files commonly use the `.sh` extension.
- A script can contain multiple commands.
- A script can be executed manually or scheduled through cron.

### 2.2 Create a Shell Script

Create a file named `hello.sh`:

```bash
vi hello.sh
```

You can also use:

```bash
nano hello.sh
```

Add the following content:

```bash
#!/bin/bash

echo "Hello Guys"
echo "Welcome"
echo "Today date is:"
date
echo "Cronjob demo"
```

Save and exit the editor.

**Meaning of the Script:**

| Line | Meaning |
|---|---|
| `#!/bin/bash` | Tells the system to execute the script using Bash |
| `echo "Hello Guys"` | Prints a message |
| `echo "Welcome"` | Prints another message |
| `date` | Displays the current date and time |
| `echo "Cronjob demo"` | Prints a final message |

### 2.3 Give Execute Permission

A newly created script may not have execute permission.

**Check permissions:**

```bash
ls -l hello.sh
```

**Give execute permission to the owner:**

```bash
chmod u+x hello.sh
```

**Check permissions again:**

```bash
ls -l hello.sh
```

The `x` permission indicates that the file can be executed by the user who has that permission.

### 2.4 Run the Shell Script

Run using its path:

```bash
./hello.sh
```

Or run using Bash:

```bash
bash hello.sh
```

**Meaning:**

- `./` means the file is in the current directory.
- `bash` executes the script using the Bash shell.

---

## 3. Schedule a Shell Script Using Crontab

Suppose the script is located at:

```text
/home/ec2-user/hello.sh
```

**Step 1: Open the crontab editor.**

```bash
crontab -e
```

**Step 2: Add the following entry.**

```bash
* * * * * /home/ec2-user/hello.sh
```

This runs the script every minute.

### 3.1 Save Script Output in a Log File

To save the output in a log file:

```bash
* * * * * /home/ec2-user/hello.sh > /home/ec2-user/hello.log 2>&1
```

**Meaning:**

| Part | Meaning |
|---|---|
| `* * * * *` | Run every minute |
| `/home/ec2-user/hello.sh` | Script to execute |
| `>` | Redirect standard output and overwrite the file |
| `hello.log` | Log file where output is stored |
| `2>&1` | Redirect standard error to the same destination as standard output |

**Note:** Since `>` overwrites the log file, previous output is replaced whenever the job runs.

### 3.2 Append Output Instead of Overwriting

If you want to keep previous log entries and add new output at the end, use `>>`.

```bash
* * * * * /home/ec2-user/hello.sh >> /home/ec2-user/hello.log 2>&1
```

**Difference between `>` and `>>`:**

| Symbol | Meaning |
|---|---|
| `>` | Overwrites the file |
| `>>` | Appends output to the end of the file |
| `2>&1` | Sends standard error to the same destination as standard output |

### 3.3 Check the Log File

**Display the log:**

```bash
cat /home/ec2-user/hello.log
```

**Watch new log entries:**

```bash
tail -f /home/ec2-user/hello.log
```

If the job is configured to run every minute, wait for the next minute and check the log.

### 3.4 Check Configured Cron Jobs

**List current user's cron jobs:**

```bash
crontab -l
```

**Edit cron jobs:**

```bash
crontab -e
```

**Delete all jobs in the current user's crontab:**

```bash
crontab -r
```

---

## 4. Restricting Cron Access for Users

Linux provides two files to control which users can use crontab:

1. `/etc/cron.allow`
2. `/etc/cron.deny`

### 4.1 /etc/cron.allow

This file contains the usernames that are allowed to use cron.

**Create the file:**

```bash
sudo touch /etc/cron.allow
```

**Edit the file:**

```bash
sudo vi /etc/cron.allow
```

Add the usernames that should be allowed, one username per line.

**Example:**

```text
ec2-user
devops
```

When `cron.allow` exists, users generally need to be listed in it to use crontab.

### 4.2 /etc/cron.deny

This file contains the usernames that are not allowed to use cron.

**Create the file:**

```bash
sudo touch /etc/cron.deny
```

**Edit the file:**

```bash
sudo vi /etc/cron.deny
```

Add one restricted username per line.

**Example:**

```text
guest
```

### 4.3 Difference Between cron.allow and cron.deny

| File | Purpose |
|---|---|
| `/etc/cron.allow` | Specifies users allowed to use cron |
| `/etc/cron.deny` | Specifies users denied from using cron |

**Important:**

- If `cron.allow` exists, it generally takes precedence.
- Only users listed in `cron.allow` are allowed.
- If `cron.allow` does not exist, `cron.deny` is used to block listed users.
- Exact behavior can vary slightly by cron implementation and Linux distribution.

### 4.4 Check Whether a User Can Use Crontab

Log in as the relevant user and try:

```bash
crontab -l
```

Or try to edit:

```bash
crontab -e
```

If the user is not permitted, the system displays an access-denied message.

---

## 5. SSH: Remote Access to Linux Servers

### 5.1 What is SSH?

SSH (Secure Shell) is a protocol used to securely connect to and manage a remote computer/server over a network.

**Example:**

You can connect from your local computer to an AWS EC2 Linux instance and execute commands on that remote server.

### 5.2 SSH Command

**General format:**

```bash
ssh username@server-ip
```

**Example:**

```bash
ssh ec2-user@<server-public-ip>
```

**Meaning:**

| Part | Meaning |
|---|---|
| `ssh` | Starts a secure remote connection |
| `ec2-user` | Username on the remote server |
| `<server-public-ip>` | Public IP address of the remote server |

### 5.3 Connect Using a PEM Key

For an EC2 instance that uses key-based authentication:

**Command:**

```bash
ssh -i "devops.pem" ec2-user@<server-public-ip>
```

**Meaning:**

| Part | Meaning |
|---|---|
| `ssh` | Secure Shell command |
| `-i` | Specifies the identity/private-key file |
| `devops.pem` | Private key file used for authentication |
| `ec2-user` | Remote server username |
| `<server-public-ip>` | Public IP address of the remote server |

The private key must be available at the specified local path and have suitable permissions.

For Linux/macOS:

```bash
chmod 400 devops.pem
```

### 5.4 SSH Using Password Authentication

A password-based SSH connection may be configured on a Linux server, but it is generally less secure than key-based authentication.

The following is an example of the configuration process. Perform it only when you administer the server and have a secure reason to enable it.

**Step 1: Switch to an administrator account.**

```bash
sudo -i
```

**Step 2: Set a password for the required user.**

```bash
passwd ec2-user
```

Enter and confirm the password when prompted.

**Step 3: Edit the SSH server configuration.**

```bash
vi /etc/ssh/sshd_config
```

Find the password-authentication setting and configure it as required:

```text
PasswordAuthentication yes
```

Make sure there is no conflicting active setting elsewhere in the configuration.

**Step 4: Validate the SSH configuration.**

```bash
sshd -t
```

If there is no output, the configuration syntax is valid.

**Step 5: Restart the SSH service.**

On Amazon Linux/RHEL-based systems:

```bash
systemctl restart sshd
```

On Ubuntu, the service is commonly also named `ssh`:

```bash
systemctl restart ssh
```

**Security Note:**

- Prefer SSH key-based authentication for EC2 and production servers.
- Do not enable password login unnecessarily.
- If password login is enabled, use strong passwords and restrict network access.

---

## 6. SCP: Securely Copy Files Between Servers

### 6.1 What is SCP?

SCP (Secure Copy Protocol) is used to securely transfer files and directories between computers/servers over SSH.

**Examples of use:**

- Copy a file from one server to another.
- Copy a directory from one server to another.
- Copy a file from a remote server to the local computer.
- Copy a file from the local computer to a remote server.

### 6.2 Copy a File From Local Machine to Remote Server

**General format:**

```bash
scp filename username@server-ip:/destination/path/
```

**Example:**

```bash
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
```

**Meaning:**

| Part | Meaning |
|---|---|
| `scp` | Secure copy command |
| `devops.tar` | File to copy |
| `ec2-user` | Username on the destination server |
| `<server2-public-ip>` | Destination server's IP address |
| `/tmp/` | Destination directory |

### 6.3 Copy a Directory From Local Machine to Remote Server

Use `-r` for a directory.

**General format:**

```bash
scp -r directory_name username@server-ip:/destination/path/
```

**Example:**

```bash
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
```

`-r` means recursive copying. It copies the directory and its contents.

### 6.4 Copy a File From Remote Server to Local Machine

**General format:**

```bash
scp username@server-ip:/source/path/filename /local/destination/
```

**Example:**

```bash
scp ec2-user@<server-public-ip>:/tmp/devops.tar .
```

The `.` means the current local directory.

### 6.5 Copy a Directory From Remote Server to Local Machine

**General format:**

```bash
scp -r username@server-ip:/source/path/directory_name /local/destination/
```

**Example:**

```bash
scp -r ec2-user@<server-public-ip>:/tmp/Devops .
```

### 6.6 Copy a File From Server 1 to Server 2

**General format:**

```bash
scp /path/to/file username@server2-ip:/destination/path/
```

**Example:**

```bash
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
```

This command is run from Server 1 (or another machine that can access both the source file and destination server).

If the destination server uses a password, SCP may prompt for that password.

If it uses key-based authentication, specify the appropriate key using `-i`.

**Example:**

```bash
scp -i "devops.pem" devops.tar ec2-user@<server2-public-ip>:/tmp/
```

### 6.7 Copy a Directory From Server 1 to Server 2

**Command:**

```bash
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
```

The `-r` option is required to copy a directory and its contents.

**Important:**

If the source and destination servers both require different private keys, configure SSH access appropriately.

Do not copy private keys to another server just to make SCP work.

---

## 7. Quick Command Revision

### 7.1 Cron and Crontab

| Command | Use |
|---|---|
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `crontab -r` | Remove all current user's cron jobs |
| `sudo systemctl status crond` | Check cron service status on systems using crond |
| `sudo systemctl restart crond` | Restart cron service on systems using crond |
| `chmod u+x hello.sh` | Give the file owner execute permission |
| `./hello.sh` | Execute the script from the current directory |
| `cat hello.log` | Display log file contents |
| `tail -f hello.log` | Watch new log entries |

### 7.2 Output Redirection

| Symbol | Meaning |
|---|---|
| `>` | Redirect standard output; overwrite destination file |
| `>>` | Redirect standard output; append to destination file |
| `2>` | Redirect standard error |
| `2>&1` | Redirect standard error to the current standard-output destination |

### 7.3 SSH and SCP

| Command | Use |
|---|---|
| `ssh username@server-ip` | Connect to a remote server |
| `ssh -i key.pem username@server-ip` | Connect using a private key |
| `scp file username@server-ip:/path/` | Copy a local file to a remote server |
| `scp -r directory username@server-ip:/path/` | Copy a local directory to a remote server |
| `scp username@server-ip:/path/file .` | Copy a remote file to the current local directory |
| `scp -r username@server-ip:/path/directory .` | Copy a remote directory to the current local directory |

---

## 8. Important Points to Remember

1. Cron is the background service that runs scheduled jobs.
2. Crontab is used to configure and manage scheduled jobs.
3. A cron schedule has five time fields: minute, hour, day of month, month, and day of week.
4. A shell script is a file containing Linux commands.
5. Use `chmod u+x filename.sh` to give the file owner execute permission.
6. Use `crontab -e` to schedule a script.
7. Use `>` to overwrite output and `>>` to append output.
8. Use `2>&1` to send standard error to the same destination as standard output.
9. `cron.allow` and `cron.deny` control which users can use crontab.
10. SSH is used for secure remote login.
11. SCP is used for secure file and directory transfer.
12. Use `scp -r` when copying a directory.
13. Protect private SSH keys and prefer key-based authentication.

---

## 9. Practical Commands Checklist

Use this checklist to practise the commands on your Linux server.

- [ ] Check cron service status.
- [ ] Create `hello.sh`.
- [ ] Add Linux commands inside the script.
- [ ] Check file permissions using `ls -l`.
- [ ] Give execute permission using `chmod u+x`.
- [ ] Execute the script using `./hello.sh`.
- [ ] Open crontab using `crontab -e`.
- [ ] Schedule the script to run every minute.
- [ ] Redirect output to a log file.
- [ ] Check the log using `cat` and `tail -f`.
- [ ] List cron jobs using `crontab -l`.
- [ ] Practise `cron.allow` and `cron.deny`.
- [ ] Connect to an EC2 instance using SSH.
- [ ] Understand password-based and key-based SSH authentication.
- [ ] Copy files using SCP.
- [ ] Copy directories using `scp -r`.
