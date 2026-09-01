# 🔁 RIP Routing Lab

## 🎯 Objective

Configure **RIPv2 (Routing Information Protocol Version 2)** between multiple routers so that each router can dynamically learn remote networks without manually configuring static routes.

This lab demonstrates how RIP exchanges routing information between neighboring routers and enables communication between devices located on different LANs.

---

## 🖧 Network Topology

```text
PC0 --- Switch0 --- R3 ----- R4 ----- R5 --- Switch2 --- PC2
                          |
                       Switch1
                          |
                         PC1
```

### Router Connections

```text
R3 G0/0 → Switch0 Fa0/24
R3 G0/1 → R4 G0/0

R4 G0/0 → R3 G0/1
R4 G0/1 → R5 G0/1
R4 G0/2 → Switch1 Fa0/24

R5 G0/1 → R4 G0/1
R5 G0/0 → Switch2 Fa0/24
```

### PC Connections

```text
PC0 Fa0 → Switch0 Fa0/1
PC1 Fa0 → Switch1 Fa0/1
PC2 Fa0 → Switch2 Fa0/1
```

---

## 💻 IP Addressing

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC0 | Fa0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| R3 | G0/0 | 192.168.1.1 | 255.255.255.0 | — |
| R3 | G0/1 | 10.0.0.1 | 255.255.255.252 | — |
| R4 | G0/0 | 10.0.0.2 | 255.255.255.252 | — |
| R4 | G0/1 | 10.0.0.5 | 255.255.255.252 | — |
| R4 | G0/2 | 192.168.2.1 | 255.255.255.0 | — |
| PC1 | Fa0 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |
| R5 | G0/1 | 10.0.0.6 | 255.255.255.252 | — |
| R5 | G0/0 | 192.168.3.1 | 255.255.255.0 | — |
| PC2 | Fa0 | 192.168.3.10 | 255.255.255.0 | 192.168.3.1 |

---

## 🌐 Point-to-Point Networks

### R3 ↔ R4

```text
Network:   10.0.0.0/30
R3:        10.0.0.1
R4:        10.0.0.2
Broadcast: 10.0.0.3
```

### R4 ↔ R5

```text
Network:   10.0.0.4/30
R4:        10.0.0.5
R5:        10.0.0.6
Broadcast: 10.0.0.7
```

A `/30` subnet provides:

- 1 Network Address
- 2 Usable Host Addresses
- 1 Broadcast Address

This makes `/30` suitable for point-to-point router links.

---

## ⚙️ Router Interface Configuration

### R3

```text
enable
configure terminal

interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface g0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit

end
```

### R4

```text
enable
configure terminal

interface g0/0
ip address 10.0.0.2 255.255.255.252
no shutdown
exit

interface g0/1
ip address 10.0.0.5 255.255.255.252
no shutdown
exit

interface g0/2
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

end
```

### R5

```text
enable
configure terminal

interface g0/1
ip address 10.0.0.6 255.255.255.252
no shutdown
exit

interface g0/0
ip address 192.168.3.1 255.255.255.0
no shutdown
exit

end
```

---

## 🔁 RIP Version 2 Configuration

### R3

```text
enable
configure terminal

router rip
version 2
network 10.0.0.0
network 192.168.1.0
no auto-summary

end
```

### R4

```text
enable
configure terminal

router rip
version 2
network 10.0.0.0
network 192.168.2.0
no auto-summary

end
```

### R5

```text
enable
configure terminal

router rip
version 2
network 10.0.0.0
network 192.168.3.0
no auto-summary

end
```

---

## 🔎 RIP Verification

Use:

```text
show ip protocols
```

The output should confirm:

```text
Routing Protocol is "rip"
Default version control: send version 2, receive 2
Automatic network summarization is not in effect
```

### R3 Advertised Networks

```text
10.0.0.0
192.168.1.0
```

### R4 Advertised Networks

```text
10.0.0.0
192.168.2.0
```

### R5 Advertised Networks

```text
10.0.0.0
192.168.3.0
```

---

## 🛣️ Routing Table Verification

Use:

```text
show ip route
```

Routes learned through RIP are marked with:

```text
R
```

For example, R3 should learn remote LANs such as:

```text
R 192.168.2.0/24
R 192.168.3.0/24
```

This confirms that routing information is being exchanged dynamically.

---

## ✅ Connectivity Testing

From PC0, test connectivity with PC1:

```text
ping 192.168.2.10
```

Then test PC2:

```text
ping 192.168.3.10
```

Successful replies confirm end-to-end connectivity between all LANs.

---

## 🧠 How RIP Works

RIP is a **Distance Vector Dynamic Routing Protocol**.

Routers running RIP periodically exchange routing information with neighboring routers instead of requiring every remote route to be configured manually.

RIP uses **hop count** as its routing metric.

The path with the lowest number of router hops is preferred.

Important RIP characteristics:

```text
Maximum Hop Count:       15
Unreachable Network:     16 hops
Update Interval:         30 seconds
Administrative Distance: 120
Metric:                  Hop Count
```

---

## 🚀 Why RIPv2?

RIPv2 improves on RIPv1 by supporting:

- Classless routing
- CIDR
- VLSM
- Subnet mask information in routing updates
- Multicast routing updates

RIPv2 sends updates to:

```text
224.0.0.9
```

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows the complete three-router RIP topology.

### 2. R3 RIP Protocol

```text
screenshots/02-r3-rip-protocol.png
```

Shows the RIP configuration and advertised networks on R3.

### 3. R4 RIP Protocol

```text
screenshots/03-r4-rip-protocol.png
```

Shows the RIP configuration and advertised networks on R4.

### 4. R5 RIP Protocol

```text
screenshots/04-r5-rip-protocol.png
```

Shows the RIP configuration and advertised networks on R5.

### 5. RIP Routing Table

```text
screenshots/05-rip-routing-table.png
```

Shows dynamically learned routes marked with `R`.

### 6. Connectivity Test

```text
screenshots/06-connectivity-test.png
```

Shows successful communication between devices on remote LANs.

---

## 📁 Files

```text
RIP/
│
├── README.md
├── RIP.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-r3-rip-protocol.png
    ├── 03-r4-rip-protocol.png
    ├── 04-r5-rip-protocol.png
    ├── 05-rip-routing-table.png
    └── 06-connectivity-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco Switches
- RIPv2
- IPv4
- CIDR / Subnetting
- Cisco IOS CLI

---

## 🏁 Result

Successfully configured **RIPv2 across three routers**, allowing routers to dynamically exchange routing information.

All three LANs were able to communicate successfully without manually configuring individual static routes.

This lab demonstrates:

- Dynamic routing
- RIPv2 configuration
- RIP route exchange
- `/30` point-to-point subnetting
- Dynamic route learning
- Routing table verification
- End-to-end connectivity testing