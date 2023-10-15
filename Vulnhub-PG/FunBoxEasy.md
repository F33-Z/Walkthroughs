# FunBoxEasy Walkthrough

## Port Scanning
After Port scanning with nmap, we discovered port 80, and 22 open 


## Enumeration
running nikto : 
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/fb73659a-08fc-48f2-9bb8-f9a6ac71653b)

robots:

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/05d68442-94f9-44e5-9aa6-1d135ad9aa3a)

running feroxbuster for directory brute forcing :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/c0e44ec6-c458-43ed-b69a-28135699e6d0)

store website :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/9b11a81b-0219-4fc1-b142-650272116ddc)

Gym website : 
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/c3aa231e-fcf1-4222-9863-7e66699c7bd6)

## Initial Access
autheticated RCE in the online book store :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/050ea898-31b4-47b0-badc-9a6967ba3c0f)
Initial Access :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/b593e1e6-3587-4c0f-8457-e946834a0907)

Stabilizing the shell with metasploit :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/081e856c-51a8-4409-86b0-5b2c6dea4a52)
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/db469f9a-4250-4e96-84c6-883e00e83cfc)



## Privilege Escalation
running linpeas :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/64a7a8ec-5f66-40a7-a90f-1179c938b085)

exploiting SUID `time` :
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f90bb145-b3f7-41cf-a747-a1ab7e9b370c)








