# Firewall-Secure-Network-Architecture-project
Firewall &amp; Secure Network Architecture project

🔐 Azure Firewall & Secure Network Architecture Project

📌 Overview

This project demonstrates the design and implementation of a secure Azure virtual network architecture using Azure Firewall, Network Security Groups (NSGs), and User Defined Routes (UDRs).

The goal is to simulate a real-world enterprise setup where:

* All outbound traffic is inspected by a central firewall
* Only explicitly allowed traffic is permitted
* Unauthorized traffic is blocked

⸻

🏗️ Architecture Summary

This deployment includes:

* A Virtual Network (VNet) with multiple subnets
* An Azure Firewall deployed in a dedicated subnet
* A Virtual Machine (VM) for testing connectivity
* A Route Table forcing traffic through the firewall
* Firewall rule collections (NAT, Network, Application)

⸻

🧭 Traffic Flow Design

1. VM initiates outbound request
2. Route Table redirects traffic to Azure Firewall
3. Firewall evaluates rules:
    * Network rules (IP-based)
    * Application rules (FQDN-based)
4. Traffic is:
    * ✅ Allowed (if rule matches)
    * ❌ Denied (if no rule matches)

⸻

🔧 Deployment Steps

1. Virtual Network Setup

* Created VNet with address space: 192.168.0.0/16
* Subnets:
    * AzureFirewallSubnet
    * WorkloadSubnet (192.168.10.0/24)

⸻

2. Azure Firewall Deployment

* Deployed Azure Firewall in dedicated subnet
* Assigned:
    * Public IP (for outbound/inbound NAT)
    * Private IP (used in route table)

⸻

3. Virtual Machine Configuration

* Deployed VM inside WorkloadSubnet
* Enabled:
    * RDP access (for testing)
    * Public IP (initial access only)

⸻

4. Network Security Groups (NSGs)

* Configured inbound rules:
    * Allow RDP (port 3389) from trusted IP
* Default deny rules enforced for other traffic

⸻

5. Firewall Rule Collections

🔹 NAT Rule Collection

* Allows inbound traffic to VM via firewall
* Example:
    * Public IP → Private VM IP (RDP)

⸻

🔹 Network Rule Collection

* IP-based filtering

Example:

Name: AllowHTTP
Protocol: TCP
Source: 192.168.10.0/24
Destination: *
Port: 80

⸻

🔹 Application Rule Collection

* FQDN-based filtering

Example:

Allow access to:
- www.microsoft.com
- time.windows.com

⸻

🛣️ Route Table Configuration (Critical)

A User Defined Route (UDR) was created to force all traffic through the firewall.

Configuration:

Setting	Value
Address Prefix	0.0.0.0/0
Next Hop Type	Virtual Appliance
Next Hop IP	Firewall Private IP

Result:

* All outbound traffic is routed to Azure Firewall
* No direct internet access from VM

⸻

🔍 Validation & Testing

✅ Allowed Traffic

* HTTP (port 80) traffic successfully passes through firewall
* Approved websites accessible

❌ Blocked Traffic

* Non-allowed ports blocked
* Unauthorized destinations denied

⸻

📸 Evidence (Screenshots Included)

This repository contains:

* Firewall rule collections (NAT, Network, Application)
* Route Table showing:
    * Next Hop = Virtual Appliance
* NSG configurations
* Connectivity test results

⸻

🧪 Troubleshooting Summary

Key issues encountered and resolved:

* ❌ Mixed rule types in Network rules → fixed by separating rule types
* ❌ Incorrect use of FQDN in Network rules → moved to Application rules
* ❌ Routing bypassing firewall → fixed using UDR
* ❌ Deployment delays → resolved by waiting for provisioning completion

⸻

📊 Security Improvements Achieved

* Centralized traffic inspection
* Reduced attack surface
* Controlled outbound access
* Enforced least privilege network design

⸻

📘 Key Concepts Demonstrated

* Azure Firewall architecture
* Network vs Application rules
* NAT configuration
* User Defined Routing (UDR)
* NSG + Firewall layered security

⸻

🚀 Lessons Learned

* Always separate rule types (Network vs Application)
* Avoid overly permissive rules (*)
* Validate routing alongside firewall rules
* Allow time for Azure deployments to complete

⸻

📁 Repository Structure

.
├── README.md
├── architecture-diagram.png
├── screenshots/
│   ├── firewall-rules.png
│   ├── route-table.png
│   ├── nsg-rules.png
│   └── connectivity-tests.png
├── troubleshooting-report.md

⸻

✅ Conclusion

This project successfully demonstrates how to:

* Build a secure Azure network
* Enforce traffic inspection using Azure Firewall
* Control access using layered security (NSG + Firewall + Routing)

The final architecture ensures that:

* All traffic is inspected
* Only approved traffic is allowed
* The environment follows cloud security best practices

⸻

📌 Next Improvements (Optional)

* Enable Azure Firewall Threat Intelligence
* Integrate with Azure Monitor & Log Analytics
* Add HTTPS (port 443) filtering
* Implement private endpoints

⸻
