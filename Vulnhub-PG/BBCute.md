# BBCute Walkthrough

## port scanning
found following ports :

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/4fc184fe-3134-4898-bd75-a2695e933c83)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/5d27b0d2-0ab3-4ae6-b742-ffb82ba6da6e)


## Enumeration
wesbite was a ubuntu default page
dirb : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/86588837-213d-4100-bd1f-bfa616c1ae77)

index.php was this website : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/119dfb89-31f6-4463-ae8d-d8641c01f546)

it is cutenews v2.1.2
## Initial Access
there is this exploit : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/fcdc9958-fcb5-4892-bbd0-8a29a26aad65)

we modified the script by reving /cutenews

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/f85dbdd2-91df-47c9-b49e-c85047f04a60)

we were able to run commands : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/fcdb73ae-d97f-48a5-a826-218b9961165d)

getting a nc shell : 

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/d2fdead4-75e4-4a91-994e-3717bee183e3)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/8ec22810-e254-43a0-a4e9-05136e1edd7f)

## Privilege Escalation
exploiting hping3

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/95d4aee2-ac84-48a2-b6d8-c8dd8830ac99)

![image](https://github.com/F33-Z/Walkthroughs/assets/73140750/4c76dd3f-1994-4515-af9d-49a77ad98d5d)








