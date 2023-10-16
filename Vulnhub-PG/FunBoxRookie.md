# FunBoxRookie Walkthrough

## port scanning

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/3f4dac0e-4059-428c-9b88-cd15b3c5a6ad)

## Enumeration
### ftp

anonymous access :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/fed1ff91-4881-4c89-8a45-017f61063488)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/42115d45-303e-4dc2-b7e8-40c56ff30d1e)

same message :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/00191977-868f-4efb-a6e4-b7816d2b7658)

all zip files seem protected

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/42796bad-4555-4e4e-a059-3b887afaf91e)

ls | grep -Po '.*?(?=.zip)'

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/07f6edf0-36a6-4402-93e0-730aa022a1a1)

automate cracking all zip password :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/8be90ca5-7755-4b3e-be09-e42d4dd31dda)

results : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/09b392ef-17e4-4afd-b15b-768f7988a929)

cathrine.zip == catwoman
tom.zip == iubire

## Initial Access
after getting credentilas we unzipped both of them, we got two id-rsa, but only the key of tom worked 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/620def78-ba50-40ae-81dc-7eb39f823638)

to fix the problem of rbash shell :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/ea23864f-b578-473d-a73a-8f796ffe8ad8)


## Privilege Escalation
exploiting lxd : https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation

```bash
git clone https://github.com/lxc/distrobuilder
#Make distrobuilder
cd distrobuilder
make
#Prepare the creation of alpine
mkdir -p $HOME/ContainerImages/alpine/
cd $HOME/ContainerImages/alpine/
wget https://raw.githubusercontent.com/lxc/lxc-ci/master/images/alpine.yaml
#Create the container
sudo $HOME/go/bin/distrobuilder build-lxd alpine.yaml -o image.release=3.18
```
in my kali :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f4f8a636-312c-4b2d-b726-3ce8ef4ee2e0)

in the target :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/50aeec9b-801a-427e-8cb9-88ffb5759ae9)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f907055f-e2de-491e-b990-d820066a0f67)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/5aca7d8b-7fb3-414c-86f9-666de12ab3c5)









