# Switch Security Steps

Before enter the aspects detailly, let's sum up titles about switch security and explain them shortly. Here is a full list of switch security to do:

- **Access Control:** Limiting access to switches is a fundamental security measure. It involves ***setting up strong passwords*** or ***using authentication methods like RADIUS*** (Remote Authentication Dial-In User Service) to control who can access and configure the switch.
- **VLAN Segmentation:** Virtual LAN (VLAN) segmentation involves ***dividing a network into smaller logical segments***, ***each isolated from others***. This helps restrict the communication between devices in different VLANs, ***minimizing the potential for unauthorized access*** and lateral movement of threats.
- **Port Security:** Implementing port security features ***allows administrators to control which devices are allowed to connect to switch ports***. Techniques like ***MAC address filtering***, ***limiting the number of MAC addresses per port***, and using ***sticky MAC addresses*** help prevent unauthorized devices from gaining access to the network.
- **Secure Management Protocols:** Switches typically support various management protocols like SNMP (Simple Network Management Protocol), SSH (Secure Shell), and HTTPS (Hypertext Transfer Protocol Secure) for remote configuration and monitoring. ***Enabling secure versions of these protocols*** and ***using strong encryption mechanisms*** helps protect sensitive management traffic from eavesdropping or tampering.
- **Firmware Updates:** Regularly **updating switch firmware is essential to patch security vulnerabilities** and ensure that the latest security enhancements are in place. This reduces the risk of exploitation by attackers who target known vulnerabilities.
- **Logging and Monitoring:** Enabling logging and monitoring features ***allows network administrators to capture and analyze switch activity, including security events***. Monitoring can help ***identify suspicious activities***, ***detect intrusion attempts***, and provide an audit trail for forensic investigations.
- **Defense Against DoS Attacks:** Switches can be targeted by Denial-of-Service (DoS) attacks, where an attacker floods the switch with an overwhelming amount of traffic, causing it to become unresponsive. ***Implementing features like rate limiting, traffic prioritization, and storm control helps mitigate the impact of such attacks***.
- **Physical Security:** Protecting switches physically is as important as securing them digitally. Physical security measures include ***securing switch cabinets or racks in locked rooms***, ***controlling access to switch locations***, and ***monitoring environmental factors like temperature and humidity***.


## Preparation
