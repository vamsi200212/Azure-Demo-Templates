# 🟦 Azure Load Balancer with Private IIS Web Servers

This project demonstrates how to set up a **Public Load Balancer** in Azure to distribute traffic across **two private Windows VMs** running IIS Web Servers. This is ideal for learning basic Azure networking, VM provisioning, and load balancing.

## ⚙️ Configuration Steps

### 1. Create Private Windows VMs

Provision two Windows Server VMs (`backend-server-1` and `backend-server-2`) in the same subnet. These VMs do not require public IPs.

> 📌 Ensure that the NSG associated with the subnet allows **inbound traffic on port 80** (HTTP).

### 2. Install IIS and Set a Custom Homepage

Use PowerShell inside each VM to install IIS and set a custom `index.html`:

**On `backend-server-1`:**

```powershell
Install-WindowsFeature -Name Web-Server
Set-Content -Path "C:\inetpub\wwwroot\index.html" -Value "<html><body><h1>I'm backend server1</h1></body></html>"
```

**On `backend-server-2`:**

```powershell
Install-WindowsFeature -Name Web-Server
Set-Content -Path "C:\inetpub\wwwroot\index.html" -Value "<html><body><h1>I'm backend server2</h1></body></html>"
```

### 3. Configure the Network Security Group (NSG)

Add an inbound rule in `backend-subnet-nsg`:

- **Name:** Allow-HTTP  
- **Priority:** 100  
- **Protocol:** TCP  
- **Port:** 80  
- **Source:** Any  
- **Destination:** BackendServersSubnet CIDR  
- **Action:** Allow  

### 4. Set Up the Azure Load Balancer

Go to the Azure Portal and create a **Public Load Balancer** with the following configuration:

**Frontend IP Configuration:**

- Create and assign a new Public IP

**Backend Pool:**

- Add `backend-server-1` and `backend-server-2`

**Health Probe:**

- **Protocol:** TCP  
- **Port:** 80  
- **Interval:** 5 seconds  
- **Unhealthy threshold:** 2

**Load Balancing Rule:**

- **Protocol:** TCP  
- **Frontend Port:** 80  
- **Backend Port:** 80  
- **Backend Pool:** Backend VMs  
- **Health Probe:** Use the one created above  
- **Session Persistence:** None

## 🌐 Testing

### Test from inside the VM

```powershell
Invoke-WebRequest http://localhost
```

Expected outputs:

- VM1: I'm backend server1  
- VM2: I'm backend server2

### Test from internet

Open browser and hit:

```
http://<your-load-balancer-public-ip>
```

You should see alternating results when you refresh:
- I'm I'm backend server1  
- I'm I'm backend server1  

## ✅ Outcome

- Public Load Balancer distributes traffic to private IIS servers
- Custom content is served from each VM
- NSG and Health Probes correctly configured

## 🧠 Key Learnings

- NSGs must allow port 80 for Load Balancer to reach the VMs  
- Load Balancer can connect to **private IP** VMs  
- Health Probes ensure healthy VM selection  
- PowerShell can quickly configure IIS and deploy static content
