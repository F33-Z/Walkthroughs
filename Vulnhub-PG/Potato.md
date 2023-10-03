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

we did find a login page in the /admin page :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/202c4e3e-fe1a-461f-af83-4fb47c6b6a4b)

Trying the credentials to login it and also some default credentials like admin:admin, admin:password still we failed to login as admin
after checking the /admin/logs directory :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/115b68ca-bfc0-485a-aad3-2c95adfc7c2b)
these files indicated that there was a change in the password of the admin user.

### Port 22 - SSH
Since the version of this service wasn't old and it didnt had any known vulnerabilities, the only thing i tried was to log in to ssh using the credentials we found earlier admin:potato but it failed

### Initial access

### Privilege Escalation
