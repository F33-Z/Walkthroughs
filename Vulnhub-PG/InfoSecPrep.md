# InfoSecPrep Walkthough

## Port Scanning
22,80
## Enumeration
the website : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/75ae2231-27aa-44a4-85b0-db25a2ff6231)

one user : oscp
nikto : 
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/43ec8bcb-579a-480f-9418-f7edb9bdb6ba)

robots.txt :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/200fe969-c93a-4b97-af31-e9c80aca8435)

## Initial Access
decoding secret.txt :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/3382925b-c2d2-46d0-a097-8025cf621056)

SSH using `oscp` user and the private key :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e6f391c9-7a10-4245-aa9f-65feb2e83f57)


## privilege Escalation
linpeas result :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/3dbd25f7-8944-4694-b50d-c07d40df803c)

many Attack vectors : Bash SUID
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f9a517fc-bcee-4fe1-9d70-118a0894b083)

