[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/mislavkovac)
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

<img width="841" alt="nmap-scan" src="https://user-images.githubusercontent.com/73172789/117553590-b8d97d00-b052-11eb-8cb4-026922816820.png">

Nmap scan shows us that port 80 is open. We can visit web page on port 80 with typing following in the browser: `http://<Machine_IP>/`  
Accessing webpage we see following:

<img width="1440" alt="web-page" src="https://user-images.githubusercontent.com/73172789/117553574-a5c6ad00-b052-11eb-809a-e05bb9a1c424.png">

On this page Rick is asking Morty for help. Morty needs to find out 3 secret ingredients so he can return Rick back. But problem is Rick forgot password of his PC.
We need to find out these 3 ingredients.

After we know that we can scout source code of web page. After viewing the source code we can see some interesting comment:

<img width="1438" alt="source-code" src="https://user-images.githubusercontent.com/73172789/117553562-99425480-b052-11eb-9061-10802877c449.png">

So, that is first step where we found the username. Next step is finding out where we can use this username. As we have web page running we can start scanning hiden 
folders of web page (web server) with dirb tool: `dirb http://<Machine_IP>/ <path_to_worldlist>`. I've used the common.txt wordlist from the dirb tool and I've got 
next results:

<img width="562" alt="dirb" src="https://user-images.githubusercontent.com/73172789/117553543-90518300-b052-11eb-8ba6-073c410a8cb1.png">

File robots.txt looks very interesting. We can check this file by accessing it from web browser: `http://<Machine_IP>/robots.txt`
So, we got:

<img width="1440" alt="robots-txt" src="https://user-images.githubusercontent.com/73172789/117553531-757f0e80-b052-11eb-8438-7dff084af62b.png">

This looks like a password that is hard to remember, so I assumed that is the password that Rick forgot. So, if this is the case we have username and potential 
password in robots.txt. But we don't know where we can login with this credentials. To find out where we can enter and check this credentials, we can 
start scan with tool called nikto with: `nikto -h <Machine_IP>`. After running scan with nikto we've got some interesting things:

<img width="1242" alt="Screenshot 2021-05-09 at 00 03 48" src="https://user-images.githubusercontent.com/73172789/117554846-6d2ad180-b05a-11eb-878c-1a2318241557.png">

We found something that we couldn't find with dirb scanner. New PHP script is found called login.php and we can access it through web browser with: `http://<Machine_IP>/login.php`. Name of this script is strong indication that we can use previous credentials for login. So, accessing this URL we have:

<img width="1438" alt="Screenshot 2021-05-09 at 00 15 59" src="https://user-images.githubusercontent.com/73172789/117555010-c5160800-b05b-11eb-9e6c-6c3e2dad9690.png">

Entering collected credentials we got a hit! After logging in with previous credentials we have got following web interface:

<img width="1431" alt="Screenshot 2021-05-09 at 00 18 14" src="https://user-images.githubusercontent.com/73172789/117555043-132b0b80-b05c-11eb-8194-a499a8268846.png">

In this web interface we see that we can execute commands like in terminal so let's try for example command `ls -la`. We get following:

<img width="1440" alt="Screenshot 2021-05-09 at 23 27 43" src="https://user-images.githubusercontent.com/73172789/117587425-33bb9a00-b11e-11eb-9c05-166b68e9af82.png">

So, we can see some interesting things like Sup3rS3cretPickl3Ingred.txt and clue.txt. We can try to read the Sup3rS3cretPickl3Ingred.txt with cat command, but we 
got:

<img width="1440" alt="Screenshot 2021-05-09 at 23 31 01" src="https://user-images.githubusercontent.com/73172789/117587525-aaf12e00-b11e-11eb-8c57-088ae9f7583a.png">

We need to find another way to read this file. Let's try accesing it with web browser, and we have a hit or the first answer:

<img width="1440" alt="Screenshot 2021-05-09 at 23 34 39" src="https://user-images.githubusercontent.com/73172789/117587613-2521b280-b11f-11eb-889d-63e5515c77f1.png">

Two more ingredients to find. Reading clue.txt file we found out that we can look around the file system to find other two ingredients. So let's try command `ls -la /` to see what is in root folder:

<img width="1440" alt="Screenshot 2021-05-09 at 23 38 47" src="https://user-images.githubusercontent.com/73172789/117587760-b7c25180-b11f-11eb-8155-9d5f46f03c09.png">

Now we can list `/home` directory:

<img width="1439" alt="Screenshot 2021-05-09 at 23 40 26" src="https://user-images.githubusercontent.com/73172789/117587802-f35d1b80-b11f-11eb-8842-f6561a6aa627.png">

There are two directories in `/home` directory: `ubuntu` and `rick`. `rick` directory is more interesting than ubuntu so let's check what is inside with `ls -la /home/rick`:

<img width="1440" alt="Screenshot 2021-05-09 at 23 43 14" src="https://user-images.githubusercontent.com/73172789/117587875-577fdf80-b120-11eb-8aa4-093eed06dc97.png">

In rick directory is only one file and we don't know what kind of this file is but we know that this is not directory. Also we know that we cant use `cat` command
but we can try to use `less /home/rick/'second ingredients'` command:

<img width="1440" alt="Screenshot 2021-05-09 at 23 45 53" src="https://user-images.githubusercontent.com/73172789/117587930-b7768600-b120-11eb-8a20-c518513751ec.png">

Now we have two out of three ingredients. Let's find out third one. When I was solving this question I assumed that third ingredient must be in root folder. Also I 
assumed that I don't have privileges for reading `/root` folder. But when I realized that I can use `sudo` with `sudo -l`:

<img width="1440" alt="Screenshot 2021-05-09 at 23 53 05" src="https://user-images.githubusercontent.com/73172789/117588116-b7c35100-b121-11eb-91a1-2f09e3823720.png">

So, now we can try to use `sudo ls -la /root` to list root directory:

<img width="1440" alt="Screenshot 2021-05-09 at 23 53 05" src="https://user-images.githubusercontent.com/73172789/117588146-dcb7c400-b121-11eb-8480-da3ae5f418a7.png">

File 3rd.txt looks promisingly so let's try to use `sudo less /root/3rd.txt` to se its content:

<img width="1440" alt="Screenshot 2021-05-09 at 23 55 35" src="https://user-images.githubusercontent.com/73172789/117588173-11c41680-b122-11eb-80dc-82a9acb3d42d.png">

And we got the third ingredient!

That's it we solve this CTF. You can find this CTF by TryHackMe on link: https://tryhackme.com/room/picklerick 

If you like this writeup please consider supporting me on link in supporting section on the right side of your screen or by clicking on the donation button at the 
top of this text. Thanks in advance.
