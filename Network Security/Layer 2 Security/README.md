# Switchport Security (Port Security)

Attackers’ task is comparatively very easy when they can enter the network they want to attack. Ethernet LANs are very much vulnerable to attack as the switch ports are open to use by default. Various attacks such as Dos attack at layer 2, address spoofing can take place. If the administrator has control over the network then obviously the network is safe. To take total control over the switch ports, the user can use a feature called port-security. If somehow **prevent an unauthorized user to use these ports**, then the security will increase up to a great extent at layer 2. Users can secure a port in two steps: 

- **Limiting the number of MAC addresses** to a single switch port, if more than the limit, MAC addresses are learned from a single port then appropriate action will be taken.
- If unauthorized access is observed, the **traffic should be discarded by using any of the options**, or more appropriately, the user should **generate a log message** so that unauthorized access can be easily observed.

Switches learn MAC addresses when the frame is forwarded through a switch port. By using port security, users can limit the number of MAC addresses that can be learned to a port, set static MAC addresses, and set penalties for that port if it is used by an unauthorized user. Users can either use **restrict**, **shutdown** or **protect** port-security violation commands.

- **shutdown:** This mode is **mostly preferred** as compared to other modes as it **shut down the port** immediately if unauthorized access is done. It will also **generate a log**, `increment violation counter's value`, and **send an SNMP trap**. This port will remain in a shutdown state until the administrator will perform the “no shutdown” command.
- **restrict:** This mode **drops packets until enough secure MAC addresses are removed to drop below the maximum value**. `It doesn't shutdown the port but block the traffic`. In addition to this, it will **generate a log message**, **increment violation counter's value**, and will also **send an SNMP trap**.
- **protect: ** This mode **drops the packets with unknown source mac addresses until you remove enough secure mac addresses to drop below the maximum value**. What is more, `it doesn't generate a log message and increase the violation counter`.

Besides that we can use **sticky** command when we do switchport security configuration. By using the sticky command, the user **provides static MAC address security without typing the absolute MAC address**. For example, if user provides a maximum limit of 2 then the first 2 MAC addresses learned on that port will be placed in the running configuration. After the 2nd learned Mac address, if the 3rd user wants to access then the appropriate action will be taken according to the violation mode applied.

> Note: **The port security will work on access port only to enable port security**, the user first has to make it an access port.

With port security configuration, you can ***prevent MAC Flooding, DHCP Snooping Attacks.***

## Switchport Security Configuration 

Here is my unsafe topology. Let's make it safer.

<p align="center"><img src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/switchport_security_topology.png" /></p>

We will configure four types of switchport security configurations one by one. In the first two interfaces, we will set port security violations as 'protect' and 'restrict'. After then, we will set accept a maximum secure MAC address count is 3 in the port.

```
Switch(config)#int fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-security violation protect
Switch(config-if)#switchport port-security maximum 3
Switch(config-if)#ex
Switch(config)#int fa0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-security violation restrict 
Switch(config-if)#ex
Switch(config-if)#switchport port-security maximum 3
```
In this step, we will set port security violations as 'shutdown' and mac-address as 'sticky' or PC's MAC address. Therefore, we **don't need to define the maximum MAC address count**.

```
Switch(config)#int fa0/3
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport port-security violation shutdown 
Switch(config-if)#switchport port-security mac-address sticky 
Switch(config-if)#ex
Switch(config)#int fa0/4
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport port-security violation shutdown 
Switch(config-if)#switchport port-security mac-address 0001.97ab.a712
```
# DHCP Security

No authentication mechanism is available between DHCP servers and clients. Therefore, any DHCP server newly deployed on a network can allocate IP addresses and other network parameters to DHCP clients. A bogus DHCP server connects to an aggregation switch through a Layer 2 network. When clients connected to the switches apply for IP addresses through DHCP, the bogus DHCP server responds before other servers and assigns IP addresses to the clients, leading to IP address conflict and affecting network services.

### Security Policy 

To defend against the preceding attack, configure the following security policies on a switch:
- **DHCP server validity check:** Configure traffic policies to enable the switch to forward reply packets from only valid DHCP servers.
- **DHCP Snooping:** Configure DHCP snooping and configure valid DHCP server interfaces as trusted interfaces to filter out invalid DHCP servers.

## DHCP Snooping Prevention

There are two options to prevent a DHCP snooping attack. The first alternative is to make a port security configuration in your switch. And another option is setting the DHCP server's interface as **trust**. Thereby, **our switch identifies only trust DHCP server** and the attacker's server will have useless. Let's configure it!

Here is my vulnerable topology that we prevent it. 

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/dhcp_snooping_prevention_topology.png"/></p>

Let's try to get IP from local DHCP server.

<p align="center"><img height="200" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/dhcp_snooping.png"/></p>

The photo clearly shows that developer's pc didn't get IP from company's DHCP server. To block DHCP snooping attack, we can execute this command in our switch's CLI.

```
Switch(config)#ip dhcp snooping vlan 1
Switch(config)#int fa0/2
Switch(config-if)#ip dhcp snooping trust
```
Again, make a request from DHCP server.

<p align="center"><img height="200" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/dhcp_snooping_prevention.png"/></p>

And we cut off snooping attack. Our local DHCP server works correctly. Finally, you can check your interfaces for DHCP snooping prevention.

```
Switch#sh ip dhcp snooping 
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
none
Insertion of option 82 is enabled
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Interface                  Trusted    Rate limit (pps)
-----------------------    -------    ----------------
FastEthernet0/1            no         unlimited       
FastEthernet0/3            no         unlimited       
FastEthernet0/2            yes        unlimited
```

# VLAN Security
When devices are separated into multiple VLANs, it’s easier to prevent a compromised computer from infecting the entire network. Nevertheless, VLANs do come with some unique security risks that MSPs must keep in mind. The most important risk to consider is VLAN hopping.

## VLAN Hopping

In a VLAN hopping attack, a hacker connected to one VLAN gains access to other VLANs that they do not have permission to enter. In a secure VLAN, each computer is connected to one switch access port. Each computer can only send traffic to their specific connected port by accessing a single VLAN. However, with VLAN hopping, an attacker is able to send packets to ports that are not normally accessible, penetrating other VLANs. VLAN hopping can be accomplished in one of two ways:

### Switch Spoofing

With a switch spoofing method, an attacker imitates a trunking switch by using the **VLAN’s tagging and trunking protocol** (Multiple VLAN Registration Protocol, IEEE 802.1Q, or Dynamic Trunking Protocol). By forming a trunk link, the hacker can gain access to traffic from all of the VLANs.

### Switch Spoofing Prevention

Different techniques are used to deal with each type of VLAN hopping attack. To prevent switch spoofing, **disable Dynamic Trunking Protocol** to ensure that ports will not automatically negotiate trunks. You should also **make certain that any port that is not intended to be a trunk is explicitly set up to be an access port**.

Here is my topology. In this configuration, we will set the switch port mode as access and disable DTP with `switchport nonegotiate`command for stop the switch spoofing attack.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/switch_spoofing_prevention_topology.png"></p>

```
Switch(config)#interface range fastEthernet 0/1-2
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport nonegotiate
```
```
Switch(config)#interface range fastEthernet 0/3-4
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport nonegotiate
```
At the end of the configuration, you can go over DTP protocol usage in the network.
```
Switch#show dtp 
Global DTP information
    Sending DTP Hello packets every 30 seconds
    Dynamic Trunk timeout is 300 seconds
    0 interfaces using DTP
```
### Double Tagging

VLAN double tagging exploits 802.1Q tagging, taking advantage of the fact that some switches only remove one 802.1Q tag. In a double tagging attack, **the hacker appends two VLAN tags rather than the usual one**. The outer tag (which belongs to the attack’s own VLAN) is removed, leaving the inner tag of the victim’s VLAN or default native VLAN to be forwarded to the trunk link. When the switch encounters the packet, it sees the second tag and allows the hacker access to the victim’s VLAN.

### Double Tagging Prevention

Double tagging can be **prevented using a three-step process**. First, avoid **putting any hosts on the default VLAN (VLAN 1)**. Second, **be sure that the native VLAN on every trunk port is an unused VLAN ID**. Finally, **enable explicit tagging of the native VLAN for all trunk ports**. Additionally, you must use **port security**, **configure allowed VLANs**, **enable BPDU Guard** for prevent double tagging attack. Here is my example topology.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/double_tagging_prevention_topology.png"></p>

Configure allowed VLANs:
```
Switch(config)#int range fa0/1-2
Switch(config-if-range)#switchport access vlan 20
Switch(config-if-range)#ex
Switch(config)#int range fa0/3-4
Switch(config-if-range)#switchport access vlan 10
```

Enable BPDU Guard:
```
Switch(config)#int range fastEthernet 0/1-4
Switch(config-if-range)#spanning-tree bpduguard enable
```

# ARP Security


## ARP

<p align="center"><img height="350" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/ARP.gif"></p>

Sending IP packets on a multi-access network requires mapping an IP address to an Ethernet MAC address. **Ethernet LANs use ARP to map MAC addresses to IP addresses**. The switching device maintains this mapping in a cache that it consults when forwarding packets to network devices. If the ARP cache does not contain an entry for the destination device, the host (the DHCP client) broadcasts an ARP request for that device's address and stores the response in the cache.

When we connect to a network, ARP requests are made continuously while communicating on the network. The returned answers are stored in the **ARP table**. We can see the ARP table of our computer with the ARP command on the terminal.

<p align="center"><img src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp.png"></p>

When we connect to a network, ARP packets are constantly sent to our computer. The reason for this is that computers on the network, including the gateway on the network, are constantly sending packets. It is checked whether the MAC address of the sent packets and our MAC address match. For example, with the **netdiscover** tool, it detects devices on the same network as us and sends ARP request packets to the specified ip range. Respectively, it means who owns this IP address. If an ARP Reply package comes from the other side, this package contains the MAC address. We can listen for these packets with the wireshark tool.

<p align="center"><img src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/wireshark.png"></p>

When the content of the sent packet is looked at, it seems that the broadcast is broadcast because the MAC address is not known. This request is forwarded to all addresses within the scanning range.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp_packet.png"></p>

If there is a device at the requested address, the device sends its MAC address to the address from which the request is made. In this way, the MAC address of the address is added to the ARP table. In the next request, communication can be provided without sending a broadcast to the network.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp_reply_packet.png"></p>

MAC addresses are uniquely identified as people's identification numbers. In a network we are connected to, we can easily learn the MAC addresses of other connected devices. When we first connect to the Internet, we constantly send packets to the network device that will direct us. These packets we send are routed according to the MAC address since we are on the same network.


## ARP Poisoning (Spoofing)

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp_spoofing.gif"></p>

The purpose of the ARP poisoning attack is to capture the packet by imitating the same MAC address of the packet that goes according to the MAC address on the network. In this way, the packet that needs to go to the router is in the hands of the attacker. When the user looks at the ARP table at a time when there is no ARP poisoning, he will see that the MAC addresses are unique.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp_a.png"></p>

Before starting the poisoning process of the ARP table, we need to activate ip forwarding in order to forward the incoming packets. For IP forwarding to be active, the value of ip_forward must be 1, that is, active. With the command we use, we set the value of 1 in the file.

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```
Attacker's IP Address: 192.168.56.102

Target's IP Address: 192.168.56.1

We will use the **ARPSpoof** tool when performing our attack. Its use is shown in the tool's manual note.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arpspoof.png"></p>

We can start the process by poisoning the ARP table of the target system. Our goal is to continuously send ARP reply packets.

```
sudo arpspoof -i eth0 -t 192.168.56.1 192.168.56.102
```
Now, interception will be done by sending ARP Reply packets continuously.
```
sudo arpspoof -i eth0 -t 192.168.56.102 192.168.56.1
```

The target of ARP poisoning attacks is actually poisoning the MAC address of the gateway and seizing the outgoing incoming packets. When we look at the ARP table of the attacked user, we see that the MAC addresses in the table are the same. Our attack was successful.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/arp_spoofing.png"></p>


## Dynamic ARP Inspection

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/DAI.gif"></p>

Dynamic ARP Inspection (DAI) is the security mechanism that **prevents malicious ARP attacks by rejecting unknown ARP Packets**. ARP attacks can be done as a Man-in-the-Middle Attack by an attacker. In this type of attack, by capturing the traffic between two hosts, attacker poisons the ARP Cache and sends his/her own MAC address as requested ip address. To prevent from such a maniplation, we should **validate IP-MAC matchings**. Dynamic ARP Inspection does this job and validates IP-MAC matchings.

Dynamic ARP Inspection (DAI) uses **DHCP Snooping binding database that is created by DHCP Snooping by listening DHCP Messages between the nodes**. According to the DHCP Snooping binding database, DAI decides what to do. If there is a record in the database about sender’s IP and MAC address then it accepts the ARP Packet. If there is no record in the database, ARP packet is rejected. Instead of using DHCP Snooping, **static IP-MAC mappings** can be also used for this validation process.

### How does DAI work?

Dynamic ARP Inspection (DAI) uses Trust states for interfaces. There are two trust states for interfaces:

- **Trusted**
- **Untrusted**

**If an interface set as Trusted, DAI does not work for this interface**. But if it is an Untrusted, DAI precedures work and the MAC-IP matchings are checked. In other words, if we set an interface as trusted, we do not do a validation on these interfaces. In a network all the interfaces connected to the hosts are configured as Untrusted while the interfaces connected to the switches are configured as Trusted. By doing this, ARP Packets are checked if it is coming from a host device. Because, generally such an attacks can come from a host interface.

## Dynamic ARP Inspection Configuration

To configure Dynamic ARP Inspection on switches, we will use the below simple switch topology. As you can see below, there are two switch connected to each other and there are two PCs are connected to each switch.

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/DAI_topology.png"></p>

AS we have mentined above, we can use DAI for VLANs. Here, we will configure DAI for VLAN 10 only. The hosts in VLAN 10 will be Untrusted. So, ARP packets coming from these interfaces will be checked for IP-MAC validation. The other interfaces will be configured as trusted interfaces. So, let’s start to configure DAI.

### Enabling Dynamic ARP Inspection

To enable ARP Inspection on VLAN 10, we will use “ip arp inspection vlan 10” command globally on a switch.

```
Switch A# configure terminal
Switch A(config)# ip arp inspection vlan 10
```

To set any interfaces as trusted we will use “ip arp inspection trust” command under that interface. On Switch A, we will set FastEthernet 0/1 and FastEthernet 0/3 as Trusted. The remaining ports will be Untrusted by default.

```
Switch A(config)# interface fastethernet 0/1
Switch A(config-if)# ip arp inspection trust
Switch A(config-if)# exit
Switch A(config)# interface fastethernet 0/3
Switch A(config-if)# ip arp inspection trust
```

Now, let’s do the similar configuration on the other switch, on Switch B.

```
Switch B# configure terminal
Switch B(config)# ip arp inspection vlan 10
```

```
Switch B(config)# interface fastethernet 0/1
Switch B(config-if)# ip arp inspection trust
Switch B(config-if)# exit
Switch B(config)# interface fastethernet 0/3
Switch B(config-if)# ip arp inspection trust
```

### DAI Verification

Now, it is time to verification. To verify Dynamic ARP Inspection, we can use the below show commands on switches.

```
Switch A# show ip arp inspection vlan 10

Source Mac Validation      : Disabled

Destination Mac Validation : Disabled

IP Address Validation      : Disabled

Vlan     Configuration    Operation   ACL Match          Static ACL

—-     ————-    ———   ———          ———-

1     Enabled          Active

Vlan     ACL Logging      DHCP Logging

—-     ———–      ————

1     Deny             Deny

 

Switch A# show ip arp inspection interfaces fastethernet 0/1

Interface        Trust State     Rate (pps)

—————  ———–     ———-

Fa0/1            Trusted               None
```
```
Switch A# show ip arp inspection interfaces fastethernet 0/2

Interface        Trust State     Rate (pps)

—————  ———–     ———-

Fa0/2            Untrusted               None
```
```
Switch A# show ip arp inspection statistics vlan 10

Vlan      Forwarded        Dropped     DHCP Drops     ACL Drops

—-      ———        ——-     ———-     ———-

1              2              0              0              0

Vlan   DHCP Permits    ACL Permits   Source MAC Failures

—-   ————    ———–   ——————-

1              2              0                    0

Vlan   Dest MAC Failures   IP Validation Failures

—-   —————–   ———————-

1                  0                        0
```
**_by wasny0ps_**
