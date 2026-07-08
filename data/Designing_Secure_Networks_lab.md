# **CYS 530—WEEK 2—LAB**

## **DESIGNING SECURE NETWORKS**

# **Assigning Devices to VLANs Based on Their Roles**

## **1\. Executive Summary & Security Scenario**

In an unsegmented flat network, all devices reside in a single broadcast domain (typically the default **VLAN 1**). If an attacker compromises a high-risk or untrusted device—such as a visitor's laptop on a guest network—they can easily perform network reconnaissance, intercept sensitive traffic, and move laterally to critical internal assets (e.g., HR databases or network management interfaces).  
To mitigate this architectural vulnerability, this lab demonstrates how to apply the **Principle of Least Privilege** at Layer 2\. By implementing Virtual Local Area Networks (VLANs), we partition a single physical switch into multiple isolated logical broadcast domains aligned with organizational roles.

## **2\. Lab Topology & Address Allocation Table**

The following design isolates four distinct corporate roles across a Cisco Layer 2 Switch (Switch-01):

| Operational Role | VLAN ID | Subnet Segment | Target Switch Ports | Security Policy / Description |
| ----- | ----- | ----- | ----- | ----- |
| **Finance / HR** | 10 | 192.168.10.0/24 | FastEthernet 0/1 \- 0/5 | Highly restricted data. Critical HR & Finance access only. |
| **Engineering** | 20 | 192.168.20.0/24 | FastEthernet 0/6 \- 0/10 | High-bandwidth development environment. |
| **Guest Network** | 30 | 192.168.30.0/24 | FastEthernet 0/11 \- 0/15 | Untrusted Internet-only access. Strictly isolated from corporate segments. |
| **Management SVI** | 99 | 192.168.99.0/24 | — *(Virtual)* | Dedicated out-of-band management interface for SSH/HTTPS access. |
| **Unused / Native** | 100 | — *(Unrouted)* | GigabitEthernet 0/1 \- 0/2 | Used solely to terminate untagged native trunk traffic securely. |

## **3\. Step-by-Step Lab Execution**

Follow these precise steps to configure the switch, assign ports, establish secure trunks, and verify the network architecture.

### **Task 1: Create and Name the Role-Based VLANs**

First, we must define the VLANs in the switch's local database. Unnamed VLANs are difficult to audit, so descriptive, standard naming is critical.

`! Access Global Configuration Mode`  
`Switch-01# configure terminal`

`! Create and name VLAN 10 (Finance/HR)`  
`Switch-01(config)# vlan 10`  
`Switch-01(config-vlan)# name Finance_HR`  
`Switch-01(config-vlan)# exit`

`! Create and name VLAN 20 (Engineering)`  
`Switch-01(config)# vlan 20`  
`Switch-01(config-vlan)# name Engineering`  
`Switch-01(config-vlan)# exit`

`! Create and name VLAN 30 (Guest)`  
`Switch-01(config)# vlan 30`  
`Switch-01(config-vlan)# name Guest_Untrusted`  
`Switch-01(config-vlan)# exit`

`! Create and name VLAN 99 (Management)`  
`Switch-01(config)# vlan 99`  
`Switch-01(config-vlan)# name Network_Management`  
`Switch-01(config-vlan)# exit`

`! Create and name VLAN 100 (Secure Native)`  
`Switch-01(config)# vlan 100`  
`Switch-01(config-vlan)# name Secure_Native`  
`Switch-01(config-vlan)# exit`

### **Task 2: Assign Switch Access Ports to Roles**

By default, all physical interfaces on a Cisco switch belong to VLAN 1\. We must explicitly convert host-facing ports to **access mode** and map them to their corresponding VLAN.

#### **A. Configure Finance & HR Interfaces (Fa0/1 – Fa0/5)**

`Switch-01(config)# interface range FastEthernet 0/1 - 5`  
`Switch-01(config-if-range)# description HOST_ACCESS_FINANCE_HR`  
`Switch-01(config-if-range)# switchport mode access`  
`Switch-01(config-if-range)# switchport access vlan 10`  
`Switch-01(config-if-range)# exit`

#### **B. Configure Engineering Interfaces (Fa0/6 – Fa0/10)**

`Switch-01(config)# interface range FastEthernet 0/6 - 10`  
`Switch-01(config-if-range)# description HOST_ACCESS_ENGINEERING`  
`Switch-01(config-if-range)# switchport mode access`  
`Switch-01(config-if-range)# switchport access vlan 20`  
`Switch-01(config-if-range)# exit`

#### **C. Configure Guest/Untrusted Interfaces (Fa0/11 – Fa0/15)**

`Switch-01(config)# interface range FastEthernet 0/11 - 15`  
`Switch-01(config-if-range)# description HOST_ACCESS_GUEST_UNTRUSTED`  
`Switch-01(config-if-range)# switchport mode access`  
`Switch-01(config-if-range)# switchport access vlan 30`  
`Switch-01(config-if-range)# exit`

### **Task 3: Secure the Out-of-Band Management Interface**

Using the default VLAN 1 for administrative access is an unsafe configuration. Instead, assign an IP address to the Switch Virtual Interface (SVI) of your dedicated Management VLAN (VLAN 99).

`! Configure Management SVI IP address`  
`Switch-01(config)# interface vlan 99`  
`Switch-01(config-if)# description SWITCH_MANAGEMENT_IP`  
`Switch-01(config-if)# ip address 192.168.99.2 255.255.255.0`  
`Switch-01(config-if)# no shutdown`  
`Switch-01(config-if)# exit`

`! Set the default gateway to allow remote management from other subnets`  
`Switch-01(config)# ip default-gateway 192.168.99.1`

### **Task 4: Configure Secure Trunking & Pruning (Uplinks)**

To pass multi-VLAN traffic to downstream switches or routers, we establish 802.1Q trunk links. For security, we alter the default native VLAN from 1 to 100 to mitigate **VLAN Double-Tagging hopping attacks**, and restrict allowed VLANs to only active segments.

`! Access the uplink interfaces (e.g., Gigabit links connected to core routers/switches)`  
`Switch-01(config)# interface range GigabitEthernet 0/1 - 2`  
`Switch-01(config-if-range)# description INTER_SWITCH_TRUNK`  
`Switch-01(config-if-range)# switchport mode trunk`

`! Assign an unused, unrouted Native VLAN to prevent VLAN hopping`  
`Switch-01(config-if-range)# switchport trunk native vlan 100`

`! Security Best Practice: Restrict allowed VLAN traffic to explicitly defined IDs (Pruning)`  
`Switch-01(config-if-range)# switchport trunk allowed vlan 10,20,30,99`  
`Switch-01(config-if-range)# exit`

### **Task 5: Blackhole Unused Switchports**

Unused, enabled ports present a significant physical intrusion risk. Any physical access port not assigned to an active role should be locked down and assigned to an isolated blackhole VLAN.

`! Create an isolated blackhole VLAN`  
`Switch-01(config)# vlan 666`  
`Switch-01(config-vlan)# name Blackhole_Unused`  
`Switch-01(config-vlan)# exit`

`! Assign all remaining unused interfaces, disable them, and move them to the blackhole VLAN`  
`Switch-01(config)# interface range FastEthernet 0/16 - 24`  
`Switch-01(config-if-range)# description UNUSED_DISABLED_PORT`  
`Switch-01(config-if-range)# switchport mode access`  
`Switch-01(config-if-range)# switchport access vlan 666`  
`Switch-01(config-if-range)# shutdown`  
`Switch-01(config-if-range)# exit`

## **4\. Verification and Security Auditing Command Suite**

Verify your configurations using the following fundamental diagnostic commands:

### **Command A: Verify Port-to-VLAN Mapping**

Run show vlan brief to ensure host ports are mapped to their corresponding target roles and that VLAN 1 contains only the minimal default configuration.

| `VLAN` | `Name` | `Status` | `Ports` |
| :---- | :---- | :---- | :---- |
| `1` | `default` | `active` |  |
| `10` | `Finance_HR` | `active` | `Fa0/1, Fa0/2, Fa0/3, Fa0/4, Fa0/5` |
| `20` | `Engineering` | `active` | `Fa0/6, Fa0/7, Fa0/8, Fa0/9, Fa0/10` |
| `30` | `Guest_Untrusted` | `active` | `Fa0/11, Fa0/12, Fa0/13, Fa0/14, Fa0/15` |
| `99` | `Network_Management` | `active` |  |
| `100` | `Secure_Native` | `active` |  |
| `666` | `Blackhole_Unused` | `active` | `Fa0/16, Fa0/17, Fa0/18, Fa0/19...` |

### **Command B: Verify Secure Trunking Status**

Ensure your inter-switch links are trunking correctly, showing the customized native VLAN and the pruned allowed list.

`Switch-01# show interfaces trunk`

`Port   Mode  Encapsulation  Status    Native vlan`  
`Gi0/1  on    802.1q         trunking  100`  
`Gi0/2  on    802.1q         trunking  100`

`Port   Vlans allowed on trunk`  
`Gi0/1  10,20,30,99`  
`Gi0/2  10,20,30,99`

### **Command C: Verify Layer 2 Domain Isolation (Ping Test)**

To verify your security boundaries before enabling a Layer 3 Router/Firewall:

1. Configure a host on Fa0/1 (Finance) with the IP 192.168.10.10.  
2. Configure a host on Fa0/6 (Engineering) with the IP 192.168.20.10.  
3. Configure a host on Fa0/2 (Finance) with the IP 192.168.10.11.  
4. Run a ping from host 192.168.10.10 to 192.168.10.11. This ping **must succeed** (same broadcast domain).  
5. Run a ping from host 192.168.10.10 to 192.168.20.10. This ping **must fail** (isolated broadcast domains).

Once confirmed, Layer 2 separation is successfully enforced. Any inter-VLAN communication will now require a Router/Firewall interface, where stateful security policies can inspect and control the transit of packets.  
