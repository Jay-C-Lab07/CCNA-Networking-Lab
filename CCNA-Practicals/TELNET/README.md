# 🖥️ Telnet Remote Access Lab

## 🎯 Objective

Configure **Telnet remote access** on a Cisco router so that a PC can connect to the router remotely through the network and access the router CLI.

This lab demonstrates:

- Basic IP connectivity
- VTY line configuration
- Telnet password authentication
- Remote CLI access
- Enable secret configuration
- Privileged EXEC access

---

## 🖧 Network Topology

```text
PC0 -------- Switch0 -------- Router0
```

---

## 🔌 Device Connections

```text
PC0 Fa0        → Switch0 Fa0/1
Switch0 Fa0/24 → Router0 G0/0
```

---

## 💻 IP Addressing

### Router0

```text
Interface:   GigabitEthernet0/0
IP Address:  192.168.50.1
Subnet Mask: 255.255.255.0
```

### PC0

```text
IP Address:      192.168.50.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.50.1
```

---

## ⚙️ Router Interface Configuration

```text
enable
configure terminal

interface g0/0
ip address 192.168.50.1 255.255.255.0
no shutdown
exit

end
```

Verify using:

```text
show ip interface brief
```

The interface should show:

```text
GigabitEthernet0/0   192.168.50.1   up   up
```

---

## 🔐 Telnet Configuration

Configure the router hostname:

```text
enable
configure terminal

hostname R0
```

Configure an enable secret:

```text
enable secret cisco123
```

Disable DNS lookup for mistyped commands:

```text
no ip domain-lookup
```

Configure the VTY lines:

```text
line vty 0 4
password telnet123
login
transport input telnet
exit

end
```

---

## 🧠 VTY Lines

VTY stands for:

```text
Virtual Teletype
```

VTY lines are used for remote access to Cisco network devices.

The command:

```text
line vty 0 4
```

configures five virtual terminal sessions:

```text
0
1
2
3
4
```

---

## 🔑 Telnet Password

The configured Telnet password is:

```text
telnet123
```

This password is requested when a remote user connects to the router using Telnet.

---

## 🛡️ Enable Secret

The privileged EXEC password is:

```text
cisco123
```

After logging in through Telnet, the user initially reaches:

```text
R0>
```

To enter privileged EXEC mode:

```text
enable
```

After entering the enable secret, the prompt changes to:

```text
R0#
```

---

## 🌐 Connectivity Verification

Before attempting Telnet, basic connectivity was verified from PC0.

Command:

```text
ping 192.168.50.1
```

Successful replies confirmed communication between PC0 and Router0.

Example:

```text
Reply from 192.168.50.1: bytes=32 time<1ms TTL=255
```

The ping test showed:

```text
Sent = 4
Received = 4
Lost = 0
```

---

## 💻 Telnet Connection

From PC0:

```text
telnet 192.168.50.1
```

The router responded with:

```text
Trying 192.168.50.1 ...Open

User Access Verification

Password:
```

After entering:

```text
telnet123
```

the router prompt appeared:

```text
R0>
```

This confirmed successful remote Telnet access.

---

## 🔐 Privileged Remote Access

After successfully connecting through Telnet:

```text
R0>enable
```

The router requested the enable secret.

After entering the correct password:

```text
cisco123
```

the prompt changed to:

```text
R0#
```

This confirms successful privileged EXEC access remotely through Telnet.

---

## ⚠️ Troubleshooting Performed

Initially, the Telnet connection timed out:

```text
% Connection timed out; remote host not responding
```

The cause was that Router0's G0/0 interface was not active.

The interface was enabled using:

```text
interface g0/0
no shutdown
```

After the interface changed to `up/up`, connectivity was verified using ping and Telnet worked successfully.

---

## ⚠️ Password Troubleshooting

During testing, incorrect Telnet passwords were entered multiple times.

After three failed authentication attempts, the router closed the connection:

```text
[Connection to 192.168.50.1 closed by foreign host]
```

A new Telnet session was started and the correct password was entered.

---

## 🧠 How Telnet Works

Telnet allows a user to remotely access the command-line interface of a network device.

In this lab:

```text
PC0
 |
 | Telnet Request
 |
 v
Switch0
 |
 v
Router0
 |
 | Password Authentication
 |
 v
Remote CLI Access
```

Instead of physically connecting a console cable to the router, an administrator can remotely manage the router through the network.

---

## ⚡ Important Telnet Facts

```text
Protocol:      Telnet
Default Port:  TCP 23
Purpose:       Remote CLI Access
Security:      Unencrypted
VTY Lines:     Used for remote sessions
```

Telnet transmits information, including passwords, in plain text.

Because of this, Telnet is generally replaced by SSH in modern production networks.

---

## 🔐 Telnet vs SSH

| Feature | Telnet | SSH |
|---|---|---|
| Port | TCP 23 | TCP 22 |
| Encryption | No | Yes |
| Security | Low | High |
| Remote CLI Access | Yes | Yes |
| Recommended Today | No | Yes |

Telnet is useful for learning remote-access concepts, but SSH is preferred in real networks.

---

## 🔎 Useful Commands

Check interface status:

```text
show ip interface brief
```

Check the running configuration:

```text
show running-config
```

Test connectivity:

```text
ping 192.168.50.1
```

Connect through Telnet:

```text
telnet 192.168.50.1
```

Enter privileged EXEC mode:

```text
enable
```

---

## 📸 Screenshots

### 1. Network Topology

```text
screenshots/01-topology.png
```

Shows the complete Telnet topology:

```text
PC0 → Switch0 → Router0
```

### 2. Router Telnet Configuration

```text
screenshots/02-router-telnet-config.png
```

Shows:

```text
hostname R0
enable secret
line vty 0 4
password telnet123
login
transport input telnet
```

### 3. Ping Test

```text
screenshots/03-ping-test.png
```

Shows successful communication from PC0 to:

```text
192.168.50.1
```

### 4. Telnet Login

```text
screenshots/04-telnet-login.png
```

Shows:

```text
telnet 192.168.50.1
```

followed by successful authentication and the:

```text
R0>
```

prompt.

### 5. Privileged Access

```text
screenshots/05-privileged-access.png
```

Shows:

```text
R0>enable
Password:
R0#
```

confirming privileged remote access.

---

## 📁 Files

```text
Telnet/
│
├── README.md
├── Telnet.pkt
│
└── screenshots/
    ├── 01-topology.png
    ├── 02-router-telnet-config.png
    ├── 03-ping-test.png
    ├── 04-telnet-login.png
    └── 05-privileged-access.png
```

---

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco 2960 Switch
- Telnet
- IPv4
- ICMP
- Cisco IOS CLI

---

## 🏁 Result

Successfully configured **Telnet remote access** on Router0.

PC0 was able to:

```text
Ping Router0
Connect using Telnet
Authenticate using the VTY password
Enter privileged EXEC mode
```

The final remote prompt:

```text
R0#
```

confirmed successful privileged access through Telnet.

This lab demonstrates:

- VTY configuration
- Telnet authentication
- Remote device management
- Enable secret configuration
- Connectivity verification
- Telnet troubleshooting