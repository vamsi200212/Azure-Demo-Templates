# Azure Firewall Routing Lab 🔥

This project demonstrates how to control internal traffic between subnets in an Azure Virtual Network using **Azure Firewall**. The setup ensures that all traffic from a public subnet VM to a private subnet VM is routed through an Azure Firewall for inspection and control.

---

## 🧱 Architecture Overview

- **Public Subnet**: Contains a VM with outbound access
- **Private Subnet**: Contains a VM with no direct public access
- **AzureFirewallSubnet**: Contains the Azure Firewall instance
- **Route Table**: Routes traffic from Public Subnet to Private Subnet via Firewall
- **NSGs**: Control inbound traffic from the firewall to VMs

---

## 📦 Resources Deployed

- Virtual Network with 3 Subnets:
  - `PublicSubnet`
  - `PrivateSubnet`
  - `AzureFirewallSubnet`
- 2 VMs:
  - `PublicVM` (in PublicSubnet)
  - `PrivateVM` (in PrivateSubnet)
- Azure Firewall (deployed without Public IP)
- Network Security Groups
- Route Tables
- Firewall Policy with network rules

---

## 🔁 Traffic Flow


- Route table in the **Public Subnet** forwards traffic to **Private Subnet CIDR** via **Azure Firewall’s private IP**.
- Azure Firewall applies **Network Rules** to allow traffic.
- NSG on the **Private Subnet** allows traffic **from the firewall** on specific ports (e.g., 22, 3389, etc.).

---

## ✅ Use Cases

- Internal east-west traffic inspection
- Lab/testing for subnet-to-subnet traffic control
- Foundation for more advanced scenarios (e.g., internet egress, forced tunneling)

---

## 🚀 Deployment Options

You can deploy this lab using:

- [ ] ARM Template (exported from Azure)
- [ ] Terraform or Bicep (manual conversion coming soon)
- [ ] Azure Portal (follow steps from `/docs` folder)

---

## 🛡️ Notes

- No Public IP is assigned to the firewall (internal traffic only).
- ICMP (ping) requires allowing **ICMP protocol** in NSG and Firewall policy.
- You can enable logging in Azure Firewall diagnostics for deep inspection.

---

## 👨‍💻 Author

**Teja Sai Vamsi Kanuri**  
Feel free to connect or raise issues/suggestions!
