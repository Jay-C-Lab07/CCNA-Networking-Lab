# 🔀 Inter-VLAN Routing Configuration Lab

## 🎯 Objective

To understand and verify **Inter-VLAN Routing using Router-on-a-Stick** in Cisco Packet Tracer.

The lab demonstrates how to:

- Create multiple VLANs
- Assign access ports to VLANs
- Configure an 802.1Q trunk
- Create router subinterfaces
- Configure VLAN default gateways
- Route traffic between different VLANs
- Verify VLAN and trunk configurations
- Test Inter-VLAN connectivity

## 🖧 Network Topology

The lab uses:

- 1 Cisco Router
- 1 Cisco Switch
- 4 PCs
- VLAN 10 — SALES
- VLAN 20 — HR

PC0 and PC1 belong to **VLAN 10 — SALES**.

PC2 and PC3 belong to **VLAN 20 — HR**.

The router is connected to the switch using a single trunk link.

## 💻 IP Addressing

| Device | VLAN | IP Address | Subnet Mask | Default Gateway |
| --- | --- | --- | --- | --- |
| PC0 | VLAN 10 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | VLAN 10 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | VLAN 20 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC3 | VLAN 20 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |

## 🏷️ VLAN Configuration

VLAN 10 and VLAN 20 were created on the switch.

The access ports were assigned as:

```text
VLAN 10 SALES → Fa0/1, Fa0/2
VLAN 20 HR    → Fa0/3, Fa0/4
```

The configuration used was:

```bash
enable
configure terminal

vlan 10
name SALES
exit

vlan 20
name HR
exit

interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit
```

## 🔗 Trunk Configuration

Switch port `Fa0/24` was configured as an **802.1Q trunk** toward the router.

```bash
interface fastEthernet 0/24
switchport mode trunk
exit
end
```

The trunk allows traffic belonging to multiple VLANs to travel between the switch and router.

## 🌐 Router-on-a-Stick Configuration

The physical router interface connected to the switch was enabled:

```bash
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit
```

### 🔵 VLAN 10 Subinterface

```bash
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```

`192.168.10.1` acts as the default gateway for VLAN 10.

### 🟠 VLAN 20 Subinterface

```bash
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
end
```

`192.168.20.1` acts as the default gateway for VLAN 20.

## 🔍 VLAN Verification

The VLAN configuration was verified using:

```bash
show vlan brief
```

The output confirmed:

```text
VLAN 10 SALES → Fa0/1, Fa0/2
VLAN 20 HR    → Fa0/3, Fa0/4
```

## 🔗 Trunk Verification

The trunk connection was verified using:

```bash
show interfaces trunk
```

The CLI output confirmed:

```text
Fa0/24   on   802.1q   trunking
```

The active VLANs on the trunk were:

```text
1,10,20
```

This confirmed that traffic for VLAN 10 and VLAN 20 could travel through the trunk link.

## 🌐 Router Subinterface Verification

The router configuration was verified using:

```bash
show ip interface brief
```

The output confirmed:

```text
GigabitEthernet0/0       unassigned       up   up
GigabitEthernet0/0.10    192.168.10.1     up   up
GigabitEthernet0/0.20    192.168.20.1     up   up
```

This confirmed that the physical router interface and both VLAN subinterfaces were operational.

## 🔄 Inter-VLAN Traffic Flow

When PC0 in VLAN 10 communicates with PC2 in VLAN 20, traffic follows this path:

```text
PC0
192.168.10.10
      ↓
VLAN 10
      ↓
Switch
      ↓
Fa0/24 Trunk
      ↓
Router G0/0.10
192.168.10.1
      ↓
Layer 3 Routing
      ↓
Router G0/0.20
192.168.20.1
      ↓
Fa0/24 Trunk
      ↓
Switch
      ↓
VLAN 20
      ↓
PC2
192.168.20.10
```

The router performs **Layer 3 routing** between the two VLAN networks.

## 🧪 Connectivity Testing

Same-VLAN communication was verified using:

```bash
ping 192.168.10.11
```

PC0 successfully communicated with PC1 within VLAN 10.

Inter-VLAN communication was then tested from PC0 in VLAN 10 to PC2 in VLAN 20:

```bash
ping 192.168.20.10
```

The successful result showed:

```text
Packets: Sent = 4, Received = 4, Lost = 0
```

This confirmed that **Inter-VLAN Routing was working successfully**.

## 📸 Screenshots

The `screenshots` directory contains:

- `01-topology.png` — Complete Inter-VLAN Routing topology
- `02-vlan-verification.png` — VLAN and port assignment verification
- `03-trunk-verification.png` — 802.1Q trunk verification
- `04-router-subinterfaces.png` — Router interface and subinterface verification
- `05-inter-vlan-ping.png` — Successful Inter-VLAN connectivity test

## 📁 Files

- `Inter-VLAN-Routing.pkt` — Cisco Packet Tracer Inter-VLAN Routing practical
- `screenshots/` — Practical evidence and verification screenshots

## 🏁 Result

Successfully implemented and verified **Inter-VLAN Routing using Router-on-a-Stick**. VLAN 10 and VLAN 20 were created and assigned to separate switch ports, Fa0/24 was configured as an 802.1Q trunk, router subinterfaces were configured as the default gateways for both VLANs, and successful communication between devices in different VLANs was verified.

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- VLANs
- 802.1Q Trunking
- Router-on-a-Stick
- Router Subinterfaces
- IPv4 Addressing
- ICMP Ping Testing
- `show vlan brief`
- `show interfaces trunk`
- `show ip interface brief`