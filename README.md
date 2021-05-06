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
