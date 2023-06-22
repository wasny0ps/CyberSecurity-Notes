# Switchport Security (Port Security)

Attackers’ task is comparatively very easy when they can enter the network they want to attack. Ethernet LANs are very much vulnerable to attack as the switch ports are open to use by default. Various attacks such as Dos attack at layer 2, address spoofing can take place. If the administrator has control over the network then obviously the network is safe. To take total control over the switch ports, the user can use a feature called port-security. If somehow **prevent an unauthorized user to use these ports**, then the security will increase up to a great extent at layer 2. Users can secure a port in two steps: 

- **Limiting the number of MAC addresses** to a single switch port, if more than the limit, MAC addresses are learned from a single port then appropriate action will be taken.
- If unauthorized access is observed, the **traffic should be discarded by using any of the options**, or more appropriately, the user should **generate a log message** so that unauthorized access can be easily observed.

Switches learn MAC addresses when the frame is forwarded through a switch port. By using port security, users can limit the number of MAC addresses that can be learned to a port, set static MAC addresses, and set penalties for that port if it is used by an unauthorized user. Users can either use **restrict**, **shutdown** or **protect** port-security commands.

- **shutdown:** This mode is **mostly preferred** as compared to other modes as it **shut down the port** immediately if unauthorized access is done. It will also **generate a log**, `increment violation counter's value`, and **send an SNMP trap**. This port will remain in a shutdown state until the administrator will perform the “no shutdown” command.
- **restrict:** This mode **drops packets until enough secure MAC addresses are removed to drop below the maximum value**. `It doesn't shutdown the port but block the traffic`. In addition to this, it will **generate a log message**, **increment violation counter's value**, and will also **send an SNMP trap**.
- **protect: ** This mode **drops the packets with unknown source mac addresses until you remove enough secure mac addresses to drop below the maximum value**. What is more, `it doesn't generate a log message and increase the violation counter`.

Besides that we can use **sticky** command when we do switchport security configuration. By using the sticky command, the user **provides static MAC address security without typing the absolute MAC address**. For example, if user provides a maximum limit of 2 then the first 2 MAC addresses learned on that port will be placed in the running configuration. After the 2nd learned Mac address, if the 3rd user wants to access then the appropriate action will be taken according to the violation mode applied.

## Switchport Security Configuration 

Here is my unsafe topology. Let's make it safer.

<img src="" />

# DHCP Security

# VLAN Security

# Dynamic ARP Inspection
