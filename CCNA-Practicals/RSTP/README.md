# ⚡ RSTP Configuration Lab

## 🎯 Objective

To understand and verify the operation of **Rapid Spanning Tree Protocol (RSTP)** in a redundant switched network using Cisco Packet Tracer.

The lab demonstrates how RSTP:

- Elects a Root Bridge
- Selects Root Ports
- Selects Designated Ports
- Uses Alternate Ports as backup paths
- Prevents Layer 2 switching loops
- Reconverges quickly after a link failure
- Restores the preferred path after link recovery

## 🖧 Network Topology

The lab uses:

- 3 Cisco switches
- 3 PCs
- Redundant switch-to-switch links forming a triangle topology

Each PC is connected to one switch.

## 💻 IP Addressing

| Device | IP Address | Subnet Mask |
| --- | --- | --- |
| PC0 | 192.168.1.10 | 255.255.255.0 |
| PC1 | 192.168.1.20 | 255.255.255.0 |
| PC2 | 192.168.1.30 | 255.255.255.0 |

## ⚙️ RSTP Configuration

RSTP was enabled on all switches using:

```bash
enable
configure terminal
spanning-tree mode rapid-pvst
end
```

The same configuration was applied on Switch0, Switch1, and Switch2.

## 👑 Root Bridge Election

RSTP was verified using:

```bash
show spanning-tree
```

Switch2 was elected as the **Root Bridge**.

The CLI output confirmed:

```text
Spanning tree enabled protocol rstp
This bridge is the root
```

Root Bridge MAC Address:

```text
0001.43C2.873A
```

## 🔀 RSTP Port Roles

### 🟢 Switch2 — Root Bridge

Switch2 is the Root Bridge, so its active switch-to-switch ports operate as **Designated Forwarding ports**.

### 🔵 Switch0

Switch0 uses its direct connection toward Switch2 as its **Root Port**.

### 🟠 Switch1

Switch1 also uses its direct connection toward Switch2 as its **Root Port**.

The redundant connection toward Switch0 was placed into an **Alternate Blocking** state.

The CLI output showed:

```text
Fa0/23   Altn BLK
Fa0/24   Root FWD
```

This means:

- `Fa0/24` → Root Port ✅
- `Fa0/23` → Alternate backup port 🚫
- The Alternate Port remains blocked during normal operation to prevent a Layer 2 loop

## ⚡ Failover Testing

To simulate a link failure, the Root Port on Switch1 was manually shut down:

```bash
configure terminal
interface fastEthernet 0/24
shutdown
end
```

After the link failure, RSTP rapidly recalculated the topology.

The previous Alternate Port became the new Root Port:

```text
Fa0/23   Root FWD
```

Traffic now followed the backup path:

```text
Switch1 → Switch0 → Switch2
```

The new Root Path Cost became:

```text
38
```

This demonstrates the fast reconvergence capability of RSTP.

## 🔄 Link Recovery

The original link was restored using:

```bash
configure terminal
interface fastEthernet 0/24
no shutdown
end
```

RSTP recalculated the topology and restored the preferred path.

The port roles returned to:

```text
Fa0/23   Altn BLK
Fa0/24   Root FWD
```

This confirms that RSTP automatically responds to both **link failure and link recovery**.

## 🧪 Connectivity Testing

Connectivity between the PCs was verified using ICMP ping tests.

Example:

```bash
ping 192.168.1.20
ping 192.168.1.30
```

The devices remained reachable while RSTP handled redundant paths and failover.

## 📸 Screenshots

The `screenshots` directory contains:

- `01-topology.png` — Complete RSTP topology
- `02-root-bridge.png` — Root Bridge verification
- `03-alternate-port.png` — Alternate/Blocked port verification
- `04-failover-after-link-down.png` — Backup path becoming Root Port
- `05-recovery-after-link-restored.png` — Preferred path restored
- `06-connectivity-test.png` — Successful connectivity testing

## 📁 Files

- `RSTP.pkt` — Cisco Packet Tracer RSTP practical
- `screenshots/` — Practical evidence and verification screenshots

## 🏁 Result

Successfully implemented and verified **Rapid Spanning Tree Protocol (RSTP)** in a redundant Layer 2 network. RSTP elected Switch2 as the Root Bridge, selected appropriate Root, Designated, and Alternate Ports, prevented switching loops, rapidly reconverged after a link failure, restored the preferred path after recovery, and maintained end-to-end network connectivity.

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Rapid-PVST
- Spanning Tree Protocol
- ICMP Ping Testing
- `show spanning-tree`