# 🌍 NAT & PAT Configuration Lab

## 🎯 Objective

Configure **NAT (Network Address Translation)** and **PAT (Port Address Translation)** in Cisco Packet Tracer so that multiple private network devices can communicate with an external network using a shared public-facing IP address.

This lab demonstrates:

- Inside and outside NAT interfaces
- Private-to-public IP translation
- PAT overload
- ACL-based NAT matching
- Default routing
- NAT translation verification
- End-to-end connectivity

---

## 🖧 Network Topology

```text
PC0 --------\
             \
Laptop0 ----- Switch0 ----- Router0 ----- Router1 ----- Server0
             /
PC1 --------/
```

---

## 🔌 Device Connections

```text
PC0 Fa0        → Switch0 Fa0/1
Laptop0 Fa0    → Switch0 Fa0/2
PC1 Fa0        → Switch0 Fa0/3
Switch0 Fa0/24 → Router0 G0/0

Router0 G0/1   → Router1 G0/0

Router1 G0/1   → Server0 Fa0
```

---

## 💻 IP Addressing

### Inside Private LAN

```text
Network: 192.168.10.0/24
```

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| Laptop0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC1 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| Router0 G0/0 | 192.168.10.1 | 255.255.255.0 | — |

### Router0 ↔ Router1 Link

```text
Router0 G0/1 = 203.0.113.1
Router1 G0/0 = 203.0.113.2
```

### External Network

```text
Network: 198.51.100.0/24
```

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| Router1 G0/1 | 198.51.100.1 | 255.255.255.0 | — |
| Server0 | 198.51.100.10 | 255.255.255.0 | 198.51.100.1 |

---

## ⚙️ Router0 Interface Configuration

```text
enable
configure terminal

interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface g0/1
ip address 203.0.113.1 255.255.255.0
no shutdown
exit

end
```

---

## ⚙️ Router1 Interface Configuration

```text
enable
configure terminal

interface g0/0
ip address 203.0.113.2 255.255.255.0
no shutdown
exit

interface g0/1
ip address 198.51.100.1 255.255.255.0
no shutdown
exit

end
```

---

## 🔁 NAT Inside and Outside Configuration

On Router0:

```text
enable
configure terminal

interface g0/0
ip nat inside
exit

interface g0/1
ip nat outside
exit
```

This defines:

```text
G0/0 → Inside / Private Network
G0/1 → Outside / External Network
```

---

## 📜 ACL Configuration

The private network must be identified before NAT can translate it.

```text
access-list 1 permit 192.168.10.0 0.0.0.255
```

This matches all devices in:

```text
192.168.10.0/24
```

---

## 🔀 PAT Overload Configuration

PAT was enabled using Router0's outside interface:

```text
ip nat inside source list 1 interface GigabitEthernet0/1 overload
```

The keyword:

```text
overload
```

enables PAT.

This allows multiple private devices to share the same outside IP address.

---

## 🛣️ Default Route

Router0 needs to know where to send traffic destined for unknown external networks.

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

This sends external traffic toward Router1.

---

## 🧠 NAT vs PAT

### NAT

NAT translates one IP address into another.

Example:

```text
Private IP
192.168.10.10

        ↓ NAT

Public / Global IP
203.0.113.1
```

### PAT

PAT allows multiple private devices to share a single public IP address.

Example:

```text
192.168.10.10 \
192.168.10.11  ----> 203.0.113.1
192.168.10.12 /
```

PAT separates different sessions using port numbers or protocol identifiers.

---

## 🔎 NAT Terminology

### Inside Local

The private IP address of the internal device.

Example:

```text
192.168.10.10
```

### Inside Global

The translated address representing the inside device externally.

Example:

```text
203.0.113.1
```

### Outside Local

The address of the external destination as seen from the inside network.

Example:

```text
198.51.100.10
```

### Outside Global

The actual globally reachable address of the external device.

Example:

```text
198.51.100.10
```

---

## 📊 NAT Translation Verification

Use:

```text
show ip nat translations
```

Example output:

```text
Pro   Inside global     Inside local       Outside local       Outside global

icmp  203.0.113.1       192.168.10.10      198.51.100.10       198.51.100.10
```

This confirms:

```text
192.168.10.10
      ↓
203.0.113.1
```

was translated successfully.

---

## 🔀 Multiple Client PAT Verification

Different private clients can use the same inside-global address.

Example:

```text
192.168.10.10
192.168.10.11
192.168.10.12
        ↓
203.0.113.1
```

PAT keeps each session separate using different port numbers or ICMP identifiers.

This allows many private hosts to communicate externally using one translated address.

---

## ✅ Connectivity Testing

From an inside client:

```text
ping 198.51.100.10
```

Successful replies confirm that the private device can reach the external server through Router0 and Router1.

---

## 🔎 Useful Verification Commands

Check interfaces:

```text
show ip interface brief
```

Check NAT translations:

```text
show ip nat translations
```

Check NAT statistics:

```text
show ip nat statistics
```

Check routing table:

```text
show ip route
```

Check NAT configuration:

```text
show running-config
```

---

## ⚡ Important NAT/PAT Facts

```text
NAT Full Form: Network Address Translation
PAT Full Form: Port Address Translation

NAT Purpose:
Translate IP addresses between networks

PAT Purpose:
Allow multiple private devices to share one global IP

PAT Keyword:
overload
```

---

## 🌍 Real-World Use

NAT and PAT are commonly used in:

- Home networks
- Enterprise networks
- Internet gateways
- Branch offices
- Firewalls
- Edge routers

A typical home router uses PAT so that phones, laptops, TVs, and other devices can all access the Internet using a single public IP address.

---

## 🔐 Why Private IPs Need NAT

Private IPv4 ranges include:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

These addresses are not directly routable on the public Internet.

NAT allows private devices to communicate with external networks by translating their private addresses.

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows the complete NAT/PAT topology.

### 2. Router0 NAT Configuration

```text
screenshots/02-router0-nat-config.png
```

Shows:

```text
ip nat inside
ip nat outside
access-list 1
ip nat inside source list 1 interface GigabitEthernet0/1 overload
default route
```

### 3. NAT Translation - PC0

```text
screenshots/03-nat-translations-pc0.png
```

Shows:

```text
Inside Local:  192.168.10.10
Inside Global: 203.0.113.1
```

### 4. Multiple Client PAT Translation

```text
screenshots/04-nat-translations-multiple-clients.png
```

Shows private clients being translated through the same global IP address.

### 5. Connectivity Test

```text
screenshots/05-connectivity-test.png
```

Shows successful ping communication from the private LAN to:

```text
198.51.100.10
```

---

## 📁 Files

```text
NAT-PAT/
│
├── README.md
├── NAT-PAT.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-router0-nat-config.png
    ├── 03-nat-translations-pc0.png
    ├── 04-nat-translations-multiple-clients.png
    └── 05-connectivity-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco Switch
- Packet Tracer Server
- NAT
- PAT
- ACL
- IPv4
- Static Routing
- ICMP
- Cisco IOS CLI

---

## 🏁 Result

Successfully configured **NAT and PAT in Cisco Packet Tracer**.

The private LAN devices were able to communicate with an external server.

PAT translated multiple private IP addresses through the same Router0 outside address.

The command:

```text
show ip nat translations
```

confirmed successful private-to-global address translations.

This lab demonstrates:

- NAT inside/outside configuration
- ACL-based NAT matching
- PAT overload
- Private-to-global address translation
- Multiple-client PAT
- Default routing
- NAT verification
- External connectivity testing