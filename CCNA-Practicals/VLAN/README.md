# 🌐 VLAN Configuration Lab

## 🎯 Objective

To configure multiple VLANs on a Cisco switch, assign switch ports to the appropriate VLANs, and verify communication between devices within the same VLAN.

## 🧩 VLAN Details

| VLAN ID | VLAN Name | Assigned Ports |
| ------- | --------- | -------------- |
| 10      | Sales     | Fa0/1, Fa0/2   |
| 20      | HR        | Fa0/3, Fa0/4   |

## 💻 IP Addressing

| Device | VLAN    | IP Address    | Subnet Mask   |
| ------ | ------- | ------------- | ------------- |
| PC0    | VLAN 10 | 192.168.10.10 | 255.255.255.0 |
| PC1    | VLAN 10 | 192.168.10.11 | 255.255.255.0 |
| PC2    | VLAN 20 | 192.168.20.10 | 255.255.255.0 |
| PC3    | VLAN 20 | 192.168.20.11 | 255.255.255.0 |

## ⚙️ Switch Configuration

```bash
enable
configure terminal

vlan 10
name Sales
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

end
copy running-config startup-config
```

## 🔎 Verification

The VLAN configuration was verified using:

```bash
show vlan brief
```

The output confirmed:

* VLAN 10 (Sales) → Fa0/1 and Fa0/2
* VLAN 20 (HR) → Fa0/3 and Fa0/4

## 📡 Connectivity Testing

### ✅ Sales VLAN

PC0 successfully communicated with PC1.

```bash
ping 192.168.10.11
```

### ✅ HR VLAN

PC2 successfully communicated with PC3.

```bash
ping 192.168.20.11
```

### 🚫 VLAN Isolation

Communication between VLAN 10 and VLAN 20 failed because Inter-VLAN Routing was not configured.

Example:

```bash
ping 192.168.20.10
```

This confirms that devices in different VLANs are logically isolated.

## 📸 Screenshots

The `screenshots` folder contains evidence of:

* Network topology
* VLAN configuration and verification
* Sales VLAN connectivity
* HR VLAN connectivity
* VLAN isolation testing

## 📂 Files

* `VLAN.pkt` — Cisco Packet Tracer VLAN lab file
* `screenshots/` — Topology, configuration, and connectivity evidence

## ✅ Result

Successfully created and configured VLAN 10 and VLAN 20, assigned access ports, verified VLAN membership, tested same-VLAN communication, and confirmed isolation between different VLANs.

## 🛠️ Tools Used

* Cisco Packet Tracer
* Cisco IOS CLI
* VLAN Configuration
* Ping Testing
* `show vlan brief`
