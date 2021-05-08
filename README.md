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

So, that is first step where we found the username: `R1ckRul3s`
Next step is finding out where we can use this username. As we have web page running we can start scanning hiden folders of web page (web server) with dirb tool: `dirb http://<Machine_IP>/ <path_to_worldlist>`. I've used the common.txt wordlist from the dirb tool and I've got next results:

<img width="562" alt="dirb" src="https://user-images.githubusercontent.com/73172789/117553543-90518300-b052-11eb-8ba6-073c410a8cb1.png">

File robots.txt looks very interesting. We can check this file by accessing it from web browser: `http://<Machine_IP>/robots.txt`
So, we got:

<img width="1440" alt="robots-txt" src="https://user-images.githubusercontent.com/73172789/117553531-757f0e80-b052-11eb-8438-7dff084af62b.png">

This looks like a password that is hard to remember, so I assumed that is the password that Rick forgot. So, if this is the case we have username `R1ckRul3s` and 
potential password in robots.txt. But we don't know where we can login with this credentials. To find out where we can enter and check this credentials, we can 
start scan with tool called nikto with: `nikto -h <Machine_IP>`. After running scan with nikto we've got some interesting things:

<img width="1242" alt="Screenshot 2021-05-09 at 00 03 48" src="https://user-images.githubusercontent.com/73172789/117554846-6d2ad180-b05a-11eb-878c-1a2318241557.png">

We found something that we couldn't find with dirb scanner. New PHP script is found called login.php and we can access it through web browser with: `http://<Machine_IP>/login.php`. Name of this script is strong indication that we can use previous credentials for login. So, accessing this URL we have:

<img width="1438" alt="Screenshot 2021-05-09 at 00 15 59" src="https://user-images.githubusercontent.com/73172789/117555010-c5160800-b05b-11eb-9e6c-6c3e2dad9690.png">

Entering collected credentials we got a hit! After logging in with previous credentials we have got following web interface:

<img width="1431" alt="Screenshot 2021-05-09 at 00 18 14" src="https://user-images.githubusercontent.com/73172789/117555043-132b0b80-b05c-11eb-8194-a499a8268846.png">
