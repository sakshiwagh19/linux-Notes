1. Automating / Scheduling Tasks Using Cron
1.1 What is Cron?
Cron is a background service (daemon) in Linux.
It executes commands or scripts automatically at scheduled times.
It is useful when we want a task to run repeatedly without executing it manually.
Example
Suppose you want to run a backup command every day at 12:00 AM (midnight). Instead of running it manually every day, you can schedule it using cron.
1.2 What is Crontab?
Crontab is the command-line program used to manage scheduled cron jobs.
We use it to add, edit, list, and remove scheduled jobs.
The cron daemon reads the configured schedule and runs the commands at the specified time.
1.3 Cron Time Format
A crontab entry contains five time fields followed by the command:
┌──────── minute (0–59)
│ ┌────── hour (0–23)
│ │ ┌──── day of month (1–31)
│ │ │ ┌── month (1–12)
│ │ │ │ ┌ day of week (0–7; Sunday is 0 or 7)
│ │ │ │ │
* * * * * command
Field
Meaning
Allowed values
Minute
Minute of the hour
0–59
Hour
Hour of the day
0–23
Day of month
Date of the month
1–31
Month
Month
1–12
Day of week
Day of the week
0–7
Common symbols
Symbol
Meaning
Example
*
Every possible value
* * * * * = every minute
,
Multiple selected values
0 9,18 * * * = 9 AM and 6 PM
-
A range of values
0 9-17 * * * = every hour from 9 AM to 5 PM
/
A step or interval
*/5 * * * * = every 5 minutes
Examples
* * * * * command
Runs every minute.
*/5 * * * * command
Runs every 5 minutes.
0 12 * * * command
Runs every day at 12:00 PM (noon).
0 0 * * * command
Runs every day at 12:00 AM (midnight).
30 8 * * 1-5 command
Runs at 8:30 AM, Monday to Friday.
Cron uses the server's configured time zone. Check the server time zone if a job runs at an unexpected time.
1.4 Why Do We Need Crontab?
If you want to execute a regular task every day or every minute, you do not need to run the command manually each time. You can schedule it once in crontab.
Example: Run a shell script every minute to generate a log entry automatically.
1.5 Important Crontab Commands
Command
Purpose
crontab -e
Edit the current user's crontab and add or modify jobs
crontab -l
List the current user's cron jobs
crontab -r
Remove all cron jobs of the current user
crontab -u username -l
List cron jobs belonging to a particular user (with suitable privileges)
crontab -u username -e
Edit cron jobs belonging to a particular user (with suitable privileges)
Important: crontab -r removes the current user's entire crontab. Use it carefully.
1.6 Cron Service Commands
Cron jobs are executed by the cron service/daemon.
On many Linux distributions that use systemd:
sudo systemctl status crond
On some Debian/Ubuntu systems, the service name is cron:
sudo systemctl status cron
Start the service:
sudo systemctl start crond
Restart the service after configuration changes:
sudo systemctl restart crond
Enable the service to start automatically at boot:
sudo systemctl enable crond
Use cron instead of crond on systems where the service has that name.
2. Creating and Running a Shell Script
2.1 What is a Shell Script?
A shell script is a file containing a sequence of Linux commands. The shell executes those commands in order.
Shell script files commonly use the .sh extension.
A script can contain multiple commands.
A script can be run manually or scheduled through cron.
2.2 Create a Shell Script
Create a file named hello.sh:
vi hello.sh
You can also use nano hello.sh if nano is installed.
Add the following content:
#!/bin/bash

echo "Hello Guys"
echo "Welcome"
echo "Today date is:"
date
echo "Cronjob demo"
Save and exit the editor.
Meaning of the script
Line
Meaning
#!/bin/bash
Tells the system to execute the script using Bash
echo "Hello Guys"
Prints a message
echo "Welcome"
Prints another message
date
Displays the current date and time
echo "Cronjob demo"
Prints a final message
2.3 Give Execute Permission
A newly created script may not have execute permission.
Check its permissions:
ls -l hello.sh
Give the file execute permission for its owner:
chmod u+x hello.sh
Check again:
ls -l hello.sh
The x permission indicates that the file can be executed by the user who has that permission.
2.4 Run the Shell Script
Run it using its path:
./hello.sh
Or run it using Bash:
bash hello.sh
The ./ means that the file is in the current directory.
3. Schedule a Shell Script Using Crontab
Suppose the script is located at:
/home/ec2-user/hello.sh
Open the crontab:
crontab -e
Add this entry:
* * * * * /home/ec2-user/hello.sh
This runs the script every minute.
3.1 Save Script Output in a Log File
To save the output in a log file:
* * * * * /home/ec2-user/hello.sh > /home/ec2-user/hello.log 2>&1
Meaning:
* * * * * — run every minute.
/home/ec2-user/hello.sh — script to execute.
> — redirect standard output to a file and overwrite the file each time.
/home/ec2-user/hello.log — log file where output is stored.
2>&1 — redirect standard error to the same destination as standard output.
Note: Since > overwrites the log file, previous output is replaced whenever the job runs.
3.2 Append Output Instead of Overwriting
If you want to keep the previous log entries and add new output at the end, use >>:
* * * * * /home/ec2-user/hello.sh >> /home/ec2-user/hello.log 2>&1
> = overwrite the file.
>> = append output to the end of the file.
2>&1 = send errors to the same place as standard output.
3.3 Check the Log File
Display the log:
cat /home/ec2-user/hello.log
To watch new log entries as they are added:
tail -f /home/ec2-user/hello.log
If the job is configured to run every minute, wait for the next minute and check the log.
3.4 Check Configured Cron Jobs
List the current user's cron jobs:
crontab -l
Edit the jobs:
crontab -e
Delete all jobs in the current user's crontab:
crontab -r
4. Restricting Cron Access for Users
Linux provides two files to control which users can use crontab:
/etc/cron.allow
/etc/cron.deny
4.1 /etc/cron.allow
This file contains the usernames that are allowed to use cron.
Create the file:
sudo touch /etc/cron.allow
Edit it:
sudo vi /etc/cron.allow
Add the usernames that should be allowed, one username per line.
Example:
ec2-user
devops
When cron.allow exists, users generally need to be listed in it to use crontab.
4.2 /etc/cron.deny
This file contains the usernames that are not allowed to use cron.
Create the file:
sudo touch /etc/cron.deny
Edit it:
sudo vi /etc/cron.deny
Add one restricted username per line.
Example:
guest
4.3 Difference Between cron.allow and cron.deny
File
Purpose
/etc/cron.allow
Specifies users allowed to use cron
/etc/cron.deny
Specifies users denied from using cron
Important: If cron.allow exists, it generally takes precedence: only listed users are allowed. If it does not exist, cron.deny is used to block listed users. Exact behavior can vary slightly by cron implementation and distribution.
4.4 Check Whether a User Can Use Crontab
Log in as the relevant user and try:
crontab -l
Or try to edit:
crontab -e
If the user is not permitted, the system displays an access-denied message.
5. SSH: Remote Access to Linux Servers
5.1 What is SSH?
SSH (Secure Shell) is a protocol used to securely connect to and manage a remote computer/server over a network.
For example, you can connect from your local computer to an AWS EC2 Linux instance and execute commands on that remote server.
5.2 SSH Command
General format:
ssh username@server-ip
Example:
ssh ec2-user@<server-public-ip>
Here:
ssh — starts a secure remote connection.
ec2-user — username on the remote server.
<server-public-ip> — public IP address of the remote server.
5.3 Connect Using a PEM Key
For an EC2 instance that uses key-based authentication:
ssh -i "devops.pem" ec2-user@<server-public-ip>
Meaning:
-i — specifies the identity/private-key file.
devops.pem — private key file used for authentication.
ec2-user — remote server username.
<server-public-ip> — remote server's public IP address.
The private key must be available at the specified local path and have suitable permissions.
For example, on Linux/macOS:
chmod 400 devops.pem
5.4 SSH Using Password Authentication
A password-based SSH connection may be configured on a Linux server, but it is generally less secure than key-based authentication.
The following is an example of the configuration process. Perform it only when you administer the server and have a secure reason to enable it.
Step 1: Switch to an administrator account
sudo -i
Step 2: Set a password for the required user
passwd ec2-user
Enter and confirm the password when prompted.
Step 3: Edit the SSH server configuration
vi /etc/ssh/sshd_config
Find the password-authentication setting and configure it as required:
PasswordAuthentication yes
Make sure there is no conflicting active setting elsewhere in the configuration.
Step 4: Validate the SSH configuration
sshd -t
If there is no output, the configuration syntax is valid.
Step 5: Restart the SSH service
On Amazon Linux/RHEL-based systems:
systemctl restart sshd
On Ubuntu, the service is commonly also named ssh:
systemctl restart ssh
Security note: Prefer SSH key-based authentication for EC2 and production servers. Do not enable password login unnecessarily. If password login is enabled, use strong passwords and restrict network access.
6. SCP: Securely Copy Files Between Servers
6.1 What is SCP?
SCP (Secure Copy Protocol) is used to securely transfer files and directories between computers/servers over SSH.
Examples of use:
Copy a file from one server to another.
Copy a directory from one server to another.
Copy a file from a remote server to the local computer.
Copy a file from the local computer to a remote server.
6.2 Copy a File From Local Machine to Remote Server
General format:
scp filename username@server-ip:/destination/path/
Example:
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
Meaning:
scp — secure copy command.
devops.tar — file to copy.
ec2-user — username on the destination server.
<server2-public-ip> — destination server's IP.
/tmp/ — destination directory.
6.3 Copy a Directory From Local Machine to Remote Server
Use -r for a directory:
scp -r directory_name username@server-ip:/destination/path/
Example:
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
-r means recursive copying. It copies the directory and its contents.
6.4 Copy a File From Remote Server to Local Machine
General format:
scp username@server-ip:/source/path/filename /local/destination/
Example:
scp ec2-user@<server-public-ip>:/tmp/devops.tar .
The . means the current local directory.
6.5 Copy a Directory From Remote Server to Local Machine
scp -r username@server-ip:/source/path/directory_name /local/destination/
Example:
scp -r ec2-user@<server-public-ip>:/tmp/Devops .
6.6 Copy a File From Server 1 to Server 2
General format:
scp /path/to/file username@server2-ip:/destination/path/
Example:
scp devops.tar ec2-user@<server2-public-ip>:/tmp/
This command is run from Server 1 (or another machine that can access both the source file and destination server).
If the destination server uses a password, SCP may prompt for that password. If it uses key-based authentication, specify the appropriate key using -i.
Example:
scp -i "devops.pem" devops.tar ec2-user@<server2-public-ip>:/tmp/
6.7 Copy a Directory From Server 1 to Server 2
scp -r Devops/ ec2-user@<server2-public-ip>:/tmp/
The -r option is required to copy a directory and its contents.
If the source and destination servers both require different private keys, configure SSH access appropriately. Do not copy private keys to another server just to make SCP work.
7. Quick Command Revision
Cron and Crontab
Command
Use
crontab -e
Edit cron jobs
crontab -l
List cron jobs
crontab -r
Remove all current user's cron jobs
sudo systemctl status crond
Check cron service status on systems using crond
sudo systemctl restart crond
Restart cron service on systems using crond
chmod u+x hello.sh
Give the file owner execute permission
./hello.sh
Execute the script from the current directory
cat hello.log
Display log file contents
tail -f hello.log
Watch new log entries
Output Redirection
Symbol
Meaning
>
Redirect standard output; overwrite destination file
>>
Redirect standard output; append to destination file
2>
Redirect standard error
2>&1
Redirect standard error to the current standard-output destination
SSH and SCP
Command
Use
ssh username@server-ip
Connect to a remote server
ssh -i key.pem username@server-ip
Connect using a private key
scp file username@server-ip:/path/
Copy a local file to a remote server
scp -r directory username@server-ip:/path/
Copy a local directory to a remote server
scp username@server-ip:/path/file .
Copy a remote file to the current local directory
scp -r username@server-ip:/path/directory .
Copy a remote directory to the current local directory
8. Important Points to Remember
Cron is the background service that runs scheduled jobs.
Crontab is used to configure and manage scheduled jobs.
A cron schedule has five time fields: minute, hour, day of month, month, and day of week.
A shell script is a file containing Linux commands.
Use chmod u+x filename.sh to give the file owner execute permission.
Use crontab -e to schedule a script.
Use > to overwrite output and >> to append output.
Use 2>&1 to send standard error to the same destination as standard output.
cron.allow and cron.deny control which users can use crontab.
SSH is used for secure remote login.
SCP is used for secure file and directory transfer.
Use scp -r when copying a directory.
Protect private SSH keys and prefer key-based authentication.
