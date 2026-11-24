# Notes: Cron, Shell Scripting, SSH and SCP

# 1. Automating / Scheduling Tasks Using Cron

## 1.1 What is Cron?

Cron is a background service (daemon) in Linux that executes commands or scripts automatically at scheduled times.

It is used to automate repetitive tasks without executing them manually every time.

### Why Do We Need Cron?

Suppose we want to perform the following tasks regularly:

- Take a backup every day.
- Delete temporary files every week.
- Generate reports every morning.
- Monitor server information at regular intervals.
- Execute a shell script every minute.

Instead of manually executing these commands, we can schedule them using Cron.

**Real-time Example:**

Suppose you want to execute a backup script every day at 12:00 AM (midnight).

Without Cron:
- You need to execute the backup script manually every day.

With Cron:
- Schedule the backup script once.
- Cron automatically executes it every day at the specified time.

### Important Points

- Cron is a background service.
- It runs automatically.
- It executes commands according to a schedule.
- It is useful for task automation.
- It works even when the user is not actively using the terminal, provided the system is running and the cron service is available.

## 1.2 What is Crontab?

Crontab stands for Cron Table.

It is a command-line utility used to create, manage, and schedule cron jobs.

A crontab file contains:
- Five time fields.
- The command or script to execute.

The Cron daemon reads the crontab configuration and executes the scheduled commands at the specified time.

### Difference Between Cron and Crontab

| Cron | Crontab |
|---|---|
| Cron is a background service (daemon). | Crontab is a command-line utility. |
| It executes scheduled jobs. | It manages scheduled jobs. |
| It works in the background. | Users use it to configure schedules. |
| It reads scheduled configurations. | It stores the user's scheduled commands. |

**Remember:**

Cron = Executes scheduled tasks.

Crontab = Creates and manages scheduled tasks.

## 1.3 Cron Time Format

A crontab entry contains five time fields followed by the command.

### Cron Syntax

```text
┌──────── minute (0–59)
│ ┌────── hour (0–23)
│ │ ┌──── day of month (1–31)
│ │ │ ┌── month (1–12)
│ │ │ │ ┌ day of week (0–7)
│ │ │ │ │
* * * * * command
```

### Five Fields of Cron

| Field | Meaning | Allowed Values |
|---|---|---|
| Minute | Minute of the hour | 0–59 |
| Hour | Hour of the day | 0–23 |
| Day of Month | Date of the month | 1–31 |
| Month | Month of the year | 1–12 |
| Day of Week | Day of the week | 0–7 |

**Day of Week:**
- 0 = Sunday
- 1 = Monday
- 2 = Tuesday
- 3 = Wednesday
- 4 = Thursday
- 5 = Friday
- 6 = Saturday
- 7 = Sunday

### Common Cron Symbols

| Symbol | Meaning | Example |
|---|---|---|
| `*` | Every possible value | `* * * * *` |
| `,` | Multiple selected values | `0 9,18 * * *` |
| `-` | Range of values | `0 9-17 * * *` |
| `/` | Step or interval | `*/5 * * * *` |

### Explanation of Symbols

**1. Asterisk (*) – Every Value**

```bash
* * * * * command
```

Meaning: Executes the command every minute.

**2. Comma (,) – Multiple Values**

```bash
0 9,18 * * * command
```

Meaning: Executes the command at:
- 9:00 AM
- 6:00 PM

**3. Hyphen (-) – Range of Values**

```bash
0 9-17 * * * command
```

Meaning: Executes the command every hour from 9:00 AM to 5:00 PM.

**4. Slash (/) – Step or Interval**

```bash
*/5 * * * * command
```

Meaning: Executes the command every 5 minutes.

### Common Cron Examples

| Cron Expression | Explanation |
|---|---|
| `* * * * *` | Every minute |
| `*/5 * * * *` | Every 5 minutes |
| `*/10 * * * *` | Every 10 minutes |
| `*/15 * * * *` | Every 15 minutes |
| `0 * * * *` | Every hour |
| `0 12 * * *` | Every day at 12:00 PM |
| `0 0 * * *` | Every day at 12:00 AM |
| `30 8 * * *` | Every day at 8:30 AM |
| `0 9 * * 1` | Every Monday at 9:00 AM |
| `30 8 * * 1-5` | Monday to Friday at 8:30 AM |
| `0 0 1 * *` | First day of every month at midnight |
| `0 18 * * 0` | Every Sunday at 6:00 PM |

**Important:** Cron uses the server's configured time zone. Check the server time zone if a job runs at an unexpected time.

Check the current date and time:

```bash
date
```

Check the configured time zone:

```bash
timedatectl
```

## 1.4 Why Do We Need Crontab?

Crontab is useful when we want to execute commands repeatedly at a particular time.

For example:

- Run a script every minute.
- Generate a daily report.
- Take automatic backups.
- Remove old temporary files.
- Monitor disk usage.

Instead of manually executing the commands, we schedule them once using crontab.

## 1.5 Important Crontab Commands

| Command | Purpose |
|---|---|
| `crontab -e` | Edit the current user's crontab |
| `crontab -l` | List the current user's cron jobs |
| `crontab -r` | Remove all cron jobs of the current user |
| `crontab -u username -l` | List cron jobs of a particular user |
| `crontab -u username -e` | Edit cron jobs of a particular user |

### Explanation

**1. Edit Crontab**

```bash
crontab -e
```

Opens the current user's crontab editor.

We can add, modify, or remove scheduled jobs.

**2. List Crontab**

```bash
crontab -l
```

Displays all scheduled cron jobs of the current user.

**3. Remove Crontab**

```bash
crontab -r
```

Removes all scheduled cron jobs of the current user.

**4. List Another User's Crontab**

```bash
sudo crontab -u ec2-user -l
```

Displays cron jobs belonging to `ec2-user`.

**5. Edit Another User's Crontab**

```bash
sudo crontab -u ec2-user -e
```

Opens the crontab editor for `ec2-user`.

**Important:** The `-r` option removes the entire crontab. Use it carefully.

## 1.6 Cron Service Commands

Cron jobs are executed by the Cron service.

On Amazon Linux and many RHEL-based systems, the service name is `crond`.

### Check Cron Service Status

```bash
sudo systemctl status crond
```

### Start Cron Service

```bash
sudo systemctl start crond
```

### Stop Cron Service

```bash
sudo systemctl stop crond
```

### Restart Cron Service

```bash
sudo systemctl restart crond
```

### Enable Cron Service at Boot

```bash
sudo systemctl enable crond
```

### Disable Cron Service at Boot

```bash
sudo systemctl disable crond
```

On Debian/Ubuntu systems, the service is commonly named `cron` instead of `crond`.

Example:

```bash
sudo systemctl status cron
sudo systemctl start cron
sudo systemctl restart cron
```

**Difference Between Start, Restart and Enable**

| Command | Meaning |
|---|---|
| `systemctl start` | Starts the service immediately |
| `systemctl stop` | Stops the service |
| `systemctl restart` | Stops and starts the service again |
| `systemctl enable` | Configures the service to start automatically at boot |
| `systemctl disable` | Prevents automatic startup at boot |
| `systemctl status` | Displays service status |

---

# 2. Creating and Running a Shell Script

## 2.1 What is a Shell Script?

A shell script is a file containing multiple Linux commands.

The shell executes these commands one by one in the order in which they are written.

Shell scripts commonly use the `.sh` extension.

### Why Do We Use Shell Scripts?

Suppose we need to execute five Linux commands repeatedly.

Without a shell script:
- Execute each command manually.

With a shell script:
- Write all commands inside one file.
- Execute the file.
- All commands run automatically in sequence.

### Advantages

- Automates repetitive tasks.
- Saves time.
- Reduces manual work.
- Executes multiple commands together.
- Can be scheduled using Cron.

## 2.2 Create a Shell Script

Create a file named `hello.sh`.

```bash
vi hello.sh
```

Alternatively:

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

### Explanation of the Script

| Command | Meaning |
|---|---|
| `#!/bin/bash` | Specifies Bash as the interpreter |
| `echo "Hello Guys"` | Prints Hello Guys |
| `echo "Welcome"` | Prints Welcome |
| `echo "Today date is:"` | Prints a message |
| `date` | Displays the current date and time |
| `echo "Cronjob demo"` | Prints Cronjob demo |

### What is Shebang (#!)?

```bash
#!/bin/bash
```

The first line of the script is called the shebang.

It tells the operating system which interpreter should execute the script.

Here:
- `#!` identifies the interpreter directive.
- `/bin/bash` specifies the Bash interpreter.

## 2.3 Give Execute Permission

When we create a new file, it may not have execute permission.

First, check the permissions:

```bash
ls -l hello.sh
```

Example output:

```text
-rw-r--r-- 1 ec2-user ec2-user 100 Nov 24 10:00 hello.sh
```

Here:
- `rw-` = Owner permissions.
- `r--` = Group permissions.
- `r--` = Others' permissions.

The file does not have execute permission.

### Give Execute Permission to Owner

```bash
chmod u+x hello.sh
```

Meaning:

| Part | Meaning |
|---|---|
| `chmod` | Change file permissions |
| `u` | User/owner |
| `+` | Add permission |
| `x` | Execute permission |
| `hello.sh` | File name |

Check permissions again:

```bash
ls -l hello.sh
```

Example output:

```text
-rwxr--r-- 1 ec2-user ec2-user 100 Nov 24 10:00 hello.sh
```

Now the owner has execute permission.

## 2.4 Run the Shell Script

There are two common methods.

### Method 1: Execute Using ./ 

```bash
./hello.sh
```

The `./` indicates that the file is present in the current directory.

This method requires execute permission.

### Method 2: Execute Using Bash

```bash
bash hello.sh
```

Here, Bash reads and executes the script.

This method does not require the script itself to have execute permission, but the user must have permission to read the file.

### Expected Output

```text
Hello Guys
Welcome
Today date is:
Tue Nov 24 10:00:00 UTC 2026
Cronjob demo
```

The date and time will depend on your server.

---

# 3. Schedule a Shell Script Using Crontab

Now we will connect Shell Scripting with Cron.

Suppose we have created the following script:

```text
/home/ec2-user/hello.sh
```

We want to execute this script automatically every minute.

## 3.1 Schedule a Script Every Minute

**Step 1: Open the crontab editor.**

```bash
crontab -e
```

**Step 2: Add the following entry.**

```bash
* * * * * /home/ec2-user/hello.sh
```

**Step 3: Save and exit.**

Now Cron executes the script every minute.

**Step 4: Check the configured job.**

```bash
crontab -l
```

Expected output:

```text
* * * * * /home/ec2-user/hello.sh
```

## 3.2 Save Script Output in a Log File

By default, a cron job's output is not displayed in your active terminal.

We can redirect the output to a log file.

Add this entry to crontab:

```bash
* * * * * /home/ec2-user/hello.sh > /home/ec2-user/hello.log 2>&1
```

### Explanation

| Part | Meaning |
|---|---|
| `* * * * *` | Execute every minute |
| `/home/ec2-user/hello.sh` | Script to execute |
| `>` | Redirect standard output |
| `hello.log` | Destination log file |
| `2>&1` | Redirect standard error to standard output's destination |

### What is Output Redirection?

Output redirection means sending command output to a file instead of displaying it on the terminal.

Linux has three standard streams:

| Stream | Number | Meaning |
|---|---|---|
| Standard Input | 0 | Input provided to a command |
| Standard Output | 1 | Normal command output |
| Standard Error | 2 | Error messages |

### Meaning of 2>&1

```bash
2>&1
```

- `2` represents standard error.
- `>` represents redirection.
- `&1` means the current destination of standard output.

Therefore, both normal output and error messages are redirected to the same destination.

## 3.3 Difference Between > and >>

### Using >

```bash
* * * * * /home/ec2-user/hello.sh > /home/ec2-user/hello.log 2>&1
```

`>` overwrites the existing file.

Whenever the script runs, the previous contents are replaced.

### Using >>

```bash
* * * * * /home/ec2-user/hello.sh >> /home/ec2-user/hello.log 2>&1
```

`>>` appends new output to the end of the file.

Previous log entries remain available.

### Difference

| Operator | Meaning |
|---|---|
| `>` | Overwrites existing content |
| `>>` | Adds content at the end |
| `2>` | Redirects error output |
| `2>&1` | Sends error output to the same destination as standard output |

## 3.4 Check the Log File

To display the complete log file:

```bash
cat /home/ec2-user/hello.log
```

To display the last 10 lines:

```bash
tail /home/ec2-user/hello.log
```

To continuously monitor new entries:

```bash
tail -f /home/ec2-user/hello.log
```

### Difference Between cat, tail and tail -f

| Command | Purpose |
|---|---|
| `cat file` | Displays the complete file |
| `tail file` | Displays the last 10 lines by default |
| `tail -f file` | Continuously monitors new content |

**Practical Tip:** If the cron job runs every minute, wait until the next minute and check the log file.

## 3.5 Check and Remove Configured Cron Jobs

List the current user's cron jobs:

```bash
crontab -l
```

Edit cron jobs:

```bash
crontab -e
```

Remove all cron jobs:

```bash
crontab -r
```

**Important:** Deleting a cron job does not delete the shell script or its log file. It only removes the scheduled configuration.

---

# 4. Restricting Cron Access for Users

Linux provides two files to control which users can use crontab.

1. `/etc/cron.allow`
2. `/etc/cron.deny`

These files are used to manage access to Cron.

## 4.1 What is /etc/cron.allow?

`/etc/cron.allow` contains usernames that are allowed to use the crontab command.

When this file exists, only users listed in it are generally allowed to use crontab.

### Step 1: Create the File

```bash
sudo touch /etc/cron.allow
```

### Step 2: Edit the File

```bash
sudo vi /etc/cron.allow
```

### Step 3: Add Usernames

```text
ec2-user
devops
```

Save and exit.

### Meaning

Here:
- `ec2-user` is allowed to use crontab.
- `devops` is allowed to use crontab.
- Users not listed are generally denied access.

**Important:** Adding a user to `cron.allow` gives permission to use crontab. It does not automatically create a cron job for that user.

## 4.2 What is /etc/cron.deny?

`/etc/cron.deny` contains usernames that are not allowed to use crontab.

If `cron.allow` does not exist, the system generally checks `cron.deny`.

### Step 1: Create the File

```bash
sudo touch /etc/cron.deny
```

### Step 2: Edit the File

```bash
sudo vi /etc/cron.deny
```

### Step 3: Add the Restricted User

```text
guest
```

Save and exit.

### Meaning

Here:
- `guest` is not allowed to use crontab.
- Other users may use crontab unless restricted by another rule.

## 4.3 Difference Between cron.allow and cron.deny

| `/etc/cron.allow` | `/etc/cron.deny` |
|---|---|
| Specifies allowed users. | Specifies restricted users. |
| Only listed users are generally allowed when the file exists. | Listed users are denied when the allow file does not exist. |
| Takes precedence when present. | Used when the allow file is absent. |

### Important Rules

**Case 1: Both Files Exist**

When both files exist, `cron.allow` generally takes precedence.

Only users listed in `cron.allow` are allowed.

**Case 2: Only cron.allow Exists**

Only listed users are allowed.

**Case 3: Only cron.deny Exists**

Users listed in `cron.deny` are denied. Other users are generally allowed.

**Case 4: Neither File Exists**

Access is generally permitted to users, depending on the Cron implementation and system configuration.

## 4.4 Check Whether a User Can Use Crontab

Log in as the relevant user.

Example:

```bash
su - ec2-user
```

Check whether the user can list cron jobs:

```bash
crontab -l
```

Try to edit the crontab:

```bash
crontab -e
```

If the user is not permitted, the system displays an access-denied message.

### Important Difference

Permission to use crontab and permission to execute a particular script are different things.

For example:
- A user may be allowed to create cron jobs.
- But the user may not have permission to execute a script belonging to another user.

The script's file permissions and required directory permissions must also be correct.

---

# 5. SSH: Remote Access to Linux Servers

## 5.1 What is SSH?

SSH stands for Secure Shell.

It is a network protocol used to securely connect to and manage remote computers or servers.

SSH allows us to:
- Connect to remote Linux servers.
- Execute commands remotely.
- Manage AWS EC2 instances.
- Transfer files securely using SCP.
- Perform server administration.

### Real-Time Example

Suppose you have two Linux servers:

- Server 1: Your local computer.
- Server 2: AWS EC2 instance.

You want to access Server 2 from Server 1.

You can use SSH to establish a secure connection.

## 5.2 SSH Command

### General Syntax

```bash
ssh username@server-ip
```

### Example

```bash
ssh ec2-user@<server-public-ip>
```

### Explanation

| Part | Meaning |
|---|---|
| `ssh` | Starts a secure remote connection |
| `ec2-user` | Username on the remote server |
| `<server-public-ip>` | Public IP address of the remote server |

### What Happens When We Execute SSH?

1. The SSH client sends a connection request.
2. The remote server responds.
3. The SSH client verifies the server's host identity.
4. The client and server establish an encrypted connection.
5. The server authenticates the user.
6. After successful authentication, the user gets remote terminal access.

## 5.3 SSH Using a PEM Key

AWS EC2 commonly uses key-based authentication.

When we launch an EC2 instance, we can select or create a key pair.

The private key is downloaded as a `.pem` file.

We use this private key to authenticate with the EC2 instance.

### General Syntax

```bash
ssh -i "key-file.pem" username@server-ip
```

### Example

```bash
ssh -i "devops.pem" ec2-user@<server-public-ip>
```

### Explanation

| Part | Meaning |
|---|---|
| `ssh` | Secure Shell command |
| `-i` | Specifies the identity/private-key file |
| `devops.pem` | Private key file |
| `ec2-user` | Remote server username |
| `<server-public-ip>` | Public IP address of the remote server |

### Important Points

- The PEM file must be available on the machine running SSH.
- The specified path must be correct.
- The private key must have suitable permissions.
- The EC2 security group must allow SSH traffic on port 22 from the connecting IP.
- The username must match the selected EC2 operating system.

For Linux/macOS:

```bash
chmod 400 devops.pem
```

This restricts access to the private key file.

## 5.4 SSH Using Password Authentication

SSH can also authenticate users using a password.

In password-based authentication:
- The client connects to the server.
- The server requests the user's password.
- The user enters the password.
- The server verifies the credentials.
- If authentication succeeds, the user gets access.

Password authentication must be enabled on the SSH server.

### Example Configuration Process

Perform these steps only on a server you administer and when password authentication is genuinely required.

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

Find the password authentication setting.

```text
PasswordAuthentication yes
```

Make sure the active SSH configuration does not contain conflicting settings.

**Step 4: Validate the SSH configuration.**

```bash
sshd -t
```

If there is no output, the configuration syntax is valid.

**Step 5: Restart the SSH service.**

Amazon Linux/RHEL-based systems:

```bash
systemctl restart sshd
```

Ubuntu systems commonly use:

```bash
systemctl restart ssh
```

### Important Security Points

- Key-based authentication is generally preferred.
- Do not enable password authentication unnecessarily.
- Use strong passwords when password authentication is required.
- Restrict SSH network access.
- Keep private keys secure.

## 5.5 Difference Between Password-Based and Key-Based Authentication

| Password Authentication | Key-Based Authentication |
|---|---|
| Uses a username and password. | Uses a username and private key. |
| Requires password authentication to be enabled on the server. | Requires the matching public key to be authorized on the server. |
| User enters a password when prompted. | Client uses the private key to authenticate. |
| Password security is important. | Private key security is important. |
| Generally less preferred for production access. | Generally preferred for EC2 and production access. |

---

# 6. SCP: Securely Copy Files Between Servers

## 6.1 What is SCP?

SCP stands for Secure Copy Protocol.

It is used to securely transfer files and directories between computers or servers over SSH.

SCP uses SSH for secure communication and authentication.

### Why Do We Need SCP?

Suppose:
- You have a file on your local computer.
- You want to upload that file to an AWS EC2 instance.

Instead of manually transferring the file, you can use SCP.

### Common Uses

- Copy a file from a local machine to a remote server.
- Copy a directory from a local machine to a remote server.
- Download files from a remote server.
- Transfer files between two remote servers.

## 6.2 Copy a File From Local Machine to Remote Server

### General Syntax

```bash
scp filename username@server-ip:/destination/path/
```

### Example

```bash
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
```

### Explanation

| Part | Meaning |
|---|---|
| `scp` | Secure Copy command |
| `devops.tar` | Source file |
| `ec2-user` | Destination server username |
| `<server2-public-ip>` | Destination server IP address |
| `/tmp/` | Destination directory |

### What Happens?

1. SCP reads the local file.
2. It establishes an SSH connection with the destination server.
3. The destination server authenticates the user.
4. The file is transferred securely.
5. The file is stored in the specified destination directory.

## 6.3 Copy a Directory From Local Machine to Remote Server

To copy a complete directory, use the `-r` option.

### General Syntax

```bash
scp -r directory_name username@server-ip:/destination/path/
```

### Example

```bash
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
```

### Meaning of -r

`-r` means recursive copying.

It copies:
- The directory.
- Files inside the directory.
- Subdirectories.
- Files inside the subdirectories.

**Important:** Use `-r` when copying directories.

## 6.4 Copy a File From Remote Server to Local Machine

We can also download a file from a remote server to our local computer.

### General Syntax

```bash
scp username@server-ip:/source/path/filename /local/destination/
```

### Example

```bash
scp ec2-user@<server-public-ip>:/tmp/devops.tar .
```

### Meaning

- `scp` = Secure Copy command.
- `ec2-user` = Remote server username.
- `<server-public-ip>` = Remote server IP.
- `/tmp/devops.tar` = File on the remote server.
- `.` = Current local directory.

The file is downloaded into the current local directory.

## 6.5 Copy a Directory From Remote Server to Local Machine

Use `-r` to download a complete directory.

### General Syntax

```bash
scp -r username@server-ip:/source/path/directory_name /local/destination/
```

### Example

```bash
scp -r ec2-user@<server-public-ip>:/tmp/Devops .
```

This copies the complete `Devops` directory from the remote server into the current local directory.

## 6.6 Copy a File From Server 1 to Server 2

Suppose we have two Linux servers:

- Server 1: Contains the source file.
- Server 2: Receives the file.

We can transfer the file from Server 1 to Server 2 using SCP.

### General Syntax

```bash
scp /path/to/file username@server2-ip:/destination/path/
```

### Example

```bash
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
```

This command is executed from Server 1 or another machine that can access both the source file and the destination server.

### Using Password Authentication

If the destination server allows password authentication, SCP prompts for the destination user's password.

Example:

```bash
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
```

### Using PEM Key Authentication

If the destination server uses key-based authentication, specify the appropriate private key.

```bash
scp -i "devops.pem" devops.tar ec2-user@<server2-public-ip>:/tmp/
```

### Important Explanation

The `-i` option specifies the private key used to authenticate with the destination server.

The PEM file must be accessible on the machine where the SCP command is executed.

## 6.7 Copy a Directory From Server 1 to Server 2

Use `-r` to copy a complete directory.

### Command

```bash
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
```

This transfers the complete `Devops` directory and its contents to Server 2.

### Important Points

- `scp` transfers files securely.
- `-r` is used for directories.
- `-i` specifies a private key.
- The source path must exist.
- The destination directory must be accessible to the destination user.
- SSH connectivity must be available.
- The destination server's security group and network rules must permit SSH traffic.

## 6.8 Difference Between SSH and SCP

| SSH | SCP |
|---|---|
| SSH stands for Secure Shell. | SCP stands for Secure Copy Protocol. |
| Used for remote login and administration. | Used for transferring files and directories. |
| Executes commands on remote servers. | Copies files between computers. |
| Example: `ssh ec2-user@IP` | Example: `scp file ec2-user@IP:/tmp/` |
| Provides a remote terminal session. | Transfers data through SSH connectivity. |

---

# 7. Important Troubleshooting Commands

## 7.1 Cron Troubleshooting

**Check whether Cron is running:**

```bash
sudo systemctl status crond
```

**Check configured cron jobs:**

```bash
crontab -l
```

**Check current server time:**

```bash
date
```

**Check time zone:**

```bash
timedatectl
```

**Check cron logs on Amazon Linux/RHEL systems:**

```bash
sudo journalctl -u crond
```

**Follow cron logs continuously:**

```bash
sudo journalctl -u crond -f
```

### Common Reasons Why Cron Jobs May Not Execute

1. Cron service is stopped.
2. Incorrect cron expression.
3. Incorrect script path.
4. Missing file permissions.
5. Script contains errors.
6. Required environment variables are missing.
7. The script uses relative paths instead of absolute paths.
8. Output redirection or log directory permissions are incorrect.

**Important:** Cron runs with a limited environment. Use absolute paths for scripts, commands, and log files.

## 7.2 SSH Troubleshooting

**Check SSH service status:**

```bash
sudo systemctl status sshd
```

**Check SSH configuration syntax:**

```bash
sudo sshd -t
```

**Check whether password authentication is configured:**

```bash
sudo sshd -T | grep -i passwordauthentication
```

**Check SSH port connectivity from a suitable client:**

```bash
ssh -v ec2-user@<server-public-ip>
```

The `-v` option displays detailed connection information.

### Common SSH Errors

| Error | Possible Reason |
|---|---|
| Connection timed out | Network or security group issue |
| Permission denied | Incorrect username, password, or key |
| Connection refused | SSH service may not be running or listening |
| Identity file not accessible | Incorrect PEM file path |
| Host key verification failed | Host identity verification issue |

## 7.3 SCP Troubleshooting

**Common SCP Errors:**

| Error | Possible Reason |
|---|---|
| No such file or directory | Incorrect source or destination path |
| Permission denied | Insufficient file or directory permissions |
| Connection timed out | Network or security group issue |
| Permission denied (publickey) | Incorrect private key or username |
| Identity file not accessible | Incorrect PEM file path |

### Important Checks Before Executing SCP

1. Verify that the source file exists.
2. Verify the destination server IP.
3. Verify the destination username.
4. Verify the destination directory.
5. Verify SSH connectivity.
6. Use the correct authentication method.
7. Check the required file permissions.

---

# 8. Quick Command Revision

## 8.1 Cron and Crontab

| Command | Use |
|---|---|
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `crontab -r` | Remove all current user's cron jobs |
| `crontab -u username -l` | List a particular user's cron jobs |
| `crontab -u username -e` | Edit a particular user's cron jobs |
| `systemctl status crond` | Check Cron service status |
| `systemctl start crond` | Start Cron service |
| `systemctl stop crond` | Stop Cron service |
| `systemctl restart crond` | Restart Cron service |
| `systemctl enable crond` | Enable Cron at boot |

## 8.2 Shell Scripting

| Command | Use |
|---|---|
| `vi hello.sh` | Create or edit a script |
| `cat hello.sh` | Display script content |
| `ls -l hello.sh` | Check file permissions |
| `chmod u+x hello.sh` | Give owner execute permission |
| `./hello.sh` | Execute the script |
| `bash hello.sh` | Execute the script using Bash |

## 8.3 Output Redirection

| Symbol | Use |
|---|---|
| `>` | Overwrite file |
| `>>` | Append to file |
| `2>` | Redirect standard error |
| `2>&1` | Redirect standard error to standard output's destination |

## 8.4 Log File Commands

| Command | Use |
|---|---|
| `cat hello.log` | Display complete log |
| `tail hello.log` | Display last 10 lines |
| `tail -f hello.log` | Continuously monitor log entries |

## 8.5 SSH and SCP

| Command | Use |
|---|---|
| `ssh username@server-ip` | Connect to a remote server |
| `ssh -i key.pem username@server-ip` | Connect using a private key |
| `scp file username@server-ip:/path/` | Copy a local file to a remote server |
| `scp -r directory username@server-ip:/path/` | Copy a local directory to a remote server |
| `scp username@server-ip:/path/file .` | Download a remote file to the current local directory |
| `scp -r username@server-ip:/path/directory .` | Download a remote directory to the current local directory |
| `scp -i key.pem file username@server-ip:/path/` | Copy a file using a private key |

---

# 9. Important Points to Remember

1. Cron is a background service that executes scheduled jobs.
2. Crontab is used to configure and manage scheduled jobs.
3. Cron uses five time fields.
4. A shell script contains multiple Linux commands.
5. The `.sh` extension is commonly used for shell scripts.
6. The shebang `#!/bin/bash` specifies the Bash interpreter.
7. Use `chmod u+x filename.sh` to give the file owner execute permission.
8. Use `./filename.sh` to execute a script from the current directory.
9. Use `crontab -e` to schedule a script.
10. Use `crontab -l` to list configured jobs.
11. Use `crontab -r` to remove all cron jobs of the current user.
12. Use `>` to overwrite output and `>>` to append output.
13. Use `2>&1` to redirect standard error to standard output's destination.
14. `/etc/cron.allow` controls which users are allowed to use crontab.
15. `/etc/cron.deny` controls which users are denied access to crontab.
16. SSH is used for secure remote login.
17. SCP is used for secure file and directory transfer.
18. Use `-i` to specify a private key.
19. Use `-r` when copying directories.
20. Protect private SSH keys and prefer key-based authentication.

---

# 10. Practical Commands Checklist

Practice the following commands on your Linux server.

## Cron and Crontab

- [ ] Check Cron service status.
- [ ] Start and stop Cron service.
- [ ] Restart Cron service.
- [ ] Enable Cron service at boot.
- [ ] Open crontab using `crontab -e`.
- [ ] List jobs using `crontab -l`.
- [ ] Understand all five Cron time fields.
- [ ] Practise different Cron expressions.

## Shell Scripting

- [ ] Create `hello.sh`.
- [ ] Add multiple Linux commands.
- [ ] Understand the shebang.
- [ ] Check file permissions using `ls -l`.
- [ ] Give execute permission using `chmod u+x`.
- [ ] Execute the script using `./hello.sh`.
- [ ] Execute the script using `bash hello.sh`.

## Cron Automation

- [ ] Schedule a script every minute.
- [ ] Redirect script output to a log file.
- [ ] Understand `>` and `>>`.
- [ ] Understand `2>&1`.
- [ ] Display log content using `cat`.
- [ ] Monitor logs using `tail -f`.

## Cron User Access

- [ ] Create `/etc/cron.allow`.
- [ ] Add allowed usernames.
- [ ] Create `/etc/cron.deny`.
- [ ] Add restricted usernames.
- [ ] Test crontab access using another user.

## SSH

- [ ] Connect to an EC2 instance using SSH.
- [ ] Understand SSH password authentication.
- [ ] Understand SSH key-based authentication.
- [ ] Connect using a PEM file.
- [ ] Understand SSH configuration.
- [ ] Check SSH service status.

## SCP

- [ ] Copy a local file to a remote server.
- [ ] Copy a local directory using `-r`.
- [ ] Download a remote file.
- [ ] Download a remote directory.
- [ ] Copy a file from Server 1 to Server 2.
- [ ] Understand password-based SCP.
- [ ] Understand key-based SCP using `-i`.

