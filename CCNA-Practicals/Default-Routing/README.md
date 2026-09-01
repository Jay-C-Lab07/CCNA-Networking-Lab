# 🛣️ Default Routing Configuration Lab

## 🎯 Objective

To configure and verify **Default Routing** between two different LAN networks using Cisco Packet Tracer.

This lab demonstrates how to:

- Configure IP addresses on router interfaces
- Connect two different LAN networks
- Configure a default route
- Configure a return static route
- Verify routing tables
- Test end-to-end connectivity between remote networks

## 🖧 Network Topology

The lab uses:

- 2 Cisco Routers
- 2 Cisco Switches
- 2 PCs
- 3 different IP networks

The topology is:

```text
PC0 --- Switch0 --- Router0 -------- Router1 --- Switch1 --- PC1
```

## 💻 IP Addressing

| Device | Interface | IP Address | Subnet Mask |
| --- | --- | --- | --- |
| PC0 | NIC | 192.168.1.10 | 255.255.255.0 |
| Router0 | G0/0 | 192.168.1.1 | 255.255.255.0 |
| Router0 | G0/1 | 10.0.0.1 | 255.255.255.252 |
| Router1 | G0/1 | 10.0.0.2 | 255.255.255.252 |
| Router1 | G0/0 | 192.168.2.1 | 255.255.255.0 |
| PC1 | NIC | 192.168.2.10 | 255.255.255.0 |

Default Gateways:

```text
PC0 → 192.168.1.1
PC1 → 192.168.2.1
```

## ⚙️ Router0 Configuration

Router0 was configured with one interface for LAN 1 and another interface for the router-to-router connection.

```bash
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit

end
```

## ⚙️ Router1 Configuration

Router1 was configured with one interface for LAN 2 and another interface for the router-to-router connection.

```bash
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.2 255.255.255.252
no shutdown
exit

end
```

## 🛣️ Default Route Configuration

A default route was configured on Router0 using:

```bash
configure terminal

ip route 0.0.0.0 0.0.0.0 10.0.0.2

end
```

This means:

```text
If Router0 does not know the destination network,
send the traffic to Router1 at 10.0.0.2.
```

The default route acts as the **Gateway of Last Resort**.

## 🔁 Return Static Route Configuration

Router1 also needs to know how to return traffic to the `192.168.1.0/24` network.

The following static route was configured:

```bash
configure terminal

ip route 192.168.1.0 255.255.255.0 10.0.0.1

end
```

This means:

```text
To reach the 192.168.1.0/24 network,
send traffic to Router0 at 10.0.0.1.
```

## 🌐 Router Interface Verification

Router interfaces were verified using:

```bash
show ip interface brief
```

Router0 showed:

```text
GigabitEthernet0/0   192.168.1.1   up   up
GigabitEthernet0/1   10.0.0.1      up   up
```

Router1 showed:

```text
GigabitEthernet0/0   192.168.2.1   up   up
GigabitEthernet0/1   10.0.0.2      up   up
```

This confirmed that all required router interfaces were operational.

## 🔍 Router0 Routing Table Verification

Router0 routing was verified using:

```bash
show ip route
```

The routing table contained the default route:

```text
S* 0.0.0.0/0 via 10.0.0.2
```

The `S*` indicates a **static default route**.

## 🔍 Router1 Routing Table Verification

Router1 routing was verified using:

```bash
show ip route
```

The routing table contained:

```text
S 192.168.1.0/24 [1/0] via 10.0.0.1
```

This confirmed that Router1 knew how to reach LAN 1 through Router0.

## 🔄 Traffic Flow

When PC0 communicates with PC1, the traffic follows this path:

```text
PC0
192.168.1.10
     |
     ↓
Switch0
     |
     ↓
Router0
192.168.1.1
     |
     ↓
Default Route
10.0.0.2
     |
     ↓
Router1
192.168.2.1
     |
     ↓
Switch1
     |
     ↓
PC1
192.168.2.10
```

The reply traffic returns using Router1's static route:

```text
PC1
     ↓
Router1
     ↓
10.0.0.1
     ↓
Router0
     ↓
PC0
```

## 🧪 Connectivity Testing

End-to-end connectivity was tested from PC0 to PC1 using:

```bash
ping 192.168.2.10
```

Connectivity was also tested from PC1 to PC0 using:

```bash
ping 192.168.1.10
```

The ping tests were successful, confirming that traffic could travel between both LAN networks.

## 📸 Screenshots

The `screenshots` directory contains:

- `01-topology.png` — Complete Default Routing topology
- `02-router0-interfaces.png` — Router0 interface verification
- `03-router1-interfaces.png` — Router1 interface verification
- `04-router0-default-route.png` — Router0 default route verification
- `05-router1-static-route.png` — Router1 return static route verification
- `06-connectivity-test.png` — Successful end-to-end connectivity test

## 📁 Files

- `Default-Routing.pkt` — Cisco Packet Tracer Default Routing practical
- `screenshots/` — Practical evidence and verification screenshots

## 🏁 Result

Successfully configured and verified **Default Routing** between two separate LAN networks. Router0 was configured with a default route toward Router1, while Router1 was configured with a static return route toward LAN 1. Both routers successfully exchanged traffic between the `192.168.1.0/24` and `192.168.2.0/24` networks, and end-to-end connectivity was verified using ICMP ping testing.

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- IPv4 Addressing
- Default Routing
- Static Routing
- Router Interface Configuration
- ICMP Ping Testing
- `show ip interface brief`
- `show ip route`