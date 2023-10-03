# Potato Walkthrough 
Hi,
In this walkthrough, we will see how we exploit the potato machine from prouving ground play, 

### port Scanning
As Usual we start by a port scan, and to run a quick Nmap Scan, I have run this nmap command :
```bash
nmap -p- -T4 -v [IP-ADDRESS]
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/469ef562-12ac-4335-a000-94e6f385d511)

I discovered 3 open ports, 22,80 and 2112, to check what services are running in these ports, i run the command :
```bash
nmap -p22,80,2112 -sC -sV [IP-ADDRESS]
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/04c14b18-25f9-4e79-9c3f-e0cf9eddfeb7)

So there is SSH, HTTP and FTP server
### Enumeration
To continue we are going to Enumerate each port :
#### Port 2112 - FTP
Since we get that there is anonymous login to the ftp server, then we can log in using anonymous username:

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/bd6fce0a-16f8-46d9-9493-235daff4aece)

after openening these files, we found the backup of the index.php file, where we find credentials of the user admin 
and the code that the application used to use, or it may be still using same code.

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/d0869f4e-6f9b-4dac-9eff-ab22a49d9ac3)


| UserName      | Password     |
| ------------- |:-------------:| 
| admin     | potato |

I used this credentials to try to relogin to FTP, but credentials were incorrect
#### Port 80 - HTTP
In the browser, we Entered the IP address of the machine :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/3d8aed12-67ab-41d9-95ec-6bbaba329f83)

then we did run a Directory Brute force using dirsearch to find hidden Directories
```bash
dirsearch -u http://[IP-ADDRESS]
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/08dac75a-24d6-4303-b016-d38be75fd47d)

we did find a login page in the /admin page:

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/202c4e3e-fe1a-461f-af83-4fb47c6b6a4b)

Trying the credentials to login it and also some default credentials like admin:admin, admin:password still we failed to login as admin
after checking the /admin/logs directory :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/92fcb0b4-437a-46c6-bce4-7ebfbddbcfb8)


these files indicated that there was a change in the password of the admin user.

### Port 22 - SSH
Since the version of this service wasn't old and it didnt had any known vulnerabilities, the only thing i tried was to log in to ssh using the credentials we found earlier admin:potato but it failed

### Initial access
After being in a rabbit-hole for a bit, trying to use the credentials i found, and i kept throwing them everywhere there is a login, I went back to the code where there is the code that handled the login, there was a function called `strcmp`, after searching little bit about this function, i found that I can bypass the login since it is a vulnerable function,
You can read more about it in this [writeup](https://www.doyler.net/security-not-included/bypassing-php-strcmp-abctf2016)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e9e49072-06c9-4f35-a93c-9672b8a86820)

So i used this payload to bypass the login using Burpsuite :
```php
username[]=%22%22&password[]=%22%22
```
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/0a65821d-5e3d-4b40-a2eb-2faba5f4df47)

As you can see we got a message that say welcome, which means we successfully bypassed the authentication, 

while nevigating though the application, and while trying to download some log files i noticed there was a `file` parameter, i was able to see it using burpsuite, so i tried some LFI payloads and indeed i got the content of /etc/passwd

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/0a29c444-2882-4149-9ea1-2c57491c6412)







### Privilege Escalation
