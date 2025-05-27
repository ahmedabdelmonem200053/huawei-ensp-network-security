# **Huawei Firewall Network Security Project (ENSP)**  

This project implements a **secure enterprise network** using **Huawei firewalls** on **ENSP** (Enterprise Network Simulation Platform). The goal was to design a robust security architecture with **high availability, VPN connectivity, NAT, user authentication, and access control policies**.  

---

## **Project Objectives**  
1. **High Availability (Hot Standby)** – Configure **active/standby firewalls** for failover.  
2. **Zone-Based Security** – Define **Trust, DMZ, and Untrust zones** with controlled access.  
3. **IPSec VPN (Site-to-Site)** – Securely connect the **main office (Trust)** and a **remote branch**.  
4. **NAT (Network Address Translation)**  
   - **Source NAT**: Allow internal users (Trust) to access the internet (Untrust).  
   - **NAT Server**: Permit external users (Untrust) to access the **FTP server in DMZ**.  
5. **User Authentication** – Restrict web access to authenticated employees.  
6. **Security Policies** – Enforce strict traffic filtering between zones.  

---

## **Network Topology**  
The network consists of:  

### **1. Trust Zone (Internal Network)**  
- Contains employee workstations (`10.1.1.0/24`).  
- Connected to **FW1 (Active)** and **FW2 (Standby)**.  

### **2. DMZ Zone (Servers)**  
- Hosts an **FTP server** with a **public IP** for external access.  
- Only **FTP traffic** is allowed from the **Untrust zone**.  

### **3. Untrust Zone (Internet)**  
- Simulates the public internet.  
- External users can access the **DMZ FTP server** but nothing else.  

### **4. Another Branch (Remote Site)**  
- Connected via **IPSec VPN** (`10.1.4.0/24`).  
- Uses **FW3** as its firewall.  

### **5. Cloud Devices (Management & Testing)**  
- **Cloud1-3**: Used for **web-based firewall configuration**.  
- **Cloud5**: Acts as a **Trust Zone device** for testing.  

📌 **Topology Diagram**: ![`./Screenshots/topology.png`](./Screenshots/topology.png)  

---

## **Implementation Details**  

### **1. High Availability (Hot Standby - VRRP)**  
- **FW1** is the **active** firewall.  
- **FW2** is the **standby** (takes over if FW1 fails).  
- **VRRP** ensures automatic failover.  

📌 **Configurations & Status**:  
- Active Firewall (FW1): ![`./Screenshots/active_firewall.png`](./Screenshots/active_firewall.png)  
- Standby Firewall (FW2): ![`./Screenshots/standby_firewall.png`](./Screenshots/standby_firewall.png)  

---

### **2. IPSec VPN (Site-to-Site)**  
- Connects **Trust Zone (`10.1.1.0/24`)** ↔ **Branch (`10.1.4.0/24`)**.  
- Uses **IKE & ESP** for encryption.  
- **Static routes** ensure VPN traffic flows correctly.  

📌 **Verification**:  
- IPSec Configuration: ![`./Screenshots/ipsec.png`](./Screenshots/ipsec.png)  
- Successful VPN Negotiation: ![`./Screenshots/ipsec_monitor.png`](./Screenshots/ipsec_monitor.png)  
- Traffic Test (Ping from Trust → Branch): ![`./Screenshots/vpn_traffic.png`](./Screenshots/vpn_traffic.png)  

---

### **3. NAT (Network Address Translation)**  
#### **A. Source NAT (Trust → Untrust)**  
- Allows internal users (`10.1.1.0/24`) to access the internet.  
- Translates private IPs to a **public IP pool**.  

#### **B. NAT Server (Untrust → DMZ)**  
- Maps a **public IP** to the **DMZ FTP server**.  
- Only **FTP traffic** is permitted.  
 ![`./Screenshots/untrust_access_server.png`](./Screenshots/untrust_access_server.png)

📌 **NAT Policies**: ![`./Screenshots/nat_policies.png`](./Screenshots/nat_policies.png)  

---

### **4. User Management (Web Authentication)**  
- Employees must **authenticate** before accessing external HTTP services.  
- A user group (**"employee"**) was created.  
- **User01** was added for testing.  

📌 **Verification Steps**:  
1. **User Creation**: ![`./Screenshots/user_management.png`](./Screenshots/user_management.png)  
2. **Login Page (Redirect)**: ![`./Screenshots/user_login.png`](./Screenshots/user_login.png)  
3. **Access After Login**:
 ![`./Screenshots/user_access_after_login.png`](./Screenshots/user_access_after_login.png)  
4. **Online Users Monitoring**: ![`./Screenshots/user_is_online.png`](./Screenshots/user_is_online.png)  

---

### **5. Security Policies (Traffic Filtering)**  
| Policy | Source Zone | Destination Zone | Service | Action | Purpose |
|--------|-------------|------------------|---------|--------|---------|
| 1-4    | Trust       | Branch (VPN)     | IPSec   | Allow  | VPN Traffic |
| 5      | Trust       | Untrust          | HTTP    | Allow (Auth) | Employee Web Access |
| 6      | Untrust     | DMZ              | FTP     | Allow  | External FTP Access |
| 7      | Any         | Firewall         | Auth    | Allow  | User Authentication |

📌 **Full Policy List**: ![`./Screenshots/policies_in_firewall_1.png`](./Screenshots/policies_in_firewall_1.png)  

---

## **Conclusion**  
This project successfully implemented:  
✅ **High Availability (Active/Standby Firewalls)**  
✅ **Secure VPN Connectivity (IPSec)**  
✅ **Controlled NAT (Internal & External Access)**  
✅ **User Authentication (Web Access Control)**  
✅ **Strict Security Policies (Zone-Based Filtering)**  

The firewall ensures **secure, reliable, and monitored** network access while preventing unauthorized traffic.  

📂 **All screenshots** are available in the [`./Screenshots/`](./Screenshots/) directory.  
📂 **All configurations and files** are available in the [`project`](./Configs_and%20_project/) directory. 
Configs_and _project
