### 

### 

# *Computer Networks \- Mini Project Report*

Course Title : Computer Networks  
Course Code : CSE405  
Section : 03  
Semester : Summer2025

Submitted To:  
**Dr. Anisur Rahman**

Associate Professor

Department of Computer Science & Engineering,  
East West University.

Submitted By: 

|  Name | ID |
| :---: | :---: |
| Abrar Khatib Lajim | 2022-3-60-043 |

### 

### 

### 

Submission Date: *14/09/2025*  

## **1\. Introduction** 

Apex University, like East West University, operates a multi-campus environment with a wide range of academic and administrative facilities. Its network infrastructure must support thousands of users across different campuses with reliable wired and wireless connectivity, centralized services, and scalability for future growth.  
This project focuses on designing a complete network model using **Cisco Packet Tracer**, integrating **routing, DHCP, DNS, and web services** to achieve seamless connectivity across eight campuses.

## **2\. Purpose of the Network** 

The primary goals of the network are:

1. Provide **wired and wireless access** in each campus.

2. Ensure centralized **DHCP service** for IP allocation.

3. Deploy a **single DNS server** for name resolution.

4. Host a **web server** accessible via http://www.apex.edu.bd.

5. Implement efficient **routing (OSPF)** for inter-campus connectivity.

6. Support future expansion of subnets and services.

## 

## **3\. Physical Topology** 

The university network consists of **eight campuses** connected through routers in the topology shown below.

**\#Campus 1, 2, 3, 4, 6, 7** → Have LANs (wired \+ wireless).

**\#Campus 5, 8** → Serve as backbone/connection hubs;       Campus 8 also hosts the servers.

### **Est. Router Connections Table (Topology)** 

| Router | Ports | Connected To |
| :---- | :---: | :---: |
|  ***R1 (Campus 1\)*** |  fa0/0, fa0/1, se2/0, se3/0 | ***Campus 8*** (se2/0),  ***Campus 2*** (se3/0) |
|  ***R2 (Campus 2\)*** |  fa0/0, fa0/1, se2/0, se3/0 | ***Campus 8*** (se2/0),  ***Campus 1*** (se3/0) |
|  ***R3 (Campus 3\)*** |  fa0/0, fa0/1, se2/0, se3/0 | ***Campus 5*** (se2/0),  **Campus 4** (se3/0) |
|  ***R4 (Campus 4\)*** |  fa0/0, fa0/1, se2/0, se3/0 | **Campus 5** (se2/0),  **Campus 3** (se3/0) |
|  ***R5 (Campus 5\)*** |  fa0/0, fa0/1, se0/0/0, se0/0/1, se0/1/0 |    ***Campus 8*** (se0/0/0),     **Campus 3** (se0/0/1),    **Campus 4** (se0/1/0) |
|  ***R6 (Campus 6\)*** |  fa0/0, fa0/1, se2/0, se3/0 | ***Campus 8*** (se2/0),  ***Campus 7*** (se3/0) |
|  ***R7 (Campus 7\)*** |  fa0/0, fa0/1, se2/0, se3/0 | ***Campus 8*** (se2/0),  ***Campus 6*** (se3/0) |
|  ***  R8 (Campus 8\)*** |   fa0/0, fa0/1, se0/0/0, se0/0/1, se0/1/0, se0/1/1, se0/2/0 | ***Campus 1*** (se0/0/0),  ***Campus 2*** (se0/0/1),  ***Campus 7*** (se0/1/0),  ***Campus 6*** (se0/1/1),  ***Campus 5*** (se0/2/0) |

## **4\. IP Addressing Scheme** 

We use a **Class B private network (172.16.0.0/16)**. Each campus receives a dedicated /24 subnets for easy management. Router-to-router serial links use /30 subnets.

### **4.1 LAN Subnets** {#4.1-lan-subnets}

| Campus | Subnet | Gateway (Router Fa0/0) | Notes |
| :---- | :---: | :---: | :---: |
| Campus 1 | 172.16.1.0/24 | 172.16.1.1 | Wired & wireless PCs |
| Campus 2 | 172.16.2.0/24 | 172.16.2.1 | Wired & wireless PCs |
| Campus 3 | 172.16.3.0/24 | 172.16.3.1 | Wired & wireless PCs |
| Campus 4 | 172.16.4.0/24 | 172.16.4.1 | Wired & wireless PCs |
| Campus 5 | 172.16.5.0/24 | 172.16.5.1 | Wired & wireless PCs |
| Campus 6 | 172.16.6.0/24 | 172.16.6.1 | Wired & wireless PCs |
| Campus 7 | 172.16.7.0/24 | 172.16.7.1 | Wired & wireless PCs |
| Campus 8  | 172.16.8.0/24 | 172.16.8.1 | DHCP, DNS, Web, others |

### **4.2 Server IPs** 

**Campus 8 (Server LAN)**

| Server | IP Address | Purpose |
| ----- | :---: | :---: |
| DHCP Server | 172.16.8.20 | Assigns IP to all campuses |
| DNS Server | 172.16.8.10 | Resolves `www.apex.edu.bd` |
| Web Server | 172.16.8.15 | University website |
| Additional Servers (optional) | 172.16.8.30+ | Admissions, library, accounts, etc. |

### **4.3 Router Interconnections** {#4.3-router-interconnections}

### **4.3.1 Serial Point-to-Point Links** 

| Link  | Subnet (/30) | Router A (IP) | Router B (IP) |
| :---: | :---: | :---: | :---: |
| R1 ↔ R8 | ***10.0.0.0/30*** | R1-Se2/0: 10.0.0.1 | R8-Se0/0/0: 10.0.0.2 |
| R1 ↔ R2 | ***10.0.0.4/30*** | R1-Se3/0: 10.0.0.5 | R2-Se2/0: 10.0.0.6 |
| R2 ↔ R8 | ***10.0.0.8/30*** | R2-Se3/0: 10.0.0.9 | R8-Se0/0/1: 10.0.0.10 |
| R8 ↔ R7 | ***10.0.0.12/30*** | R8-Se0/1/0: 10.0.0.13 | R7-Se2/0: 10.0.0.14 |
| R7 ↔ R6 | ***10.0.0.16/30*** | R7-Se3/0: 10.0.0.17 | R6-Se2/0: 10.0.0.18 |
| R6 ↔ R8 | ***10.0.0.20/30*** | R6-Se3/0: 10.0.0.21 | R8-Se0/1/1: 10.0.0.22 |
| R5 ↔ R8 | ***10.0.0.24/30*** | R5-Se0/0/0: 10.0.0.25 | R8-Se0/2/0: 10.0.0.26 |
| R5 ↔ R3 | ***10.0.0.28/30*** | R5-Se0/0/1: 10.0.0.29 | R3-Se2/0: 10.0.0.30 |
| R3 ↔ R4 | ***10.0.0.32/30*** | R3-Se3/0: 10.0.0.33 | R4-Se2/0: 10.0.0.34 |
| R4 ↔ R5 | ***10.0.0.36/30*** | R4-Se3/0: 10.0.0.37 | R5-Se0/1/0: 10.0.0.38 |

### **4.4 Routing Protocol** 

We use **OSPF (area 0\)** for scalability and dynamic route updates. All routers advertise their LAN's and serial networks into OSPF. Since the network has **8 routers across multiple campuses**, OSPF (Open Shortest Path First) is chosen because:

1. It is a **dynamic routing protocol** (no need to manually update routes).

2. Works well with **large, hierarchical networks**.

3. Supports **VLSM (Variable Length Subnet Masking)**.

4. Converges faster than RIP.

***Router Orientation***

## **5\. Router Configurations** 

All routers are Cisco IOS based. The network uses **OSPF (area 0\)** for routing and a **central DHCP server at Campus 8**. 

| *Campus 1 Router  ![][image1]* | *Campus 2 Router  ![][image2]* |
| :---- | :---- |
| ***Campus 3 Router ** ![][image3]* | ***Campus 4 Router ** ![][image4]* |
| ***Campus 5 Router ** ![][image5]* | ***Campus 6 Router ** ![][image6]* |
| ***Campus 7 Router  ![][image7]***  | ***Campus 8 Router (Core \+ Servers)**   `![][image8]`* |

### **Sample Configuration Brief:** 

**Router 8 \- Capmpus 8**![][image9]  
![][image10]

**Router 5 \- Campus 5** ![][image11]

## **6\. Server Configurations** 

### **6.1 DHCP Server** 

The DHCP server (IP: **172.16.10.11**) is configured to allocate IP addresses dynamically to clients from **all campus LAN subnets**. Each pool is defined with the default gateway (router interface), DNS server, and valid IP ranges.

IP Address: `172.16.8.100`

Subnet Mask: `255.255.255.0`

Gateway: `172.16.8.1` (first last host in Server LAN)

| Pool Name | Default Gateway | Subnet | Start IP | End IP |
| ----- | :---: | :---: | ----- | ----- |
| Campus1 | 172.16.1.1 | 255.255.255.0 | 172.16.1.10 | 172.16.1.200 |
| Campus2 | 172.16.2.1 | 255.255.255.0 | 172.16.2.10 | 172.16.2.200 |
| Campus3 | 172.16.3.1 | 255.255.255.0 | 172.16.3.10 | 172.16.3.200 |
| Campus4 | 172.16.4.1 | 255.255.255.0 | 172.16.4.10 | 172.16.4.200 |
| Campus5 | 172.16.5.1 | 255.255.255.0 | 172.16.5.10 | 172.16.5.200 |
| Campus6 | 172.16.6.1 | 255.255.255.0 | 172.16.6.10 | 172.16.6.200 |
| Campus7 | 172.16.7.1 | 255.255.255.0 | 172.16.7.10 | 172.16.7.200 |
| Campus8 | 172.16.8.1 | 255.255.255.0 | 172.16.8.50 | 172.16.8.100 |

### **Implantation**

### **![][image12]**

**For All Routers (R1 \- R7):**

*`enable`*  
*`configure terminal`*  
*`interface fa0/0`*  
*`ip helper-address 172.16.8.100`*  
*`no shutdown`*

### **6.2 DNS Server** {#6.2-dns-server}

- Record: `www.apex.edu.bd → 172.16.8.10`  
  `IP Address: 172.16.8.10`  
  `Subnet Mask: 255.255.255.0`  
  `Gateway: 172.16.8.1`

### **Purpose** {#purpose}

The DNS server allows students, faculty, and staff to access the university’s official website using a domain name rather than an IP address.

![][image13]

### **![][image14]**

### **6.3 Web Server** {#6.3-web-server}

* Hosted at 172.16.10.12 with university’s webpage  
  IP Address: `172.16.8.15`  
  Subnet Mask: `255.255.255.0`  
  Gateway: `172.16.8.1 (router8)`


## 

## 

## **8\. Verification** {#8.-verification}

- PCs in Campus 1–7 obtain IPs automatically via DHCP.  
- All hosts can ping each other.  
- Accessing `http://www.apex.edu.bd` resolves via DNS and opens the web server.  
- Traceroute shows correct routing across campuses.

  ### **Testing** {#testing}


### 

### 

### 

## 

## **9\. Limitations** {#9.-limitations}

1. Single point of failure at Campus 8 (central servers).

2. Physical serial links assumed; real-world implementation may use fiber or VPNs.

3. Only one DNS server—redundancy not covered.

## **10\. Future Scope** {#10.-future-scope}

- Add redundancy with **HSRP or VRRP**.

- Deploy **load-balanced DNS & DHCP**.

- Implement **firewalls and ACLs** for security.

- Integrate **VoIP, Video conferencing** services.

## **11\. Conclusion** {#11.-conclusion}

The designed network provides a scalable, centralized, and fully functional model for Apex University. Using OSPF, DHCP, and DNS ensures efficient management, seamless connectivity, and support for academic and administrative processes across eight campuses.

