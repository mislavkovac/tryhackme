# Pickle Rick
This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.
## Introduction
First of all we need to start machine. I'm assuming that you are connected to VPN. So, we have 3 tasks to complete:
  * What is the first ingredient Rick needs?
  * Whats the second ingredient Rick needs?
  * Whats the final ingredient Rick needs?
Now when we know what we need to find, we can start hacking. Let's roll!
## What is the first ingredient Rick needs?
We can scout open ports with nmap: `nmap -T4 -p- -A <Machine_IP>`
After running nmap scan we get next results: 

![alt text](https://github.com/mislavkovac/tryhackme/blob/main/nmap-scan.png)
Nmap scan shows us that port 80 is open. We can visit web page on port 80 with typing following in the browser: `http://<Machine_IP>/`  
Accessing webpage we see following:

![alt text](https://github.com/mislavkovac/tryhackme/blob/main/web-page.png)

On this page Rick is asking Morty for help. Morty needs to find out 3 secret ingredients so he can return Rick back. But problem is Rick forgot password of his PC.
We need to find out these 3 ingredients.

After we know that we can scout source code of web page. After viewing the source code we can see some interesting comment:

![alt text](https://github.com/mislavkovac/tryhackme/blob/main/source-code.png)

So, that is first step where we found the username: `R1ckRul3s`
Next step is finding out where we can use this username. As we have web page running we can start scanning hiden folders of web page (web server) with dirb tool: `dirb http://<Machine_IP>/ <path_to_worldlist>`. I used common.txt wordlist from dirb tool. I've got next results:

![alt text](https://github.com/mislavkovac/tryhackme/blob/main/dirb.png)
