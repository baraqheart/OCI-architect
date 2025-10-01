### **Study Notes: OCI Public Subnets - Internet-Facing Resources**

---

#### **1. What is a Public Subnet?**

A **Public Subnet** is a subnet configured to allow instances to have **public IP addresses** and **direct internet connectivity**.

*   **Key Feature:** Enables assignment of public IP addresses to instance vNICs
*   **Default Selection:** When creating subnets, "public" is the **default option**
*   **Permanent Decision:** The public/private designation **cannot be changed** after subnet creation

---

#### **2. Public Subnet Architecture Components**

For a functional public subnet, you need this complete configuration:

##### **A. Subnet Configuration:**
*   Subnet created with **"Public Subnet"** option selected
*   This enables public IP assignment capability

##### **B. Instance Configuration:**
*   Instances must have **public IP addresses enabled** on their vNICs
*   Without public IPs, instances cannot communicate with internet even in public subnets

##### **C. Routing Configuration:**
*   Subnet's route table must have:
    ```
    Destination: 0.0.0.0/0 → Target: Internet Gateway
    ```

##### **D. Security Configuration:**
*   **Security Lists** (subnet-level) or **Network Security Groups** (resource-level) must allow desired traffic
*   Common rules: Allow HTTP (80), HTTPS (443) for web servers

---

#### **3. Complete Public Subnet Setup Example**

```
VCN: 10.0.0.0/16
└── Public Subnet: 10.0.1.0/24
    ├── Route Table:
    │   └── 0.0.0.0/0 → Internet Gateway
    ├── Security List:
    │   ├── Ingress: Allow HTTP (80), HTTPS (443), SSH (22)
    │   └── Egress: Allow All
    └── Compute Instances:
        ├── Web Server 1:
        │   ├── Private IP: 10.0.1.10
        │   └── Public IP: 203.0.113.10
        └── Web Server 2:
            ├── Private IP: 10.0.1.11
            └── Public IP: 203.0.113.11
```

**Result:** Both web servers can:
*   Serve web traffic to internet users (inbound)
*   Make outbound connections to internet services
*   Be accessed via SSH for administration

---

#### **4. Public vs Private Subnet Comparison**

| Aspect | Public Subnet | Private Subnet |
|--------|---------------|----------------|
| **Public IP Assignment** | ✅ Allowed | ❌ Not Allowed |
| **Internet Connectivity** | Direct (bidirectional) | Indirect (NAT Gateway) |
| **Internet Initiation** | ✅ Allowed | ❌ Blocked |
| **Typical Use Cases** | Web servers, load balancers | Databases, app servers |
| **Route Table Target** | Internet Gateway | NAT Gateway |

---

#### **5. Implementation Checklist for Public Subnets**

- [ ] Create subnet with **"Public Subnet"** option selected
- [ ] Ensure **Internet Gateway** exists in the VCN
- [ ] Configure **route table** with `0.0.0.0/0 → Internet Gateway`
- [ ] Launch instances with **public IP addresses enabled**
- [ ] Configure **security rules** to allow required traffic
- [ ] Test internet connectivity in both directions

---

#### **6. Common Use Cases for Public Subnets**

*   **Web Servers:** Hosting public websites and applications
*   **Load Balancers:** Distributing incoming internet traffic
*   **Bastion Hosts:** Secure administrative access points
*   **API Gateways:** Public-facing API endpoints
*   **VPN Servers:** Remote access connectivity points

---

#### **7. Security Considerations**

*   **Principle of Least Privilege:** Only open necessary ports (e.g., 80, 443 for web servers)
*   **Network Security Groups:** Use for granular, resource-level security policies
*   **Regular Audits:** Periodically review security rules and public IP assignments
*   **Monitoring:** Implement logging and monitoring for public-facing resources

---

#### **Key Design Principles**

*   **Intentional Design:** Only use public subnets for resources that genuinely need internet exposure
*   **Layered Security:** Combine route tables, security lists, and NSGs for defense in depth
*   **Documentation:** Maintain clear records of why each public subnet exists
*   **Cost Awareness:** Public IPs and internet data transfer may incur additional costs

**Remember:** Public subnets are designed for **internet-facing workloads** that require direct bidirectional communication with the public internet. Always balance functionality with security when deploying resources in public subnets.