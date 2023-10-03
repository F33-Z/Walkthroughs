# DriftingBlues6 Walkthrough

Hello EveryOne,
Today we are going to Exploit the machine `DriftingBlues6` From Prouving Grounds Play Section, 

## port Scanning
As usual in each machine, we run a port scan, in the case of this machine, the only port that was open, it is the port 80,
```bash
nmap -p- -T4 -v [IP-ADDRESS]
```

```bash
nmap -p80 -sC -sV [IP-ADDRESS]
```

## Enumeration
### port 80 - HTTP
To Enumerate the port 80, we start by checking in the browser : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/9486d464-6777-4493-a598-0a0c26b1d19c)

Since, Nmap displayed that there is a robots.txt file in the web page, we check it and found the following :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/46d14c7e-b98d-41fc-9dff-10cacf8c7d63)

when we enter the directory `/textpattern/textpattern` we found in the robots.txt we get a login page of a CMS called textpattern

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/079a43b0-bdf1-4103-916f-f8c46e00fa0c)

To Follow the hint that we found about bruteforcing zip files, we run the following commands :
```bash
feroxbuster -u http://[IP-ADDRESS]/textpattern/textpattern -w /usr/share/wordlists/dirbuster/directory-list-2.8-medium.txt -x zip
```
```bash
feroxbuster -u http://[IP-ADDRESS]/ -w /usr/share/wordlists/dirbuster/directory-list-2.8-medium.txt -x zip
```
and indeed i found the a file called `spammer.zip` in the root directory of the web server

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e95e5e5c-1943-40ba-97c8-929208709965)

To unzip spammer.zip we will need a password, since it is a protected zip file, to do so, we run a bruteforce using `fcrackzip`
```bash
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt spammer.zip 
```
We indeed cracked the password, and got a creds.txt file that contains a username and password

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f11be7bd-aef9-42bc-b438-c7a62bcb62e8)

| Username   | Password   |
|-------------|-----------|
|  mayer      | lionheart |

## Initial Access
Using the credentials we found earlier, and after searching how to exploit this CMS i found that it is vulnerable to Unrestricted File upload, So i prepared my reverse shell in PHP :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/b4bf3357-400c-4bed-adec-bfa98261b277)

After modifying the IP address and also the port, I uploaded the shell into the CMS

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/25661cd4-e67d-46d9-a228-c48299996a60)

To make sure we can exploit this vulnerability, we will need to find the directory where we the uploaded files get saved, Doing another Directory Brute Force on the /textpattern directory i found the /files directory

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/9f0fa223-b41f-475e-b5f5-397007575f7f)

Before clicking on the `shell.php` we need to set a netcat listener 
```bash
nc -nlvp 8888
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/2169e875-a9d9-4e1f-894f-c82db5e2e387)

and YES we got INITIAL ACCESS!

To get a meterpreter shell which is more stable i used the module `web-delivery` that exist in metasploit
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/2a81bc72-23da-4323-99ac-1431e4848cc4)


## Privilege Escalation

while doing some enumeration, i found that this machine is old, Running the module `linux_suggest_exploit` indicates that the application is vulnerable to [dirtycow](https://www.exploit-db.com/exploits/40839)

I renamed the file dirty.c and i uploaded to the /tmp directory
then i run this command to compile the dirty.c script 
```bash
gcc -pthread dirty.c -o dirty -lcrypt
```
after that i was able to run the exploit by running :
```bash
./dirty
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/3b7f099c-9741-4382-9960-f0b4878ae04e)

I set the password as `dirty` and i used this password to log in to the `firefart` user :
```bash
su - firefart
```

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/9aa3cfae-93b3-4004-a575-480bd8a4020f)

And Yes We become GROOT!










