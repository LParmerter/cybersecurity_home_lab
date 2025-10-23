#Lab #Cybersecurity 

1. Upon first opening my virtual machine in ubuntu, I ran the ``sudo apt update`` command to see what updates my machine could perform. 
![[Pasted image 20251002180930.png]]
	I then confirmed the updates to install them with ``sudo apt upgrade`` and restarted the system with ``reboot``. 

**Exploring Users**
2. Using ``sudo su root`` I opened up a root shell, noting the ``#`` replacing the usual ``$`` in the prompt. With the open root shell, I was able to create two new users: "bobby", using ``useradd``, and "sally", using ``adduser``.
![[Pasted image 20251002203028.png]]
	this showed that the main difference between these two commands was that ``adduser`` is a script, able to create a home directory, group, a password, and other related information to fully set up the new user, whereas ``useradd`` simply creates the user with no other information.

3. Next, I logged in as sally with ``sudo su``, which was then reflected in the prompt, and tested the default permissions by attempting to create a new user, "earl". 
![[Pasted image 20251002204541.png]]
	this resulted in an error, as the new user "sally" does not have root or sudo access to add a new user to the system. 

4. Having tested sally, I reverted to the original user, ubuntu, using ``exit`` twice, and used the command ``sudo userdel bobby`` to delete the other user created previously, bobby.
![[Pasted image 20251002205647.png]]
	I then used ``cat /etc/passwd`` to check that bobby had in fact been deleted. No extra directories were left over as bobby was created with ``useradd``, unlike sally. 

5. Now that I was done with sally for now, I changed the user's password to something I would remember with ``sudo passwd sally``. I then checked my user id with ``id``, which was 1000.
![[Pasted image 20251002210932.png]]
	the rest of this testing was completed in the ubuntu user and sally, rather than the root user. While the root user avoids having to log in and use sudo, it's bad practice to stay logged in as root and leaves a system vulnerable to outside attacks. 

**Exploring Groups**
6. First I wanted to check what group the ubuntu user belongs to, which could be done easily with the ``groups`` command. I then compared this with the groups sally belongs to, and used the ``usermod`` command with the argument ``-aG``to give sally sudo privileges by adding them to the "sudo" group. 
![[Pasted image 20251002212251.png]]

7. I then created "cybersec", a new group for sally, using the ``groupadd`` command, and added them using ``usermod -aG`` once again. Upon re-checking which groups user sally is a part of, sudo and sybersec are now displayed as well.
![[Pasted image 20251002213305.png]]

**Permissions and Access Control Lists**
8. Using ``mkdir``, I created a new directory in the home directory, called homelab1. I then checked for the specific permissions of the directory using ``ls -ld``, which revealed the owner and group owner of the directory to be myself, user logan. This also showed that both as owner and group owner, I have read, write, and execute capablities (as shown with ``[rwxrwx]r-x``), but other users may only read and execute (as shown by the ``-`` in ``rwxrwx[r-x]``). 
![[Pasted image 20251002214051.png]]

9. Next, I wanted to create a simple bash executable able to print "Hello World!". So I changed my directory using ``cd`` to homelab1, and created the bash file using ``nano``. That opened the interface seen below, where I added the shebang (``#!/bin/bash``) and the command ``echo "Hello World!"``.  The shebang tells Linux to execute the script using bash, and ``echo`` is used to print "Hello World!" to the terminal. 
![[Pasted image 20251002220628.png]]
![[Pasted image 20251002220157.png]]
	Once the script itself was written, I exited back to the terminal and used ``chmod +x`` to add the ability to execute the helloWorld file I just created. I was then able to execute it with ``./helloWorld``, which successfully output "Hello World!" to the terminal!

10. Having just made the program, I wanted to check what permissions it had by default. Using ``ls -la``, I saw that the owner and group had read write and execute permissions, whereas once again other could only read and execute. I decided that for this file, I wanted the group to only be able to write and execute, so I changed these permissions using the ``chmod g=wx`` command seen below. 
![[Pasted image 20251002223042.png]]

 11. As a last point of exploration, I wanted to view and change the Access Control List of the helloWorld file. first I checked the current list of permissions with ``getfacl``, and then used ``setfacl -m`` to modify user sally's file permissions, specifying that they should have the ability to read and write to the file only. 
![[Screenshot 2025-10-02 223541 1.png]]

And that concludes my introductory exploration of the Linux command line and the basics of file security!