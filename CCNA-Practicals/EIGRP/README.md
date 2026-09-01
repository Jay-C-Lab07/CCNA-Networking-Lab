# ⚡ EIGRP Routing Lab

## 🎯 Objective

Configure **EIGRP (Enhanced Interior Gateway Routing Protocol)** across three routers arranged in a triangle topology.

This lab demonstrates dynamic route exchange, EIGRP neighbor formation, route learning, multiple paths, and end-to-end connectivity between three LANs.

---

## 🖧 Network Topology

```text
                PC1
                 |
               Switch1
                 |
                 R1
                /  \
               /    \
             R0------R2
             |        |
           Switch0  Switch2
             |        |
            PC0      PC2
```

The triangle topology provides multiple possible paths between routers and demonstrates EIGRP redundancy and route selection.

---

## 🔌 Router Connections

```text
R0 G0/0 → Switch0 Fa0/24
R0 G0/1 → R1 G0/0
R0 G0/2 → R2 G0/2

R1 G0/0 → R0 G0/1
R1 G0/1 → R2 G0/1
R1 G0/2 → Switch1 Fa0/24

R2 G0/0 → Switch2 Fa0/24
R2 G0/1 → R1 G0/1
R2 G0/2 → R0 G0/2
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
| R0 | G0/0 | 192.168.1.1 | 255.255.255.0 | — |
| R0 | G0/1 | 10.0.0.1 | 255.255.255.252 | — |
| R0 | G0/2 | 10.0.0.9 | 255.255.255.252 | — |
| R1 | G0/0 | 10.0.0.2 | 255.255.255.252 | — |
| R1 | G0/1 | 10.0.0.5 | 255.255.255.252 | — |
| R1 | G0/2 | 192.168.2.1 | 255.255.255.0 | — |
| PC1 | Fa0 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |
| R2 | G0/0 | 192.168.3.1 | 255.255.255.0 | — |
| R2 | G0/1 | 10.0.0.6 | 255.255.255.252 | — |
| R2 | G0/2 | 10.0.0.10 | 255.255.255.252 | — |
| PC2 | Fa0 | 192.168.3.10 | 255.255.255.0 | 192.168.3.1 |

---

## 🌐 Point-to-Point Networks

### R0 ↔ R1

```text
Network:   10.0.0.0/30
R0:        10.0.0.1
R1:        10.0.0.2
Broadcast: 10.0.0.3
```

### R1 ↔ R2

```text
Network:   10.0.0.4/30
R1:        10.0.0.5
R2:        10.0.0.6
Broadcast: 10.0.0.7
```

### R0 ↔ R2

```text
Network:   10.0.0.8/30
R0:        10.0.0.9
R2:        10.0.0.10
Broadcast: 10.0.0.11
```

The `/30` subnet is suitable for router-to-router links because it provides exactly two usable host addresses.

---

## ⚙️ Router Interface Configuration

### R0

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

interface g0/2
ip address 10.0.0.9 255.255.255.252
no shutdown
exit

end
```

### R1

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

### R2

```text
enable
configure terminal

interface g0/0
ip address 192.168.3.1 255.255.255.0
no shutdown
exit

interface g0/1
ip address 10.0.0.6 255.255.255.252
no shutdown
exit

interface g0/2
ip address 10.0.0.10 255.255.255.252
no shutdown
exit

end
```

---

## ⚡ EIGRP Configuration

For this lab:

```text
EIGRP AS Number: 7
```

All routers must use the same EIGRP Autonomous System number to form neighbor relationships.

---

## 📡 EIGRP Configuration on R0

```text
enable
configure terminal

router eigrp 7
network 192.168.1.0 0.0.0.255
network 10.0.0.0 0.0.0.3
network 10.0.0.8 0.0.0.3
no auto-summary

end
```

R0 advertises:

```text
192.168.1.0/24
10.0.0.0/30
10.0.0.8/30
```

---

## 📡 EIGRP Configuration on R1

```text
enable
configure terminal

router eigrp 7
network 10.0.0.0 0.0.0.3
network 10.0.0.4 0.0.0.3
network 192.168.2.0 0.0.0.255
no auto-summary

end
```

R1 advertises:

```text
10.0.0.0/30
10.0.0.4/30
192.168.2.0/24
```

---

## 📡 EIGRP Configuration on R2

```text
enable
configure terminal

router eigrp 7
network 10.0.0.4 0.0.0.3
network 10.0.0.8 0.0.0.3
network 192.168.3.0 0.0.0.255
no auto-summary

end
```

R2 advertises:

```text
10.0.0.4/30
10.0.0.8/30
192.168.3.0/24
```

---

## 🤝 EIGRP Neighbor Verification

Use:

```text
show ip eigrp neighbors
```

Because of the triangle topology, every router should form neighbor relationships with two routers.

Example on R0:

```text
IP-EIGRP neighbors for process 7

H   Address       Interface
0   10.0.0.2      Gig0/1
1   10.0.0.10     Gig0/2
```

This confirms that R0 has formed EIGRP adjacencies with both R1 and R2.

Example on R1:

```text
10.0.0.1 → R0
10.0.0.6 → R2
```

Successful adjacency messages can also appear as:

```text
%DUAL-5-NBRCHANGE: IP-EIGRP 7: Neighbor is up: new adjacency
```

---

## 🛣️ EIGRP Routing Table Verification

Use:

```text
show ip route
```

Routes learned through EIGRP are marked with:

```text
D
```

Example learned routes on R0:

```text
D 192.168.2.0/24
D 192.168.3.0/24
```

This confirms that R0 dynamically learned the LANs behind R1 and R2.

---

## 🔀 Equal-Cost Paths

In this lab, R0 learned the `10.0.0.4/30` network using two equal-cost paths:

```text
D 10.0.0.4/30 [90/3072] via 10.0.0.2
               [90/3072] via 10.0.0.10
```

This demonstrates how EIGRP can install multiple equal-cost routes into the routing table.

The two paths are:

```text
R0 → R1 → 10.0.0.4/30
```

and:

```text
R0 → R2 → 10.0.0.4/30
```

---

## ✅ Connectivity Testing

From PC0, test connectivity with PC1:

```text
ping 192.168.2.10
```

Then test connectivity with PC2:

```text
ping 192.168.3.10
```

Successful replies confirm end-to-end communication across the EIGRP network.

---

## 🧠 How EIGRP Works

EIGRP is an advanced dynamic routing protocol used to exchange routing information between routers.

EIGRP uses the **DUAL algorithm** to determine efficient and loop-free routes.

DUAL stands for:

```text
Diffusing Update Algorithm
```

EIGRP automatically forms neighbor relationships with other EIGRP routers using the same Autonomous System number.

After forming neighbors, the routers exchange routing information and calculate the best paths to remote networks.

---

## 📊 EIGRP Metric

EIGRP primarily calculates its metric using:

```text
Bandwidth
Delay
```

Other parameters can also exist in the metric calculation, but by default bandwidth and delay are the primary values used.

EIGRP does not simply choose the path with the lowest hop count like RIP.

---

## ⚡ Important EIGRP Characteristics

```text
Full Form:                Enhanced Interior Gateway Routing Protocol
Algorithm:                DUAL
Route Code:               D
Internal Administrative Distance: 90
External Administrative Distance: 170
Default Hello Interval:   5 seconds on common high-speed links
Maximum Equal-Cost Paths: Multiple
```

EIGRP converges faster than RIP and can efficiently support redundant paths.

---

## 🔍 Successor and Feasible Successor

### Successor

The **Successor** is the best route selected by EIGRP and installed in the routing table.

### Feasible Successor

A **Feasible Successor** is a backup route that can be used quickly if the primary route fails.

This allows EIGRP to recover quickly from network failures.

---

## 📚 EIGRP Tables

EIGRP maintains three important tables:

### Neighbor Table

Contains directly connected EIGRP neighbors.

Verify using:

```text
show ip eigrp neighbors
```

### Topology Table

Contains all routes learned through EIGRP.

Verify using:

```text
show ip eigrp topology
```

### Routing Table

Contains the best routes selected by EIGRP.

Verify using:

```text
show ip route
```

---

## 🔁 RIP vs OSPF vs EIGRP

| Feature | RIP | OSPF | EIGRP |
|---|---|---|---|
| Type | Distance Vector | Link-State | Advanced Distance Vector |
| Main Metric | Hop Count | Cost | Bandwidth + Delay |
| Algorithm | Bellman-Ford based | Dijkstra SPF | DUAL |
| Administrative Distance | 120 | 110 | 90 |
| Convergence | Slow | Fast | Fast |
| Route Code | R | O | D |
| Scalability | Small | Medium/Large | Medium/Large |
| Maximum Hop Limitation | 15 | No 15-hop limit | Much higher than RIP |

---

## 🔎 Useful Verification Commands

Check interfaces:

```text
show ip interface brief
```

Check EIGRP neighbors:

```text
show ip eigrp neighbors
```

Check EIGRP topology:

```text
show ip eigrp topology
```

Check routing table:

```text
show ip route
```

Check EIGRP protocol configuration:

```text
show ip protocols
```

Check running configuration:

```text
show running-config | section router eigrp
```

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows the complete triangle EIGRP topology.

### 2. R0 EIGRP Neighbors

```text
screenshots/02-r0-eigrp-neighbors.png
```

Shows R0 successfully forming adjacency with R1 and R2.

### 3. R1 EIGRP Neighbors

```text
screenshots/03-r1-eigrp-neighbors.png
```

Shows R1 successfully forming adjacency with R0 and R2.

### 4. R2 EIGRP Neighbors

```text
screenshots/04-r2-eigrp-neighbors.png
```

Shows R2 successfully forming adjacency with R0 and R1.

### 5. EIGRP Routing Table

```text
screenshots/05-eigrp-routing-table.png
```

Shows routes learned through EIGRP marked with `D` and demonstrates multiple equal-cost paths.

### 6. Connectivity Test

```text
screenshots/06-connectivity-test.png
```

Shows successful communication between devices located on remote LANs.

---

## 📁 Files

```text
EIGRP/
│
├── README.md
├── EIGRP.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-r0-eigrp-neighbors.png
    ├── 03-r1-eigrp-neighbors.png
    ├── 04-r2-eigrp-neighbors.png
    ├── 05-eigrp-routing-table.png
    └── 06-connectivity-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco Switches
- EIGRP
- IPv4
- CIDR / Subnetting
- Cisco IOS CLI

---

## 🏁 Result

Successfully configured **EIGRP AS 7 across three routers in a triangle topology**.

All routers successfully formed EIGRP neighbor relationships and dynamically exchanged routing information.

Remote LAN networks appeared in the routing tables as `D` routes.

The triangle topology also demonstrated multiple paths between routers and EIGRP route selection.

End-to-end communication between all three LANs was successfully verified.

This lab demonstrates:

- EIGRP configuration
- Autonomous System configuration
- EIGRP neighbor formation
- DUAL-based route selection
- Dynamic route learning
- Multiple-path routing
- `/30` point-to-point subnetting
- Routing table verification
- End-to-end connectivity