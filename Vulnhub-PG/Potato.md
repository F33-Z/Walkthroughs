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

after openening these files, we found the backup of the index.php file, where we find credentials of the user admin and the code that the application was using it, or it may be still using same code
#### Port 80 - HTTP
In the browser, we checked 
### Initial access
### Privilege Escalation
