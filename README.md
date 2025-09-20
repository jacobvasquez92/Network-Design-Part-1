# Network Design: Part 1
This report outlines the goal of ficticious organization, Corporation Tech, to provide on-site and remote support, including services such as malware removal, data recovery, network issue resolution, and hardware/software installation. 
  
  <img width="998" height="979" alt="image" src="https://github.com/user-attachments/assets/21f307c4-75cf-4a6a-9300-5252a9158b8c" />
  
Following a review of the company's business goals, a reevaluation of the current network design is necessary to identify enhancements in overall logical network design, redundancy and security measures, and communication protocol upgrades.

# **Current Network Architecture Includes the Following Devices:**
- Web Server: Linux/Apache server that is accessible by the public
- Two Application Servers: Microsoft Windows Server
- Two Database Servers: Microsoft Windows Server
- Two File and Print Servers: Microsoft Windows Server
- 50 Workstations: Microsoft Windows
- Firewall: Single border device
  
# **Recommended Network Enhancements:**
- Next-Generation Firewall: Three-legged firewall creating three dedicated zones: Gateway Interface, a DMZ Interface, and an internal LAN Interface.
- Gateway Interface: Connects to external public network (Internet); this is the Wide Area Network (WAN) connection
- DMZ Interface: Connects to a switch dedicated to the DMZ.  The Linux/Apache server will reside here.
- Internal Interface: Connects to core switches that serve all internal servers and workstations.
- Logically separation of departments into distinct VLANs
- Begin transition to IPv6

## **Proposed Network Topologies**
<img width="951" height="517" alt="image" src="https://github.com/user-attachments/assets/9ee875f8-73f9-422f-aba4-88560b1aac82" />

---
<img width="949" height="459" alt="image" src="https://github.com/user-attachments/assets/aede15ec-1c48-407e-9e54-773615f9610c" />

## **High-Level 24/7 Communication Availability and Redundancy Plan**
### **Firewall**  
The existing single border firewall represents a single point of failure and is a major vulnerability.  Any failure will remove all network protection and cease all internet-based business functions.  The recommendation is to move towards a Next-Generation Firewall (NGFW) High Availability (HA) firewall cluster to eliminate the single point of failure. A redundant firewall will seamlessly take over providing continuous uninterrupted protection if a firewall goes down.    

**Benefits are as follows:**
- Cost effective redundancy solution
- NGFW add more advanced security features than traditional firewalls
- The active unit is processing network traffic while the passive unit is in standby mode ready to take over if the active unit fails

### **Switches**  
All network switches will, at minimum, comprise a pair of Layer 3 Switches, eliminating single points of failure.  Utilizing Virtual Router Redundancy Protocol (VRRP), paired switches will share a single virtual IP address and MAC address.  For client devices on the LAN, this will serve as the default gateway.  The VRRP cluster will designate a Master device that forwards all traffic, while the other(s) back up the Master, ready to take over if it fails.  Spanning Tree Protocol is utilized to prevent network loops and provide alternate paths if a link or switch fails.
  
**Benefits are as follows:**
- Eliminates single point of failure.  Without a backup, if a switch fails then all devices lose connectivity to the internet or other VLANs.
- In the event of a failover, clients on the LAN do not need to change configuration settings or have awareness of a change.
- VRRP is a standard protocol unlike Hot Standby Router Protocol (HSRP), which is a Cisco proprietary protocol.  There is flexibility in the open standard of VRRP.
- The VRRP group can be set to Active/Active to accommodate higher traffic environments.

### **Servers**  
Load balancing strategies can ensure optimal availability and redundancy based on server demand.  Load balancing will distribute traffic across multiple servers, maximizing throughput and minimizing latency.  Each server cluster should be evaluated independently based upon application requirements, traffic patterns, and scalability needs.  

**Benefits are as follows:**
- Minimize latency
- Maximize throughput
- Failover and redundancy strategies to minimize downtime
- Optimize resource utilization
- DDoS mitigation by distributing traffic across multiple servers
- Servers can be taken offline for updates without disrupting service

### **Internet Service Provoder (ISP)**  
Carrier diversity is a key component to ensure availability.  Establishing service with at least two different ISPs protects the network if the primary service provider experiences an outage.
  
### **Power Supply**
All critical network hardware should be connected to different electrical circuits and be backed up by Uninterruptible Power Supply (UPS).  Continuous operation during power outages is further ensured by backup generators

## **Recommended Transition to IPv6**
A transition to IPv6 from IPv4 is recommended to increase network efficiency, security, long term network scalability, and to remain on par with an understood 20–30-year global transition. Since the world IPv6 launch in 2012, peak IPv6 traffic has increased from ~1Gbps to 41Tbps in 2022.  Industry transition to IPv6 is inevitable as IPv4 addresses are reaching exhaustion.  The IPv6 address pool is nearly 85 trillion times larger than the IPv4 address pool which essentially eliminates the need for subnetting, there is increased efficiency in routing capabilities, better quality of service (QoS), native information security framework (IPSec), and plug-and-play configuration with or without DHCP (Stewert & Kinsey, 2021).   

A long-term transition is recommended to mitigate service disruption, hardware and software IPv6 incompatibility, the need for extensive employed training, and mobilization and implementation costs. Transition can be done department by department utilizing pilot programs that target non-critical areas protecting core business operations.  Several strategies can be put in place to ease into the transition phase. 

## **The strategies are as follows:**
1.	**Dual-Stack Implementation:** This will be the core strategy- run both protocols simultaneously on all devices allowing for a graceful transition.  Devices can communicate with either protocol but IPv6 will be prioritized when possible.  Dual-Stack will add significant overhead to network architecture.
2.	**Tunneling:** Two IPv6 hosts create an IPv6 tunnel between the hosts through an IPv4 network; several methods exist to facilitate tunneling.  Tunneling can also add significant overhead.
3.	**Translation:** Like network address translation (NAT), network address translation 64 (NAT 64) uses translation techniques that allow IPv6 enabled devices to communicate with IPv4 enabled devices.


  
# **References**
Akamai. (2022, June 6). *10 years since World IPv6 launch.* Akamai Blog. https://www.akamai.com/blog/trends/10-years-since-world-ipv6-launch 

Axigen. (2020, June 26). *How to install a demilitarized zone for your servers.* Axigen. https://www.axigen.com/articles/how-to-install-a-demilitarized-zone-for-your-servers_24.html 

Catchpoint. (n.d.). *Benefits of IPv6.* Catchpoint. https://www.catchpoint.com/benefits-of-ipv6/ipv6-tunnelling  

Check Point Software Technologies Ltd. (n.d.). *What is firewall? High availability (HA) firewall.* Check Point. https://www.checkpoint.com/cyber-hub/network-security/what-is-firewall/high-availability-ha-firewall/  

Cloudflare. (n.d.). *Types of load balancing algorithms.* Cloudflare Learning. https://www.cloudflare.com/learning/performance/types-of-load-balancing-algorithms/ 

Firewall.cx. (n.d.). *Firewall topologies & DMZ zone.* Firewall.cx. https://www.firewall.cx/networking/network-fundamentals/firewall-topologies-dmz-zone.html 

IR. (n.d.). *Navigating the IPv4-IPv6 transition: Essential strategies for success.* IR. https://www.ir.com/guides/navigating-the-ipv4-ipv6-transition-essential-strategies-for-success  

Nygren, E. (2022, June 6). *10 years since World IPv6 launch.* Akamai Blog. https://www.akamai.com/blog/trends/10-years-since-world-ipv6-launch 

Stewart, J. M., & Kinsey, D. (2025). *Network Security, Firewalls, and VPNs.* Jones & Bartlett Learning.  

U.S. Department of Health and Human Services. (2021, August). *HHS policy for the transition to Internet protocol version 6 (IPv6).* HHS.gov. https://www.hhs.gov/web/governance/digital-strategy/it-policy-archive/hhs-policy-transition-internet-protocol-version-6-ipv6.html  

Universitas Sriwijaya. (n.d.). *IPv4 issues.* CCNA Ilkom Unsri.  https://ccna.ilkom.unsri.ac.id/5/course/module7/7.2.1.2/7.2.1.2.html

Wheeler, R. (2024, March 26). *Load balancing best practices.* Kemp Technologies. https://kemptechnologies.com/blog/load-balancing-best-practices  
