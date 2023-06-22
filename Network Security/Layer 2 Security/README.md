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

## DHCP Snooping Prevention

There are two options to prevent a DHCP snooping attack. The first alternative is to make a port security configuration in your switch. And another option is setting the DHCP server's interface as **trust**. Thereby, **our switch identifies only trust DHCP server** and the attacker's server will have useless. Let's configure it!

Here is my vulnerable topology that we prevent it. 

<p align="center"><img height="300" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/dhcp_snooping_prevention_topology.png"/></p>

Let's try to get IP from local DHCP server.

<p align="center"><img height="200" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/dhcp_snooping.png"/></p>

The photo clearly shows that developer's pc didn't get IP from company's DHCP server. To block DHCP snooping attack, we can execute this command in our switch's CLI.

```
Switch(config)#ip dhcp snooping
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
When devices are separated into multiple VLANs—often by department—it’s easier to prevent a compromised computer from infecting the entire network. Nevertheless, VLANs do come with some unique security risks that MSPs must keep in mind. The most important risk to consider is VLAN hopping.

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


# Overview of Dynamic ARP Inspection

Dynamic ARP Inspection (DAI) is a security feature that **validates Address Resolution Protocol (ARP) packets in a network**. DAI allows a network administrator to intercept, log, and discard ARP packets with invalid MAC address to IP address bindings. This capability **protects the network from certain “man-in-the-middle” attacks**.

## ARP Cache Poisoning

You can attack hosts, switches, and routers connected to your Layer 2 network by “poisoning” their ARP caches. For example, a malicious user might intercept traffic intended for other hosts on the subnet by poisoning the ARP caches of systems connected to the subnet.

Consider the following configuration:

<p align="center"><img src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/Network%20Security/Layer%202%20Security/src/ARP_cache_poisoning.png"></p>

Hosts HA, HB, and HC are connected to the switch on interfaces A, B and C, all of which are on the same subnet. Their IP and MAC addresses are shown in parentheses; for example, Host HA uses IP address IA and MAC address MA. When HA needs to communicate to HB at the IP Layer, HA broadcasts an ARP request for the MAC address associated with IB. As soon as HB receives the ARP request, the ARP cache on HB is populated with an ARP binding for a host with the IP address IA and a MAC address MA; for example, IP address IA is bound to MAC address MA. When HB responds, the ARP cache on HA is populated with a binding for a host with the IP address IB and a MAC address MB.

Host HC can “poison” the ARP caches of HA and HB by broadcasting forged ARP responses with bindings for a host with an IP address of IA (or IB) and a MAC address of MC. Hosts with poisoned ARP caches use the MAC address MC as the destination MAC address for traffic intended for IA or IB. This means that HC intercepts that traffic. Because HC knows the true MAC addresses associated with IA and IB, HC can forward the intercepted traffic to those hosts using the correct MAC address as the destination. HC has inserted itself into the traffic stream from HA to HB, the classic “man in the middle” attack.
