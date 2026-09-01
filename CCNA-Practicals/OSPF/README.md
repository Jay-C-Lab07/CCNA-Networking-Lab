# 🌐 OSPF Routing Lab

## 🎯 Objective

Configure **OSPF (Open Shortest Path First)** across multiple routers so that each router can dynamically learn remote networks and build routes automatically.

This lab demonstrates OSPF neighbor formation, route advertisement, routing table updates, and end-to-end communication between multiple LANs.

---

## 🖧 Network Topology

```text
PC0 --- Switch0 --- R0 ----- R1 ----- R2 --- Switch2 --- PC2
                          |
                       Switch1
                          |
                         PC1
```

### Router Connections

```text
R0 G0/0 → Switch0 Fa0/24
R0 G0/1 → R1 G0/0

R1 G0/0 → R0 G0/1
R1 G0/1 → R2 G0/1
R1 G0/2 → Switch1 Fa0/24

R2 G0/1 → R1 G0/1
R2 G0/0 → Switch2 Fa0/24
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
| R1 | G0/0 | 10.0.0.2 | 255.255.255.252 | — |
| R1 | G0/1 | 10.0.0.5 | 255.255.255.252 | — |
| R1 | G0/2 | 192.168.2.1 | 255.255.255.0 | — |
| PC1 | Fa0 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |
| R2 | G0/1 | 10.0.0.6 | 255.255.255.252 | — |
| R2 | G0/0 | 192.168.3.1 | 255.255.255.0 | — |
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

A `/30` subnet is commonly used for point-to-point router links because it provides exactly two usable IP addresses.

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

## 🔵 OSPF Configuration

For this lab:

```text
OSPF Process ID: 1
OSPF Area:       0
```

Area 0 is the OSPF backbone area.

---

## 📡 OSPF Configuration on R0

```text
enable
configure terminal

router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0

end
```

R0 advertises:

```text
192.168.1.0/24
10.0.0.0/30
```

---

## 📡 OSPF Configuration on R1

```text
enable
configure terminal

router ospf 1
network 10.0.0.0 0.0.0.3 area 0
network 10.0.0.4 0.0.0.3 area 0
network 192.168.2.0 0.0.0.255 area 0

end
```

R1 advertises:

```text
10.0.0.0/30
10.0.0.4/30
192.168.2.0/24
```

---

## 📡 OSPF Configuration on R2

```text
enable
configure terminal

router ospf 1
network 10.0.0.4 0.0.0.3 area 0
network 192.168.3.0 0.0.0.255 area 0

end
```

R2 advertises:

```text
10.0.0.4/30
192.168.3.0/24
```

---

## 🧮 OSPF Wildcard Masks

OSPF uses wildcard masks in its `network` commands.

Examples:

```text
255.255.255.0
↓
0.0.0.255
```

and:

```text
255.255.255.252
↓
0.0.0.3
```

Therefore:

```text
network 192.168.1.0 0.0.0.255 area 0
```

matches interfaces belonging to:

```text
192.168.1.0/24
```

And:

```text
network 10.0.0.0 0.0.0.3 area 0
```

matches interfaces belonging to:

```text
10.0.0.0/30
```

---

## 🤝 OSPF Neighbor Verification

Use:

```text
show ip ospf neighbor
```

On the middle router R1, both R0 and R2 formed OSPF adjacencies.

Example:

```text
Neighbor ID     Pri   State       Address      Interface

192.168.3.1       1   FULL/DR     10.0.0.6     GigabitEthernet0/1
192.168.1.1       1   FULL/DR     10.0.0.1     GigabitEthernet0/0
```

The `FULL` state confirms that the OSPF routers successfully established adjacency and exchanged link-state information.

---

## 🛣️ OSPF Routing Table Verification

Use:

```text
show ip route
```

Routes learned through OSPF are identified by:

```text
O
```

For example, R0 dynamically learns remote LANs such as:

```text
O 192.168.2.0/24
O 192.168.3.0/24
```

This confirms that OSPF is successfully exchanging routing information.

---

## ✅ Connectivity Testing

From PC0, test connectivity to PC1:

```text
ping 192.168.2.10
```

Then test connectivity to PC2:

```text
ping 192.168.3.10
```

Successful replies confirm end-to-end routing through the OSPF-enabled network.

---

## 🧠 How OSPF Works

OSPF is a **Link-State Dynamic Routing Protocol**.

OSPF routers exchange information about their network links and build a complete view of the network topology.

OSPF then uses the **Shortest Path First algorithm** to calculate the best route.

OSPF uses **cost** as its routing metric.

Unlike RIP, which mainly uses hop count, OSPF considers link cost when selecting routes.

---

## ⚡ Important OSPF Characteristics

```text
Full Form:                Open Shortest Path First
Protocol Type:            Link-State
Algorithm