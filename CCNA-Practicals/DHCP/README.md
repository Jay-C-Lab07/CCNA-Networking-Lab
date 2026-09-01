# 📡 DHCP Configuration Lab

## 🎯 Objective

Configure a **DHCP (Dynamic Host Configuration Protocol)** server in Cisco Packet Tracer so that client devices automatically receive:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

This lab demonstrates how DHCP simplifies IP address assignment and removes the need to configure each client manually.

---

## 🖧 Network Topology

```text
                  Router0
                     |
                   Switch0
          /-----------|-----------\
         /            |            \
       PC2         Laptop0       Laptop1        PC1

                     |
                  Server0
```

The DHCP server provides IP configuration automatically to all client devices connected through the switch.

---

## 🔌 Device Connections

```text
Router0 G0/0 → Switch0 Fa0/24

Server0 Fa0 → Switch0

PC2 Fa0 → Switch0
Laptop0 Fa0 → Switch0
Laptop1 Fa0 → Switch0
PC1 Fa0 → Switch0
```

All devices are part of the same LAN.

---

## 💻 Network Addressing

The network used in this lab is:

```text
192.168.10.0/24
```

Subnet Mask:

```text
255.255.255.0
```

Default Gateway:

```text
192.168.10.1
```

DNS Server:

```text
8.8.8.8
```

---

## 🌐 Router Configuration

Router0 acts as the default gateway for the LAN.

### Router0 G0/0

```text
IP Address:  192.168.10.1
Subnet Mask: 255.255.255.0
```

Configuration:

```text
enable
configure terminal

interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

end
```

Verify using:

```text
show ip interface brief
```

The interface should show:

```text
GigabitEthernet0/0   192.168.10.1   up   up
```

---

## 🖥️ DHCP Server Configuration

DHCP was configured using the graphical interface in Cisco Packet Tracer.

Navigate to:

```text
Server0 → Services → DHCP
```

Enable the DHCP service.

The DHCP pool was configured with:

```text
Pool Name:        LAN
Default Gateway:  192.168.10.1
DNS Server:       8.8.8.8
Start IP Address: 192.168.10.2
Subnet Mask:      255.255.255.0
Maximum Users:    50
```

The DHCP server automatically assigns IP addresses to connected clients.

---

## 🔢 DHCP Address Pool

The DHCP address range starts from:

```text
192.168.10.2
```

Example client addresses:

```text
192.168.10.2
192.168.10.3
192.168.10.4
192.168.10.5
192.168.10.6
...
```

The exact IP assigned to a client may vary depending on previous DHCP leases and the order in which devices request addresses.

---

## ⚠️ DHCP Pool Conflict Troubleshooting

During configuration, two DHCP pools were present:

```text
LAN
serverPool
```

The default `serverPool` contained incomplete DHCP information such as:

```text
Default Gateway: 0.0.0.0
DNS Server:      0.0.0.0
```

As a result, clients initially received IP addresses but showed:

```text
Default Gateway: 0.0.0.0
DNS Server:      0.0.0.0
```

The DHCP pool configuration was corrected so that the active pool provided:

```text
Default Gateway: 192.168.10.1
DNS Server:      8.8.8.8
```

After renewing DHCP on the client devices, the correct settings were received.

---

## 💻 Client DHCP Configuration

On every client device:

```text
Desktop → IP Configuration → DHCP
```

The devices automatically received their network configuration.

Client devices used in the lab:

```text
PC2
Laptop0
Laptop1
PC1
```

A client receives information similar to:

```text
IPv4 Address:    192.168.10.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
DNS Server:      8.8.8.8
```

---

## 🔄 DHCP Process

DHCP commonly uses the **DORA process**.

### 1. Discover

The client broadcasts a:

```text
DHCP Discover
```

message to find an available DHCP server.

### 2. Offer

The DHCP server responds with:

```text
DHCP Offer
```

and proposes an available IP address.

### 3. Request

The client sends:

```text
DHCP Request
```

to request the offered address.

### 4. Acknowledge

The server responds with:

```text
DHCP ACK
```

confirming the IP lease.

So the process is:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledge
```

or simply:

```text
DORA
```

---

## 🧠 Why DHCP Is Used

Without DHCP, every device would need to be manually configured with:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

This becomes difficult to manage in large networks.

DHCP solves this problem by assigning these settings automatically.

---

## 📦 DHCP Information Provided to Clients

A DHCP server can provide:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Information
```

This allows devices to join a network quickly with minimal manual configuration.

---

## 🌍 Real-World Use

DHCP is commonly used in:

- Offices
- Schools
- Enterprise networks
- Home Wi-Fi networks
- Data centers
- Hotels
- Public Wi-Fi networks

For example, when a laptop connects to Wi-Fi, DHCP usually provides the device with its IP address automatically.

---

## ✅ Connectivity Testing

After DHCP configuration, connectivity between client devices was tested.

From PC2, other client devices were pinged.

Example:

```text
ping 192.168.10.x
```

Successful replies confirmed that:

- DHCP assigned valid IP addresses
- All devices were in the same subnet
- Layer 3 connectivity was working correctly

---

## 🔎 Useful Verification

On a client device:

```text
ipconfig
```

This displays the assigned:

```text
IP Address
Subnet Mask
Default Gateway
```

In Packet Tracer, DHCP information can also be verified from:

```text
Desktop → IP Configuration
```

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows the complete DHCP topology with Router0, Switch0, Server0, PCs, and laptops.

### 2. DHCP Server Pool

```text
screenshots/02-dhcp-server-pool.png
```

Shows the DHCP server configuration including:

```text
Start IP Address
Subnet Mask
Default Gateway
DNS Server
```

### 3. PC2 DHCP Address

```text
screenshots/03-pc2-dhcp-address.png
```

Shows PC2 receiving its network configuration automatically through DHCP.

### 4. Laptop0 DHCP Address

```text
screenshots/04-laptop0-dhcp-address.png
```

Shows Laptop0 receiving its DHCP-assigned network configuration.

### 5. Laptop1 DHCP Address

```text
screenshots/05-laptop1-dhcp-address.png
```

Shows Laptop1 receiving its DHCP-assigned network configuration.

### 6. Connectivity Test

```text
screenshots/06-connectivity-test.png
```

Shows successful communication between DHCP clients.

---

## 📁 Files

```text
DHCP/
│
├── README.md
├── DHCP.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-dhcp-server-pool.png
    ├── 03-pc2-dhcp-address.png
    ├── 04-laptop0-dhcp-address.png
    ├── 05-laptop1-dhcp-address.png
    └── 06-connectivity-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco Router
- Cisco Switch
- Packet Tracer Server
- DHCP
- IPv4
- ICMP
- Cisco IOS CLI

---

## 🏁 Result

Successfully configured a **DHCP server in Cisco Packet Tracer**.

The DHCP server automatically assigned IP addresses and network settings to multiple PCs and laptops.

Clients successfully received:

```text
IPv4 Address
Subnet Mask
Default Gateway
DNS Server
```

A DHCP pool conflict was also identified and corrected during troubleshooting.

End-to-end communication between the client devices was successfully verified.

This lab demonstrates:

- DHCP server configuration
- Automatic IP assignment
- DHCP address pools
- Default gateway distribution
- DNS distribution
- DHCP lease behavior
- DHCP troubleshooting
- DORA process
- Client connectivity testing