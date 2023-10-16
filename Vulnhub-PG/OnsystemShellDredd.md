# OnsystemShellDredd Walkthrough

## port scanning
port 21,61000

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/e95851aa-5c81-44cc-a7e2-a9e87609a520)

## Enumeration
ftp > .hannah > id_rsa
![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/20573061-d875-4f5e-a863-8aae2cd2d6ee)

## Initial Access
ssh by user hannah and id_rsa

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/39b59771-50a8-4030-8495-65990f3c3670)

## Privilege Escalation
running linpeas.sh

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/9263ffdd-34ce-47b9-b1f6-faeee7c6e1c0)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/fa982eb3-6d20-48f6-bcc4-2fddf2cb08fb)

SUID CPUlimit

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/26dffb88-9c3c-468a-b5d8-fcf4a620261e)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/65d05eaf-e464-44c7-9b7d-5076b3ccff12)

