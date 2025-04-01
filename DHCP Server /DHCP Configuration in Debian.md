# DHCP (Dynamic Host Configuration Protocol) Configuration Guide

## 📌  DHCP
DHCP (Dynamic Host Configuration Protocol) is a network protocol that automatically assigns IP addresses to clients, reducing the need for manual configuration. Modern routers have built-in DHCP servers that handle this process.

---

## 🔄 DHCP Process (DORA)
DHCP follows the **DORA (Discover, Offer, Request, Acknowledge)** process:

📌 **Discover** → The client broadcasts a request to find a DHCP server.  
📌 **Offer** → The DHCP server responds with an available IP offer.  
📌 **Request** → The client requests the offered IP address.  
📌 **Acknowledge** → The server confirms the lease and assigns the IP to the client.

---

## ⚙️ Installing DHCP Server
### 🏷️ On Debian:
```bash
apt install isc-dhcp-server
```
### ✅ Verify Installation:
```bash
dpkg -l | grep dhcp
```
### 📂 List DHCP Files:
```bash
dpkg -L isc-dhcp-server
```
### 📍 Locate Configuration Files:
```bash
dpkg -L isc-dhcp-server | grep /etc/
```

---

## 🛠️ Configuring DHCP Server
### 📌 Main Configuration File
📍 **Debian:** `/etc/dhcp/dhcpd.conf`

### 📝 Editing the Configuration File
```bash
vim /etc/dhcp/dhcpd.conf
```
#### ✅ Sample DHCP Configuration:
```bash
# Define authoritative DHCP server
authoritative;

# Define subnet and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.30 192.168.1.35;
    range 192.168.1.36 192.168.1.40;
    
    # Assign default gateway
    option routers 192.168.1.1;
    
    # Assign DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    
    # Set broadcast address
    option broadcast-address 192.168.1.255;
    
    # Define lease times
    default-lease-time 600;
    max-lease-time 7200;
}
```

### ⚠️ Warnings & Best Practices:
✅ **Always use `;` at the end of every statement.** Missing `;` will cause syntax errors.  
✅ **Ensure `{}` properly enclose subnet configurations.** Unclosed `{}` can break the configuration.  
✅ **Use valid IP addresses.** Incorrect values prevent DHCP from assigning IPs properly.

---

## 🔧 Assigning Network Interface
```bash
vim /etc/default/isc-dhcp-server
```
Specify the network interface:
```bash
INTERFACESv4="enp0s3"
```

### 🔍 Checking Configuration for Errors
```bash
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
#### 🛑 Common Errors:
🚨 **Syntax errors** → Often due to missing `;` at the end of statements.  
🚨 **Invalid subnet declarations** → Ensure subnet masks and IP ranges are correct.  
🚨 **Incorrect interface configuration** → The DHCP server must bind to the correct network interface.

---

## ▶️ Starting & Enabling DHCP Server
```bash
systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server
```

---

## 📌 Additional DHCP Management Commands
### ✅ Check & Start DHCP Services:
🔹 **Check Service Status:**
```bash
systemctl status isc-dhcp-server
```
🔹 **Start Service:**
```bash
systemctl start isc-dhcp-server
```
🔹 **Restart Service:**
```bash
systemctl restart isc-dhcp-server
```
🔹 **Enable Service to Start at Boot:**
```bash
systemctl enable isc-dhcp-server
```

### 🔍 Checking Open Ports:
🔹 **View Open Ports:**
```bash
netstat -nltup
```
🔹 **Check if DHCP Ports are Open:**
```bash
netstat -nltup | grep dhcp
```

### 🔥 Updating Firewall Rules:
🔹 **Allow DHCP Ports:**
```bash
ufw allow 67/udp
```
🔹 **Reload Firewall:**
```bash
ufw reload
```
🔹 **Verify Firewall Rules:**
```bash
ufw status
```

### 📜 Checking DHCP Leases:
🔹 **View Active DHCP Leases:**
```bash
cat /var/lib/dhcp/dhcpd.leases


