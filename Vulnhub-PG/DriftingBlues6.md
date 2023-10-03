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











## Privilege Escalation














