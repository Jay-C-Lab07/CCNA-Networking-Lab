# 📶 Wireless Networking Lab

## 🎯 Objective

Configure a basic wireless network in Cisco Packet Tracer using a **Home Router PT-AC** and connect multiple wireless clients securely using **WPA2-Personal**.

This lab demonstrates:

- Wireless router configuration
- SSID configuration
- WPA2 security
- Wireless client adapter setup
- DHCP-based IP assignment
- Wireless client connectivity
- Basic connectivity testing

---

## 🖧 Network Topology

```text
PC0 -------- Switch0 -------- Home Router PT-AC
                                  )))
                                  ))) Laptop0
                                  ))) Laptop1
```

The wired PC connects through the switch, while the laptops connect wirelessly to the Home Router.

---

## 🔌 Wired Connections

```text
PC0 Fa0 → Switch0 Fa0/1

Switch0 Fa0/24 → Home Router GigabitEthernet 1
```

The `Internet` port on the Home Router was not used.

---

## 📡 Wireless Clients

The wireless clients used in this lab are:

```text
Laptop0
Laptop1
```

Both laptops connect wirelessly to the Home Router.

---

## 🔧 Wireless Adapter Installation

The laptops initially could not connect wirelessly because they did not have a compatible wireless adapter installed.

Packet Tracer displayed:

```text
A WMP300N or WPC300N wireless interface is required to connect.
```

For the laptops, the following adapter was installed:

```text
WPC300N
```

Installation procedure:

```text
Laptop → Physical
      ↓
Power Off
      ↓
Install WPC300N
      ↓
Power On
```

After installing the wireless adapter, the laptops were able to detect wireless networks.

---

## 🌐 Wireless Router Configuration

The following wireless router was used:

```text
Home Router PT-AC
```

The LAN interface was configured with:

```text
IP Address: 192.168.60.1
Subnet Mask: 255.255.255.0
```

This address acts as the default gateway for the wireless clients.

---

## 📡 Wireless Network Configuration

The 2.4 GHz wireless network was configured with:

```text
SSID: CCNA-LAB
```

The SSID identifies the wireless network that clients connect to.

---

## 🔐 Wireless Security

Wireless security was configured using:

```text
WPA2-Personal
```

The pre-shared key used in the lab was:

```text
Cisco@123
```

Both laptops were configured with the same WPA2 security method and pre-shared key.

---

## 📶 Client Connection

On each laptop:

```text
Desktop → PC Wireless
```

The wireless network:

```text
CCNA-LAB
```

was selected.

Security:

```text
WPA2-Personal
```

Pre-shared key:

```text
Cisco@123
```

After entering the correct key, the laptops successfully connected to the access point.

---

## ✅ Wireless Connection Verification

Packet Tracer displayed:

```text
You have successfully connected to the access point
```

The wireless client interface also displayed:

```text
Adapter is Active
```

along with signal strength and link quality indicators.

This confirmed successful Layer 2 wireless connectivity.

---

## 📡 DHCP Configuration

The Home Router DHCP service was used to automatically provide client IP addresses.

Wireless clients received addresses from:

```text
192.168.60.0/24
```

Example client configuration:

```text
IP Address:      192.168.60.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.60.1
```

This eliminates the need to manually configure IP addresses on each wireless device.

---

## 💻 Client IP Verification

On the laptop:

```text
Desktop → IP Configuration
```

DHCP was selected.

The laptop successfully received an IP address belonging to:

```text
192.168.60.0/24
```

and the default gateway:

```text
192.168.60.1
```

---

## ✅ Connectivity Testing

From Laptop0, the Home Router was tested using:

```text
ping 192.168.60.1
```

Successful replies confirmed connectivity between the wireless client and the Home Router.

Connectivity between wireless clients was also tested.

Example:

```text
ping <Laptop1-IP>
```

Successful replies confirmed communication between devices connected through the wireless network.

---

## 🧠 What Is Wireless Networking?

Wireless networking allows devices to communicate using radio signals instead of physical Ethernet cables.

Typical wireless devices include:

```text
Laptops
Phones
Tablets
IoT Devices
Wireless Printers
```

Devices connect to an access point or wireless router using a wireless network name known as an:

```text
SSID
```

---

## 📡 SSID

SSID stands for:

```text
Service Set Identifier
```

It is the name of the wireless network.

In this lab:

```text
SSID = CCNA-LAB
```

Clients search for this SSID and connect using the correct security credentials.

---

## 🔐 WPA2

WPA2 stands for:

```text
Wi-Fi Protected Access 2
```

It provides encryption and authentication for wireless networks.

In this lab:

```text
Security: WPA2-Personal
```

was used with a pre-shared key.

---

## 📻 2.4 GHz Wireless Network

The lab used:

```text
2.4 GHz
```

The 2.4 GHz frequency band generally provides:

- Longer range
- Better wall penetration
- Wider device compatibility

However, it may experience more interference than higher-frequency wireless bands.

---

## ⚡ Important Wireless Networking Facts

```text
SSID:
Wireless network name

WPA2:
Wireless security standard

WPC300N:
Laptop wireless adapter used in Packet Tracer

2.4 GHz:
Wireless frequency band

DHCP:
Automatically assigns IP settings
```

---

## 🔎 Useful Verification

Check the laptop wireless connection:

```text
Desktop → PC Wireless
```

Check IP configuration:

```text
Desktop → IP Configuration
```

Test router connectivity:

```text
ping 192.168.60.1
```

Test client-to-client connectivity:

```text
ping <destination-IP>
```

---

## 🌍 Real-World Use

Wireless networking is commonly used in:

- Homes
- Offices
- Universities
- Hotels
- Airports
- Cafes
- Hospitals
- Enterprise networks

Wireless access allows users to move around while remaining connected to the network.

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows:

```text
PC0
Switch0
Home Router PT-AC
Laptop0
Laptop1
```

---

### 2. Wireless Router Settings

```text
screenshots/02-wireless-router-settings.png
```

Shows the wireless network configuration including:

```text
SSID: CCNA-LAB
WPA2-Personal
LAN IP: 192.168.60.1
```

---

### 3. Laptop0 Wireless Connection

```text
screenshots/03-laptop0-wireless-connection.png
```

Shows Laptop0 successfully connected to the wireless access point.

---

### 4. Laptop1 Wireless Connection

```text
screenshots/04-laptop1-wireless-connection.png
```

Shows Laptop1 successfully connected to the wireless access point.

---

### 5. DHCP IP Settings

```text
screenshots/05-dhcp-ip-settings.png
```

Shows a wireless client receiving:

```text
192.168.60.x
```

through DHCP with:

```text
Default Gateway: 192.168.60.1
```

---

### 6. Connectivity Test

```text
screenshots/06-connectivity-test.png
```

Shows successful connectivity between the wireless client and:

```text
192.168.60.1
```

and/or another wireless client.

---

## 📁 Files

```text
Wireless-Networking/
│
├── README.md
├── Wireless-Networking.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-wireless-router-settings.png
    ├── 03-laptop0-wireless-connection.png
    ├── 04-laptop1-wireless-connection.png
    ├── 05-dhcp-ip-settings.png
    └── 06-connectivity-test.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Home Router PT-AC
- Cisco 2960 Switch
- PC
- Laptops
- WPC300N Wireless Adapter
- WPA2-Personal
- DHCP
- IPv4
- ICMP

---

## 🏁 Result

Successfully configured a **wireless LAN in Cisco Packet Tracer**.

The Home Router PT-AC was configured with:

```text
SSID: CCNA-LAB
Security: WPA2-Personal
```

Both laptops successfully connected to the wireless network using the WPC300N wireless adapter.

The clients received IP addresses automatically through DHCP and successfully communicated with the router and other wireless devices.

This lab demonstrates:

- Wireless router configuration
- SSID configuration
- WPA2 security
- Wireless adapter installation
- Wireless client authentication
- DHCP-based addressing
- Wireless connectivity testing