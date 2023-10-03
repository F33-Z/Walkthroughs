# Potato Walkthrough 
### port Scanning
To run a quick Nmap Scan, I have run this nmap command :
bash```
nmap -p- -T4 -v [IP-ADDRESS]
```
I discovered 3 open ports, 22,80 and 2112, to check which services are running in these ports, i run the command :
bash```
nmap -p22,80,2112 -sC -sV [IP-ADDRESS]
```
### Enumeration
To continue we are going to Enumerate each port :
#### Port 2112 - FTP
Since we get that there is anonymous login to the ftp server, then we can log in :

after openening these files, we found the backup of the index.php file, where find credentials of the user admin
#### Port 80 - HTTP
### Initial access
### Privilege Escalation
