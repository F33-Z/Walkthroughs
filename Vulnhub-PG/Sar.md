# Sar Walkthrough
Hello Hackers!

This is f33z, and Today i will share with you my walkthrough on how i solved the machine Sar in the Prouving Ground machines, This is exactly in the play section

## Port Scanning
Port scanning is my way to know open ports in this machine, and nmap is our friend
```bash
nmap -p- -T4 -v [IP-ADDRESS]
```
this revealed that there are two open ports : 22, 80
to check which services are running in these open ports, we run nmap with -sC and -sV
```bash
nmap -p22,80 -sC -sV [IP-ADDRESS]
```
and As expected, this machine is running a web server in port 80, and a ssh in the port 22.

## Enumeration
Since The version of ssh doesnt seem old and also we dont know the credentials to use to login to SSH, then we will start with port 80
### port 80 - HTTP
The root directory of the website was the default page of apache server, since we know there is a robots.txt file as we saw in the nmap results 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e3ed9372-6738-430a-91ab-8317fdaf566f)

We got `sar2HTML` string and after searching we found that this is a CMS, this CMS is deployed in http://[IP-ADDRESS]/sar2HTML

## Initial Access
Since this is the version 3.2.1, it is vulnerable to [Remote Command Execution](https://www.exploit-db.com/exploits/47204)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/8286fe94-3695-43ac-8f37-99da53df91bc)

After understanding the exploit, I applied it on our machine : and yes we got the output of `id` after clicking on the `host`

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/d1ad8db8-c454-4bda-a9d5-630c6a6247d7)

To get an Initial Access to the webserver, we will be be getting a reverse shell from [this](https://www.revshells.com/) website, the reverse shell that i chose is `nc mkfifo` and i set the IP ADDRESS and the port

I used Burpsuite so i can encode my payload by clicking `CTRL+u`

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f4a1ff9b-8092-4770-9312-20ad71f04904)


I also set a listener on the same port 
```bash
nc -nlvp 9001
```
And YUUUP we got initial access as www-data user

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/2f4f4a71-00e7-47ad-9be8-bda57c06fad0)




![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/a7235041-d739-4728-9985-c1cd2b8a389d)

## Privilege Escalation










