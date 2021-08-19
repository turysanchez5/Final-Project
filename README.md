# Arturo Sanchez - Final-Project

# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:
nmap -sP 192.168.1.1-255

![nmap](https://user-images.githubusercontent.com/77302201/129827939-04b49b80-afa1-4456-bb61-9ef892ee5947.png)


This scan identifies the services below as potential points of entry:
- Target 1
  - Port 22 TCP ssh
  - Port 80 TCP http
  - Port 111 TCP rpcbind
  - Port 139 TCP netbios-ssn
  - Port 445 TCP netbios-ssn 

![nmapports](https://user-images.githubusercontent.com/77302201/129829554-69e288cf-f0b5-4f59-9a3e-61f5f51ddd32.png)

The following vulnerabilities were identified on the target:
- Target 1
  - Port 22 open
  - _Port 22 runs the risks of an attacker finding this open port and attacking it. 
  - Weak Password
  - _Weak passwords make it easy for hackers to gain unauthorized access to a system.

![wpscan](https://user-images.githubusercontent.com/77302201/129994784-9c1307cc-d771-4b78-9278-3a439e8d88a9.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _flag1{b9bbcb33e11b80be759c4e844862482d}
      - WPScan was used to capture user
      - Command: cd /var/www/ > grep -RE flag html 
  - `flag2.txt`: _flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
      - WPScan was used to capture user 
      - Command: cd /var/www > ls > cat flag2.txt



# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Kali
  - **Operating System**: Linux 2.6.32
  - **Purpose**: Attacking Machine
  - **IP Address**: 192.168.1.90
- ELK
  - **Operating System**: Ubuntu
  - **Purpose**: Machine Collecting Data
  - **IP Address**: 192.168.1.100
- Target 1
  - **Operating System**: Linux 3.2 -4.9
  - **Purpose**: Victims Machine
  - **IP Address**: 192168.1.110

### Description of Targets

The target of this attack was: 192.168.1.110

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Alert 1: HTTP Request Size Monitor

Alert 1 is implemented as follows:
  - **Metric**: WHEN sum() OF http.request.bytes OVER all documents
  - **Threshold**: IS ABOVE 3500
  - **Vulnerability Mitigated**: Alerts for high requests of HTTP.
![HTTP Request](https://user-images.githubusercontent.com/77302201/130006612-b5254358-a540-464e-93b4-ce8e1ff0ee2e.png)

#### Alert 2: CPU Usage Monitor
Alert 2 is implemented as follows:
  - **Metric**: WHEN max() OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: IS ABOVE 0.5
  - **Vulnerability Mitigated**: Alerts for high use of CPU caused by malicious software.
![CPU Usage](https://user-images.githubusercontent.com/77302201/130006204-b0771443-8456-4f80-8807-02c43e55dc83.png)

#### Alert 3: Excessive HTTP errors
Alert 3 is implemented as follows:
  - **Metric**: WHEN count() GROUPED OVER top 5 'http.response.status_code' 
  - **Threshold**: IS ABOVE 400
  - **Vulnerability Mitigated**: Alerts for possible brute force attacks.
![Excessive HTTP Errors](https://user-images.githubusercontent.com/77302201/130006852-bb13c0a0-f207-4ebb-a79d-597513f1b867.png)


