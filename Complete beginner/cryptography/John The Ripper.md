## John The Ripper
John the Ripper is one of the most well known, well-loved and versatile hash cracking tools out there. It combines a fast cracking speed, with an extraordinary range of compatible hash types.

#### John Basic Syntax
The basic syntax of John the Ripper commands is as follows.
`john [options] [path to file]`
`john` - Invokes the John the Ripper program
`[path to file]` - The file containing the hash you're trying to crack, if it's in the same directory you won't need to name a path, just the file.

#### Automatic Cracking  
John has built-in features to detect what type of hash it's being given, and to select appropriate rules and formats to crack it for you, this isn't always the best idea as it can be unreliable- but if you can't identify what hash type you're working with and just want to try cracking it, it can be a good option! To do this we use the following syntax:

`john --wordlist=[path to wordlist] [path to file]   `

`--wordlist=` - Specifies using wordlist mode, reading from the file that you supply in the following path...
`[path to wordlist]` - The path to the wordlist you're using, as described in the previous task.
**Example Usage:**
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

#### Identifying Hashes
To use hash-identifier, you can just pull the python file from gitlab using:
`wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`

`python3 hash-id.py` and then enter the hash you're trying to identify- and it will give you possible formats.

#### Format-Specific Cracking
Once you have identified the hash that you're dealing with, you can tell john to use it while cracking the provided hash using the following syntax:

`john --format=[format] --wordlist=[path to wordlist] [path to file]`
`--format=` - This is the flag to tell John that you're giving it a hash of a specific format, and to use the following format to crack it  
`[format]` - The format that the hash is in  

**Example Usage:**
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

then: ==john [path to file] —show —format=[format]==

### Cracking Windows Hashes
**NTHash / NTLM** - NThash is the hash format that modern Windows Operating System machines will store user and service passwords in.

What do we need to set the “format” flag to, in order to crack this?’
> **Answer: NT**
e.g: john ntlm-hash.txt --show --format=nt

### Cracking Hashes from /etc/shadow
The /etc/shadow file is the file on Linux machines where password hashes are stored. It also stores other information, such as the date of last password change and password expiration information.
#### Unshadowing
in order to crack /etc/shadow passwords, you must combine it with the /etc/passwd file in order for John to understand the data it's being given. To do this, we use a tool built into the John suite of tools called unshadow. The basic syntax of unshadow is as follows:
`unshadow [path to passwd] [path to shadow]`
`unshadow` - Invokes the unshadow tool  
`[path to passwd]` - The file that contains the copy of the /etc/passwd file you've taken from the target machine  
`[path to shadow]` - The file that contains the copy of the /etc/shadow file you've taken from the target machine.

**Example Usage:**
unshadow local_passwd local_shadow > unshadowed.txt
#### Cracking
We're then able to feed the output from unshadow, in our example use case called "unshadowed.txt" directly into John. We should not need to specify a mode here as we have made the input specifically for John, however in some cases you will need to specify the format as we have done previously using: `--format=sha512crypt`

`john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt`

### Using Single Crack Mode
`john --single --format=[format] [path to file]`
`--single` - This flag lets john know you want to use the single hash cracking mode.
**Example Usage:**
john --single --format=raw-sha256 hashes.txt

### Cracking a Password Protected Zip File
#### Zip2John
Similarly to the unshadow tool used previously, we're going to be using the zip2john tool to convert the zip file into a hash format that John is able to understand, and hopefully crack. The basic usage is like this:
`zip2john [options] [zip file] > [output file]   `

`[options]` - Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary  
`[zip file]` - The path to the zip file you wish to get the hash of
`>` - This is the output director, we're using this to send the output from this file to the...  
`[output file]` - This is the file that will store the output from


### Cracking a Password Protected RAR Archive
#### Rar2John
`rar2john [rar file] > [output file]   `

`rar2john` - Invokes the rar2john tool  
`[rar file]` - The path to the rar file you wish to get the hash of
`>` - This is the output director, we're using this to send the output from this file to the...  
`[output file]` - This is the file that will store the output from

### Cracking SSH Key Passwords
#### SSH2John

`ssh2john [id_rsa private key file] > [output file]   `

ssh2john - Invokes the ssh2john tool  
`[id_rsa private key file]` - The path to the id_rsa file you wish to get the hash of.

