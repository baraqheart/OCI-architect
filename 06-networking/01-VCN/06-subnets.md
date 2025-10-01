### **Study Notes: OCI Subnets - Detailed Concepts**

---

#### **1. Subnet Fundamentals**

A **subnet** is a subdivision of a VCN's IP address range that creates smaller, manageable network segments.

*   **Purpose:** To divide a large VCN network into organized, secure segments.
*   **CIDR Notation:** Subnets use CIDR notation with a larger prefix number than the parent VCN, creating smaller networks.
*   **Example:** A VCN with `10.0.0.0/16` can be divided into subnets:
    *   Subnet A: `10.0.1.0/24`
    *   Subnet B: `10.0.2.0/24`

**Key Rule:** **Smaller prefix number = Larger network** (e.g., `/16` is larger than `/24`).

---

#### **2. Critical Subnet Requirements**

##### **A. Non-Overlapping CIDR Ranges**
All subnets within the same VCN **must have non-overlapping CIDR blocks**.

*   **Overlapping Example:** `10.0.1.0/24` and `10.0.1.128/25` **OVERLAP** because the second range is contained within the first.
*   **Non-Overlapping Example:** `10.0.1.0/24` and `10.0.2.0/24` **DO NOT OVERLAP** and can coexist in the same VCN.

##### **B. CIDR Modification Flexibility**
Subnet CIDR blocks **can be modified** after creation, similar to VCN CIDR blocks.

---

#### **3. Subnet Scope and Availability**

##### **A. Availability Domain (AD) Specific Subnet**
*   **Scope:** Exists within a **single Availability Domain**.
*   **Use Case:** For workloads that require resources to be in specific ADs.
*   **Visual:**
    ```
    Region US-East
    ├── AD1: Subnet A (10.0.1.0/24)
    ├── AD2: Subnet B (10.0.2.0/24)
    └── AD3: Subnet C (10.0.3.0/24)
    ```

##### **B. Regional Subnet (Recommended)**
*   **Scope:** Spans **all Availability Domains** within a region.
*   **Benefit:** Provides high availability and flexibility for resource placement.
*   **Best Practice:** This is the **recommended option** for most use cases.
*   **Note:** In single-AD regions, AD-specific and regional subnets are functionally identical.

---

#### **4. Subnet as a Configuration Unit**

Subnets serve as the **primary unit of configuration** for networking components. Settings applied to a subnet affect **all resources** within that subnet.

**Configurations Applied at Subnet Level:**
*   **Route Table:** Determines how traffic is routed from the subnet.
*   **Security List:** Acts as a subnet-level firewall.
*   **DHCP Options:** Provides automatic network configuration to instances.

**Example:** If the "Default Route Table" is associated with a subnet, **all instances** in that subnet inherit the routing rules from that route table.

---

#### **5. Public vs. Private Subnets**

##### **A. Public Subnet**
*   **IP Assignment:** Instances have **both public and private IP addresses**.
*   **Internet Access:** Can communicate directly with the public internet.
*   **Typical Use:** **Web servers, load balancers** - resources that need to be accessible from the internet.
*   **Requirement:** Must have a route to an **Internet Gateway**.

##### **B. Private Subnet**
*   **IP Assignment:** Instances have **only private IP addresses** (no public IP).
*   **Internet Access:** Cannot be directly accessed from the public internet.
*   **Typical Use:** **Database servers, application servers, backend systems** - resources that should not be exposed to the internet.
*   **Outbound Access:** Can use a **NAT Gateway** for controlled outbound internet access.

##### **C. Critical Design Decision**
*   The choice between public and private **must be made during subnet creation**.
*   This setting **cannot be changed later**.
*   **Planning Tip:** Always consider security requirements when designing your subnet strategy.

---

#### **Key Terminology & Facts to Memorize**

*   Subnets **must have non-overlapping** CIDR ranges within the same VCN.
*   **Regional subnets** (spanning all ADs) are the **recommended best practice**.
*   Subnets are the **configuration unit** for Route Tables, Security Lists, and DHCP Options.
*   **Public subnets** allow instances with public IPs; **private subnets** do not.
*   The **public/private designation is permanent** and must be chosen during creation.
*   Use **public subnets** for internet-facing resources (web servers).
*   Use **private subnets** for backend resources (databases, application servers).



### **Study Notes: OCI Subnets - Practical Implementation**

---

#### **1. Visualizing Subnet Architecture**

**Regional Deployment with Multiple Subnet Types:**

```
OCI Region (e.g., us-ashburn-1)
└── Virtual Cloud Network (VCN: 10.0.0.0/16)
    ├── Availability Domain 1 (AD1)
    │   └── Subnet A (AD-Specific): 10.0.1.0/24
    ├── Availability Domain 2 (AD2) 
    │   └── Subnet B (AD-Specific): 10.0.2.0/24
    ├── Availability Domain 3 (AD3)
    │   └── Subnet C (AD-Specific): 10.0.3.0/24
    └── Subnet D (Regional): 10.0.4.0/24
        ├── Spans AD1, AD2, and AD3
        └── Recommended for high availability
```

**Key Architecture Points:**
*   A VCN **spans all Availability Domains** in its region
*   **AD-Specific Subnets** are confined to a single Availability Domain
*   **Regional Subnets** span across all Availability Domains in the region
*   All subnets must have **non-overlapping CIDR blocks** within the parent VCN

---

#### **2. Subnet Creation Configuration Parameters**

When creating a subnet in OCI, you must configure several critical parameters that define the subnet's behavior and security posture:

**Mandatory Configuration Options:**

*   **DHCP Options:** Determines automatic network configuration (DNS, domain name) for all instances in the subnet
*   **Subnet Access Type:** **Public vs. Private** - this is an **irreversible decision**
*   **Route Table:** Defines how traffic is routed from the subnet (internet gateway, NAT gateway, etc.)
*   **Security List:** Acts as the subnet-level firewall controlling inbound/outbound traffic

**Critical Design Principle:** The subnet serves as the **"unit of configuration"** - all instances within the subnet inherit these network settings.

---

#### **3. Public vs. Private Subnet Decision Framework**

##### **Public Subnet Characteristics:**
*   **IP Configuration:** Instances can have **both public and private IPs**
*   **Internet Connectivity:** Direct bidirectional internet access
*   **Typical Workloads:**
    *   Web servers
    *   Load balancers
    *   Bastion hosts
    *   Any service requiring direct internet accessibility

##### **Private Subnet Characteristics:**
*   **IP Configuration:** Instances have **private IPs only**
*   **Internet Connectivity:** No direct internet access (can use NAT Gateway for outbound)
*   **Typical Workloads:**
    *   Database servers
    *   Application servers
    *   Backend services
    *   Any resource that should not be exposed to the internet

##### **Decision-Making Critical:**
*   The **public/private designation is permanent**
*   **Cannot be modified after subnet creation**
*   **Strategic planning** is essential during architecture design phase

---

#### **4. Operational Best Practices**

*   **Start with Regional Subnets:** Use regional subnets as the **default choice** for most workloads unless you have specific AD-affinity requirements
*   **Plan CIDR Carefully:** Design your IP addressing scheme to allow for future growth and avoid overlaps
*   **Consistent Naming:** Use descriptive names that indicate subnet purpose and type (e.g., `web-public-regional`, `db-private-ad1`)
*   **Document Decisions:** Maintain architecture documentation explaining why each subnet was designed as public or private
*   **Security-First Mindset:** Default to private subnets and only use public subnets when internet exposure is explicitly required

---

#### **Key Implementation Checklist**

- [ ] **CIDR Planning:** Ensure non-overlapping ranges within VCN
- [ ] **Scope Decision:** Choose between AD-specific or regional subnet
- [ ] **Access Type:** Make permanent public/private decision
- [ ] **Route Table:** Assign appropriate routing (IGW for public, NGW for private)
- [ ] **Security List:** Configure firewall rules for the intended workload
- [ ] **DHCP Options:** Set appropriate DNS and domain settings
- [ ] **Naming Convention:** Use descriptive names for manageability

**Remember:** The subnet configuration decisions made during creation will have long-lasting impacts on your network security, availability, and management overhead.