# 🌲 STP Configuration Lab

## 🎯 Objective

To understand and verify the operation of **Spanning Tree Protocol (STP)** in a redundant switched network using Cisco Packet Tracer.

The lab demonstrates how STP:

* Elects a Root Bridge
* Selects Root Ports
* Selects Designated Ports
* Blocks redundant paths
* Prevents Layer 2 switching loops
* Maintains network connectivity

## 🖧 Network Topology

The lab uses:

* 3 Cisco switches
* 3 PCs
* Redundant switch-to-switch links forming a triangle topology

Each PC is connected to one switch.

## 💻 IP Addressing

| Device | IP Address   | Subnet Mask   |
| ------ | ------------ | ------------- |
| PC0    | 192.168.1.10 | 255.255.255.0 |
| PC1    | 192.168.1.20 | 255.255.255.0 |
| PC2    | 192.168.1.30 | 255.255.255.0 |

## 🌳 STP Root Bridge Election

STP was verified using:

```bash
show spanning-tree
```

Switch2 was automatically elected as the **Root Bridge**.

The CLI output confirmed:

```text
This bridge is the root
```

Root Bridge MAC Address:

```text
0001.43C2.873A
```

## 🔀 STP Port Roles

### Switch2

Switch2 is the Root Bridge, so its active switch ports operate as Designated Forwarding ports.

### Switch0

Switch0 uses its direct connection toward Switch2 as its Root Port.

### Switch1

Switch1 also uses its direct connection toward Switch2 as its Root Port.

The redundant connection toward Switch0 was placed into an Alternate Blocking state.

The CLI output showed:

```text
Fa0/23   Altn BLK
Fa0/24   Root FWD
```

This prevents a Layer 2 switching loop while keeping the redundant link available as a backup path.

## ✅ Connectivity Testing

Connectivity between the PCs was verified using ICMP ping tests.

Example:

```bash
ping 192.168.1.20
ping 192.168.1.30
```

The devices remained reachable even though STP blocked one redundant switch link.

## 📸 Screenshots

The `screenshots` directory contains:

* `01-topology.png` — Complete STP topology
* `02-root-bridge.png` — Root Bridge verification
* `03-blocked-port.png` — Alternate/Blocking port verification
* `04-connectivity-test.png` — Successful connectivity testing

## 📁 Files

* `STP.pkt` — Cisco Packet Tracer STP practical
* `screenshots/` — Practical evidence and verification screenshots

## 🏁 Result

Successfully implemented and verified STP in a redundant Layer 2 network. STP elected Switch2 as the Root Bridge, selected appropriate Root and Designated Ports, blocked the redundant path on Switch1 Fa0/23, and maintained end-to-end network connectivity while preventing switching loops.
