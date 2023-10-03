# MoneyBox Walkthrough
Hello There, 

This is f33z and i am here to share my walkthrough On how i Got root access to the machine MoneyBox in the play section of proving grounds, which means it does also belong to Vulnhub

## Port Scanning
We run Nmap as usual to find open ports : 
```bash
nmap -p- -T4 -v [IP-ADDRESS]
```
- -p- : for scanning all ports 0-65,535
- -T4 : for Quick Scan T1-T5
- -v : for verbose mode

We got that there is two open port 21,22,80 to check their services and their version, we run the command 
```bash
nmap -p21,22,80 -sC -sV [IP-ADDRESS]
```
this screen shows the we have the services FTP, SSH, HTTP for the followings 21,22,80 ports

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/502374f7-7579-42fe-9597-3e53c10ad690)

## Enumeration
### 21 Port - FTP
Since we can log in to FTP using anonymous access, we found an image named `trytofind.jpg`

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/ac4f71f0-45e3-4d5b-bfdb-b592d960bcb0)


this version of FTP `vsFTPd 3.0.3` does not seem to have an exploitable vulnerability, accourding to the results of this command :
```bash
searchsploit vsFTPd 3.*
```
### 80 Port - HTTP

the IP Address displays this web page, this indicates that this machine is more CTF-like, so we can think like if we are solving CTFs instead of focusing on realistic attacks only

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/0634847f-4cf3-4fcc-a146-c03c4f0015a8)

After running a Directory brute force on this web page we found that there is /blog directory
```bash
dirsearch -u http://IP-ADDRESS/
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/db451dc3-0475-4dae-9d1e-118565906f3c)

In the source code of this page, we found 
```html
<!--the hint is the another secret directory is S3cr3t-T3xt-->
```

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/ac6191d4-ad37-4b04-b32a-036801830715)

when we navigate to /blog/S3cr3t-T3xt directory, we found this page 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/4f6388a4-58e8-4bc2-9fa1-5f4eea018b93)

this page also had this `<!..Secret Key 3xtr4ctd4t4 >` hidden in its source code 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/5a142371-df80-4cef-9060-440fb5625325)


## Initial Access







## Privilege Escalation










