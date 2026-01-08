# Lab 4 Analysis: Network Gateway and Routing Configuration

## **Lab Overview**

This lab focuses on configuring a virtual network environment with three VMs using VirtualBox. The primary objectives are:
- Setting up a gateway/router VM with multiple network interfaces
- Configuring two internal networks with static IP addressing
- Enabling IP forwarding and NAT masquerading for internet access
- Implementing firewall rules to control traffic flow between networks

## **Network Architecture**

Based on the lab instructions and image analysis, the network topology is as follows:

### **Gateway VM (Ubuntu Server 24.04)**
- **Adapter 1**: NAT (for internet access) - DHCP assigned
- **Adapter 2**: Internal Network `intnet1` - Static IP: `192.168.100.1/24`
- **Adapter 3**: Internal Network `intnet2` - Static IP: `192.168.200.1/24`

### **Web Server VM (Ubuntu Server 22.04)**
- **Adapter 1**: Internal Network `intnet1` - Static IP: `192.168.100.2/24`
- Default gateway: `192.168.100.1`

### **WorkStation VM (Ubuntu Desktop 22.04)**
- **Adapter 1**: Internal Network `intnet2` - Static IP: `192.168.200.2/24`
- Default gateway: `192.168.200.1`
- DNS servers: `8.8.8.8` and `1.1.1.1`

## **Configuration Analysis with Image References**

### **Part 1: Gateway VM Configuration**

**Step 1: Network Adapter Setup**

The gateway VM was configured with three network adapters as specified in the lab. The configuration is visible in several screenshots:

- Initial network adapter configuration shows four adapters available, with Adapter 1 set to NAT:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image2.png)

- Adapter 2 configured for Internal Network `intnet1`:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image3.png)

- Additional adapter configurations show possible variations or additional interfaces:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image4.png)
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image5.png)

**Step 2: Static IP Configuration**

The netplan configuration was modified to assign static IP addresses to the internal interfaces:

- Editing the netplan configuration file:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image8.png)

- Final netplan configuration showing the static IP assignments:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image9.png)

- Verification of IP address assignment using `ip a` command:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image6.png)
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image7.png)

**Note**: The `ip a` output shows an additional interface `enp0s10` with IP `192.168.250.1/24` that is not configured in the netplan file. This suggests either:
1. An older configuration from a previous lab
2. A fourth adapter that was configured manually or via DHCP

### **Part 2: Web Server VM Configuration**

**Step 1: Network Adapter Setup**

- Network settings showing connection to Internal Network `intnet1`:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image10.png)

**Step 2: Static IP Configuration**

- Netplan configuration for the web server:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image11.png)

- Verification of IP address assignment:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image12.png)

### **Part 3: WorkStation VM Configuration**

**Step 1 & 2: Network Adapter and Static IP Configuration**

The workstation was configured using the GUI network manager:

- Network status indicator:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image13.png)

- Network connection details showing IP configuration:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image14.png)

- Network adapter identity information:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image15.png)

- Manual IPv4 configuration settings:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image16.png)

### **Part 5: Enabling IP Forwarding and NAT**

**Step 1: Enabling IP Forwarding**

- Editing the sysctl configuration file:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image17.png)

- Sysctl.conf file with IP forwarding enabled:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image18.png)

- Applying the sysctl changes:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image19.png)

**Step 2: Configuring NAT Masquerading**

- Adding the iptables NAT masquerade rule:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image20.png)

### **Part 6: Additional Firewall Configuration**

**Step 1: Installing iptables-persistent**

- Installing the package to save iptables rules:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image21.png)

**Step 2: Viewing iptables Rules**

- Current iptables rules showing multiple duplicate entries:
  ![](https://github.com/D-rank-developer/Network-Security-Practices/blob/c6593b44cdcfe868f3d2eee39255b48e433ef0a5/Internal%20Network%20Security/VM%20config/image22.png)

## **Analysis of Configuration Against Lab Requirements**

### **Successfully Implemented**

1. **Gateway VM Configuration**: All three network adapters are correctly configured with the specified IP addresses.
2. **Web Server VM**: Successfully connected to `intnet1` with the correct static IP configuration.
3. **WorkStation VM**: Correctly configured on `intnet2` with static IP and gateway settings.
4. **IP Forwarding**: Successfully enabled on the gateway VM.
5. **NAT Masquerading**: Correctly configured for internet access from internal networks.

### **Discrepancies and Observations**

1. **Additional Interface on Gateway**: The gateway VM shows an additional interface `enp0s10` with IP `192.168.250.1/24` that is not part of the lab specification. This interface is active but not configured in the netplan file.

2. **Duplicate iptables Rules**: The iptables FORWARD chain shows multiple identical rules, suggesting the configuration commands were run multiple times.

3. **Web Server Multi-homing**: The web server appears to have two network interfaces configured (based on image11 showing a default route configuration), while the lab only specifies one interface for the web server.

4. **Incomplete Firewall Rules**: The lab specifies detailed iptables rules for forwarding between specific interfaces, but the actual configuration shows generic rules rather than the specific interface-to-interface rules described in Part 6 of the lab.

## **Answers to Lab Questions**

Based on the configuration and images:

**Question 1**: Why did we choose different names for the Internal Networks?
- Different network names are required to create separate broadcast domains and subnets. This allows the gateway to route between different network segments and apply different security policies.

**Question 2**: Why do you think the IP address on enp0s3 did not change?
- enp0s3 is connected to NAT and uses DHCP, so its IP address is assigned by the VirtualBox NAT engine, not by the static configuration in netplan.

**Question 3**: Why did the ping between Web Server and Gateway fail?
- Initial failures would occur because IP forwarding was not yet enabled on the gateway. Once enabled, pings should succeed.

**Question 4**: Why did pinging intnet2 from Web Server succeed?
- This would only succeed after IP forwarding was enabled on the gateway, allowing traffic to pass between the internal networks.

**Question 5**: Internet access on WorkStation browser
- Would only work after both IP forwarding and NAT masquerading were configured on the gateway.

## **Recommendations for Improvement**

1. **Clean up iptables rules**: Remove duplicate rules and implement the specific interface-based forwarding rules as specified in the lab.

2. **Verify netplan configuration**: Ensure all network interfaces are properly configured in the netplan file, including any additional interfaces not specified in the lab.

3. **Test connectivity systematically**: Follow the lab's connectivity testing steps in order to verify each configuration change.

4. **Consider stateful firewall rules**: Implement the stateful forwarding rules mentioned in the optional part of the lab for improved security.

The configuration successfully implements the core requirements of the lab, creating a functional network with a gateway/router providing connectivity between internal networks and internet access via NAT.
