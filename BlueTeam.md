# Blue Team: Summary of Operations

### Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic and Behavior
- Suggestions for Going Further

### Network Topology
The following machines were identified on the network:

**Kali**
- Operating System: Debian Kali 5.4.0
- Purpose: The attacking machine used to conduct the pen test
- IP Address: 192.168.1.90

**ELK**
- Operating System: Ubuntu 18.04
- Purpose: ELK monitoring stack
- IP Address: 192.168.1.100

**Target 1**
- Operating System: Debian GNU/Linux 8
- Purpose: Vulnerable Wordpress host, sends logs to ELK
- IP Address: 192.168.1.110

**Capstone**
- Operating System: Ubuntu 18.04
- Purpose: Vulnerable Web Server
- IP Address: 192.168.1.105

### Description of Targets

- The target of this attack was Target 1 (192.168.1.110)
- Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are potential points of entry, which was verified by the nmap scan below 

### Monitoring the Targets

**Target 1**
- Port 22/TCP 	Open 	SSH	OpenSSH 6.7p1 Debian 5+deb8u4
- Port 80/TCP 	Open 	HTTP	Apache httpd 2.4.10 (Debian)

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/Nmap-Scan.png)

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

**Excessive HTTP Errors**

`WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes`

-  Metric: 
    - **WHEN count() GROUPED OVER top 5 ‘http.response.status_code’**
- Threshold: 
    - **IS ABOVE 400**
- Vulnerability Mitigated:
    - **Enumeration/Brute Force**
- Reliability: 
    - The alert has demonstrated highly reliability 

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/HTTP-Errors-alert.png)

**HTTP Request Size Monitor**

`WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute`

- Metric: 
    - **WHEN sum() of http.request.bytes OVER all documents**
- Threshold: 
    - **IS ABOVE 3500**
- Vulnerability Mitigated: 
    - **Code injection in HTTP requests (XSS and CRLF) or DDOS**
- Reliability:
    - This alert demonstrates medium reliability, as it has produced some false positives

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/HTTP-Request-size-alert.png)

**CPU Usage Monitor**

`WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes`

- Metric: 
    - **WHEN max() OF system.process.cpu.total.pct OVER all documents**
- Threshold: 
    - **IS ABOVE 0.5**
- Vulnerability Mitigated: 
    - **Malicious software, programs (malware or viruses) running taking up resources**
- Reliability: 
    - The alert demonstrates high reliability 

![alt text](https://github.com/carlwarnberg/Project-3/blob/main/Images/CPU-Usage-alert.png)

### Suggestions for Going Further

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

**Brute Force Vulnerability**
- Patch: Implement Two-Factor Authentication on employee accounts
- Why It Works: 
    - 2FA adds an additional layer of security to accounts. Even if a malicious user were able to obtain a username and password, the additional login authentication requirement would prevent them from gaining access
    
**DDos Vulnerability**
- Patch: Implement a scrubbing server on the network
   
- Why It Works:
    - The scrubbing server would be a dedicated machine to receive all of the network traffic destined for the target machine. This scrubbing server would be used to filter traffic, and only send non-DDoS packets. While not entirely foolproof, it would be a good addition to the overall security framework  

**Malware/Virus Vulnerability**
- Patch: Install Anti-Virus / Anti-Malware software on employee devices
  
- Why It Works: 
    - Anti-Virus software scans files or code being passed through network traffic and compares them to a database of known threats. If there is a match, it will block the malicious files
