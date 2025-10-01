### **Study Notes: OCI Internet Gateway (IGW)**

---

#### **1. What is an Internet Gateway?**

An **Internet Gateway (IGW)** is a managed gateway that provides **direct, bidirectional connectivity** between your VCN and the public internet.

*   **Managed Service:** Fully managed by OCI - no maintenance, scaling, or availability concerns
*   **Bidirectional:** Supports both inbound (ingress) and outbound (egress) internet traffic
*   **VCN Limit:** **One internet gateway per VCN** (you cannot create multiple IGWs for the same VCN)

---

#### **2. Traffic Direction Terminology**

*   **Ingress Traffic:** Traffic flowing **from the internet INTO your VCN**
*   **Egress Traffic:** Traffic flowing **from your VCN OUT to the internet**

The Internet Gateway supports **both directions** of traffic flow.

---

#### **3. Prerequisites for Internet Gateway Usage**

For an instance to use the Internet Gateway, **ALL** of these conditions must be met:

##### **A. Public Subnet Placement**
*   The instance must be in a **public subnet**
*   Public subnets are designated during creation and cannot be changed later

##### **B. Public IP Address**
*   The instance must have a **public IP address** assigned to its virtual network interface card (vNIC)

##### **C. Route Table Configuration**
*   The subnet must have a route table with a rule:
    ```
    Destination: 0.0.0.0/0 → Target: Internet Gateway
    ```

##### **D. Security Rules**
*   **Security List** or **Network Security Group** rules must **allow the desired traffic**
*   This is a common oversight - proper routing alone is insufficient without security rules

---

#### **4. Complete Configuration Checklist**

For successful internet connectivity via Internet Gateway:

- [ ] **Internet Gateway** created in the VCN
- [ ] Instance deployed in **public subnet**
- [ ] Instance has **public IP address** assigned
- [ ] Subnet's route table has **0.0.0.0/0 → IGW** rule
- [ ] **Security rules** allow desired ingress/egress traffic
- [ ] No conflicting network security rules blocking traffic

---

#### **5. Architecture Example**

```
VCN: 10.0.0.0/16
└── Public Subnet: 10.0.1.0/24
    ├── Route Table:
    │   └── 0.0.0.0/0 → Internet Gateway
    ├── Security List:
    │   ├── Ingress: Allow HTTP (80), HTTPS (443)
    │   └── Egress: Allow All
    └── Compute Instance:
        ├── Private IP: 10.0.1.10
        └── Public IP: 203.0.113.10
```

**Result:** The instance can:
*   Serve web traffic to internet users (ingress)
*   Make outbound connections to internet services (egress)

---

#### **6. Key Limitations and Considerations**

*   **One IGW per VCN:** Plan your network architecture accordingly
*   **Public Subnet Requirement:** Instances in private subnets cannot use IGW directly
*   **Security Responsibility:** While OCI manages the gateway, you are responsible for security rules
*   **Cost:** No additional charge for the gateway itself, but data transfer costs may apply

---

#### **Common Use Cases**

*   **Web servers** serving public websites
*   **Load balancers** accepting internet traffic
*   **Bastion hosts** for administrative access
*   **Any service** requiring direct public internet connectivity

**Remember:** The Internet Gateway provides the **pathway** for internet traffic, but proper **routing** and **security** configurations are equally essential for successful communication.