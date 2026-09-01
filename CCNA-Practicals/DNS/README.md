# 🌐 DNS Configuration Lab

## 🎯 Objective

Configure a **DNS (Domain Name System)** server in Cisco Packet Tracer so that client devices can access a server using a domain name instead of remembering its IP address.

This lab demonstrates:

- DNS server configuration
- DHCP-based client addressing
- DNS record creation
- Name-to-IP resolution
- Connectivity testing using a domain name

---

## 🖧 Network Topology

```text
PC0 ------\
           \
Laptop1 --- Switch0 -------- Server0
           /
PC1 ------/
```

All devices are connected to the same LAN through Switch0.

---

## 🔌 Device Connections

```text
PC0 Fa0      → Switch0 Fa0/1
Laptop1 Fa0  → Switch0 Fa0/2
PC1 Fa0      → Switch0 Fa0/3
Server0 Fa0  → Switch0 Fa0/24
```

---

## 💻 Network Addressing

The network used in this lab is:

```text
192.168.20.0/24
```

Subnet Mask:

```text
255.255.255.0
```

DNS Server:

```text
192.168.20.2
```

Since all devices are on the same subnet and no router is required for this lab, a default gateway is not necessary.

---

## 🖥️ Server Configuration

Server0 is assigned a static IP address.

```text
IP Address:  192.168.20.2
Subnet Mask: 255.255.255.0
```

The DNS server address must remain static because client devices need a fixed address to contact the DNS service.

---

## 📡 DHCP Configuration

DHCP was used to automatically assign IP addresses to the client devices.

Navigate to:

```text
Server0 → Services → DHCP
```

The DHCP pool was configured with:

```text
Pool Name:        LAN
Start IP Address: 192.168.20.10
Subnet Mask:      255.255.255.0
DNS Server:       192.168.20.2
Default Gateway:  0.0.0.0
```

The client devices were configured to obtain their IP addresses automatically.

---

## 💻 DHCP Clients

The client devices used in this lab are:

```text
PC0
Laptop1
PC1
```

On each device:

```text
Desktop → IP Configuration → DHCP
```

Each client automatically received:

```text
IPv4 Address:    192.168.20.x
Subnet Mask:     255.255.255.0
DNS Server:      192.168.20.2
```

---

## 🌐 DNS Server Configuration

Navigate to:

```text
Server0 → Services → DNS
```

Enable:

```text
DNS Service: On
```

A DNS A Record was created:

```text
Name:    www.lab.local
Type:    A Record
Address: 192.168.20.2
```

This means:

```text
www.lab.local
      ↓
192.168.20.2
```

The DNS server translates the domain name into the corresponding IPv4 address.

---

## 🧠 What DNS Does

DNS stands for:

```text
Domain Name System
```

DNS converts human-readable names into IP addresses.

For example:

```text
www.lab.local
```

is easier to remember than:

```text
192.168.20.2
```

When a client tries to access:

```text
www.lab.local
```

the client sends a DNS query to:

```text
192.168.20.2
```

The DNS server checks its records and responds with:

```text
192.168.20.2
```

The client can then communicate with that IP address.

---

## 🔎 DNS Resolution Process

The basic DNS process in this lab is:

```text
PC0
 |
 | DNS Query: www.lab.local?
 |
 v
Server0 DNS
 |
 | Response: 192.168.20.2
 |
 v
PC0
 |
 | Ping 192.168.20.2
 |
 v
Server0
```

So the client does not need to know the server IP manually.

---

## ✅ DNS Testing

From PC0:

```text
ping www.lab.local
```

The domain name was successfully resolved to:

```text
192.168.20.2
```

and the server responded successfully.

This confirms that:

- The client received the correct DNS server address
- DNS service was enabled
- The DNS record was configured correctly
- Name resolution was working
- Network connectivity was successful

---

## 🧪 Additional Verification

Clients can also verify their network settings from:

```text
Desktop → IP Configuration
```

or:

```text
ipconfig
```

The DNS field should show:

```text
192.168.20.2
```

If the client does not have the correct DNS server address, domain-name resolution will fail even if direct IP connectivity works.

---

## 🛠️ Troubleshooting Performed

Initially, DNS name resolution failed with:

```text
Ping request could not find host www.lab.local.
```

The issue occurred because the client devices had not yet been configured to obtain their settings through DHCP.

After selecting:

```text
Desktop → IP Configuration → DHCP
```

the clients received the correct DNS server address:

```text
192.168.20.2
```

After that, the command:

```text
ping www.lab.local
```

successfully resolved the domain name and reached the server.

---

## 📚 Common DNS Record Types

### A Record

Maps a hostname to an IPv4 address.

Example:

```text
www.lab.local → 192.168.20.2
```

### AAAA Record

Maps a hostname to an IPv6 address.

### CNAME Record

Creates an alias for another hostname.

### MX Record

Specifies a mail server.

### PTR Record

Used for reverse DNS lookup.

---

## ⚡ Important DNS Facts

```text
Full Form:      Domain Name System
Default Port:   53
Transport:      UDP and TCP
Purpose:        Name-to-IP Resolution
Common Record:  A Record
```

DNS normally uses UDP port 53 for standard queries and TCP port 53 for larger responses or specific operations.

---

## 🌍 Real-World Use

DNS is used whenever users access services using names instead of IP addresses.

Examples:

```text
google.com
github.com
amazon.com
```

Without DNS, users would need to remember the IP address of every website or service.

DNS makes network resources easier to access and manage.

---

## 🔁 DHCP and DNS Together

DHCP and DNS often work together.

DHCP automatically provides clients with:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server Address
```

DNS then allows those clients to resolve names into IP addresses.

In this lab:

```text
DHCP
  ↓
Gives client DNS = 192.168.20.2
  ↓
Client asks DNS for www.lab.local
  ↓
DNS responds with 192.168.20.2
```

---

## 🔎 Useful Commands

Check client IP configuration:

```text
ipconfig
```

Test direct IP connectivity:

```text
ping 192.168.20.2
```

Test DNS name resolution:

```text
ping www.lab.local
```

If supported by the Packet Tracer version:

```text
nslookup www.lab.local
```

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows PC0, Laptop1, PC1, Switch0, and Server0.

### 2. DHCP Pool

```text
screenshots/02-dhcp-pool.png
```

Shows the DHCP configuration providing the DNS server address to client devices.

### 3. DNS Record

```text
screenshots/03-dns-record.png
```

Shows:

```text
www.lab.local → 192.168.20.2
```

as an A Record with DNS service enabled.

### 4. Client DHCP Settings

```text
screenshots/04-client-dhcp-settings.png
```

Shows a client receiving its IP address and DNS server automatically through DHCP.

### 5. DNS Ping Test

```text
screenshots/05-dns-ping-test.png
```

Shows successful:

```text
ping www.lab.local
```

confirming DNS name resolution.

---

## 📁 Files

```text
DNS/
│
├── README.md
├── DNS.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-dhcp-pool.png
    ├── 03-dns-record.png
    ├── 04-client-dhcp-settings.png
    └── 05-dns-ping-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco Switch
- Packet Tracer Server
- DHCP
- DNS
- IPv4
- ICMP

---

## 🏁 Result

Successfully configured a **DNS server in Cisco Packet Tracer**.

The DNS server resolved:

```text
www.lab.local
```

to:

```text
192.168.20.2
```

Client devices received their network settings automatically using DHCP, including the DNS server address.

The domain name was successfully resolved and verified using a ping test.

This lab demonstrates:

- DNS server configuration
- A Record creation
- DHCP and DNS integration
- Automatic client IP assignment
- Name-to-IP resolution
- DNS troubleshooting
- End-to-end connectivity testing