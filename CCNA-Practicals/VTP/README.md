# 🔁 VTP Configuration Lab

## 🎯 Objective

To configure VLAN Trunking Protocol (VTP) using Cisco Packet Tracer, configure one switch as a VTP Server and two switches as VTP Clients, and verify automatic VLAN synchronization between the switches.

## 🧩 VTP Details

| Switch | VTP Mode | VTP Domain | VTP Version |
| ------- | -------- | ---------- | ----------- |
| SW0     | Server   | JAYLAB     | 2           |
| SW1     | Client   | JAYLAB     | 2           |
| SW2     | Client   | JAYLAB     | 2           |

## 🔗 Trunk Connections

| Connection | Ports |
| ---------- | ----- |
| SW0 ↔ SW1 | SW0 Fa0/24 ↔ SW1 Fa0/24 |
| SW1 ↔ SW2 | SW1 Fa0/23 ↔ SW2 Fa0/24 |

## ⚙️ Trunk Configuration

### SW0

```bash
enable
configure terminal
interface fastEthernet 0/24
switchport mode trunk
end
```

### SW1

```bash
enable
configure terminal

interface fastEthernet 0/24
switchport mode trunk
exit

interface fastEthernet 0/23
switchport mode trunk
exit

end
```

### SW2

```bash
enable
configure terminal
interface fastEthernet 0/24
switchport mode trunk
end
```

## 🌐 VTP Server Configuration

SW0 was configured as the VTP Server.

```bash
enable
configure terminal

vtp domain JAYLAB
vtp mode server
vtp password cisco123
vtp version 2

end
```

## 💻 VTP Client Configuration

SW1 and SW2 were configured as VTP Clients.

```bash
enable
configure terminal

vtp domain JAYLAB
vtp mode client
vtp password cisco123

end
```

## 🧩 VLAN Creation

The following VLANs were created only on the VTP Server (SW0):

```bash
configure terminal

vlan 10
name SALES

vlan 20
name HR

vlan 30
name IT

end
```

## 🔎 VTP Verification

The VTP configuration was verified using:

```bash
show vtp status
```

The output confirmed:

* SW0 was operating in VTP Server mode
* SW1 and SW2 were operating in VTP Client mode
* VTP Domain was set to `JAYLAB`
* VTP Version 2 was running
* VLAN database updates were synchronized between the switches

## 🔗 Trunk Verification

Trunk links were verified using:

```bash
show interfaces trunk
```

The output confirmed that the switch-to-switch links were operating as 802.1Q trunks.

## 🔄 VLAN Synchronization

VLAN synchronization was verified using:

```bash
show vlan brief
```

The following VLANs were created only on SW0:

* VLAN 10 → SALES
* VLAN 20 → HR
* VLAN 30 → IT

The same VLANs automatically appeared on SW1 and SW2 without manually creating them on the client switches.

This confirms successful VTP synchronization.

## 📌 Important Concept

VTP automatically synchronizes VLAN database information between switches in the same VTP domain.

However, VTP does **not** automatically assign switch ports to VLANs.

Access-port assignments must still be configured manually.

## 📸 Screenshots

The `screenshots` folder contains evidence of:

* VTP network topology
* VTP Server configuration
* VTP Client configuration
* VLANs created on the VTP Server
* VLANs automatically synchronized to VTP Clients
* 802.1Q trunk verification

## 📂 Files

* `VTP.pkt` — Cisco Packet Tracer VTP lab file
* `screenshots/` — Topology, configuration, synchronization, and verification evidence

## ✅ Result

Successfully configured VTP Version 2 with SW0 as the VTP Server and SW1/SW2 as VTP Clients. VLAN 10 (SALES), VLAN 20 (HR), and VLAN 30 (IT) were created on the server and automatically synchronized to both client switches through 802.1Q trunk links.

## 🛠️ Tools Used

* Cisco Packet Tracer
* Cisco IOS CLI
* VTP Version 2
* VLAN Configuration
* 802.1Q Trunking
* `show vtp status`
* `show vlan brief`
* `show interfaces trunk`