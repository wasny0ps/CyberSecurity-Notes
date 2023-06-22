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



# VLAN Security

# Dynamic ARP Inspection
