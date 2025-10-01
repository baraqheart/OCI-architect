### **Study Notes: OCI Route Tables - Traffic Direction**

---

#### **1. What is a Route Table?**

A **Route Table** is a networking component that determines how traffic is routed within a VCN and to external destinations. It acts as the "traffic director" for your network.

*   **Primary Function:** Defines paths for network traffic based on destination IP ranges.
*   **Scope:** Associated with **subnets** - all instances in a subnet use the route table assigned to that subnet.

---

#### **2. Types of Route Tables**

##### **A. Default Route Table**
*   **Creation:** Automatically created with every new VCN.
*   **Key Feature:** Contains an **implicit local route** that is not visible in the console.
*   **Local Route Function:** Enables **automatic communication between all subnets** within the same VCN without requiring any manual configuration.

##### **B. Custom Route Tables**
*   **Creation:** Manually created by users to implement specific routing requirements.
*   **Key Feature:** Also contains the **implicit local route** for intra-VCN communication.
*   **Use Case:** Essential for creating **public and private subnets** with different routing behaviors.

---

#### **3. Route Table Structure and Rules**

Route tables consist of rules with two main components:

**Route Rule Format:**
```
Destination CIDR Block    →    Target (Next Hop)
```

**Example: Internet Access Rule**
```
0.0.0.0/0    →    Internet Gateway
```
*   **Destination:** `0.0.0.0/0` (represents all internet traffic)
*   **Target:** `Internet Gateway` (the gateway that provides internet access)

---

#### **4. The Implicit Local Route**

**Critical Concept:** Every route table (both default and custom) contains an **implicit local route** that:

*   **Is NOT visible** in the route table listing in the console
*   **Automatically enables communication** between all subnets within the same VCN
*   **Cannot be modified or deleted**
*   **Eliminates the need** for manual routing rules for intra-VCN communication

**Visual Example:**
```
VCN: 10.0.0.0/16
├── Subnet A: 10.0.1.0/24
├── Subnet B: 10.0.2.0/24
└── Subnet C: 10.0.3.0/24
```
All instances in these subnets can communicate with each other **automatically** due to the implicit local route.

---

#### **5. Common Route Targets**

Route tables can direct traffic to various types of targets:

*   **Internet Gateway (IGW):** For public internet access
*   **NAT Gateway (NGW):** For private outbound internet access
*   **Service Gateway (SGW):** For accessing Oracle services (Object Storage, etc.)
*   **Dynamic Routing Gateway (DRG):** For connecting to on-premises networks or other VCNs
*   **Local Peering Gateway (LPG):** For connecting to other VCNs in the same region
*   **Private IP:** For directing traffic to specific instances within the VCN

---

#### **6. Practical Implementation Example**

**Scenario:** Creating public and private subnets with different routing

**Public Subnet Route Table:**
```
Destination        Target
0.0.0.0/0         Internet Gateway
(Local Implicit)  (Automatic intra-VCN routing)
```

**Private Subnet Route Table:**
```
Destination        Target
0.0.0.0/0         NAT Gateway
(Local Implicit)  (Automatic intra-VCN routing)
```

**Result:**
*   **Public subnet** instances can communicate directly with the internet
*   **Private subnet** instances can access the internet only through NAT Gateway
*   **All instances** can communicate with each other within the VCN

---

#### **Key Terminology & Facts to Memorize**

*   **Default Route Table** is automatically created with every VCN.
*   **All route tables** have an **implicit local route** for intra-VCN communication.
*   The **implicit local route is not visible** in the console but is always active.
*   Route rules consist of **Destination CIDR** and **Target**.
*   Use **custom route tables** to create public/private subnets with different internet access.
*   Common targets: **Internet Gateway, NAT Gateway, Service Gateway, DRG, LPG**.
*   The destination `0.0.0.0/0` represents **all internet traffic**.


### **Study Notes: OCI Route Tables - Advanced Concepts & Routing Logic**

---

#### **1. Route Table Association Rules**

**Critical Association Principles:**
*   **One subnet can be associated with ONLY ONE route table**
*   **One route table can be associated with MULTIPLE subnets**
*   **No overlapping associations:** A subnet cannot be associated with multiple route tables simultaneously

**Visual Example:**
```
Route Tables:
- Default Route Table
- Custom Route Table

Subnets:
- Subnet A → Associated with Default Route Table
- Subnet B → Associated with Custom Route Table
- Subnet C → Associated with Default Route Table  ✅ Valid
- Subnet A → Associated with both Default AND Custom ❌ Invalid
```

---

#### **2. The "Most Specific Rule Wins" Principle**

Route tables use **Longest Prefix Match** algorithm - the most specific CIDR block takes precedence when multiple rules could apply.

**Example Scenario:**
```
Route Table Rules:
1. 0.0.0.0/0        → Internet Gateway
2. 130.61.0.0/16    → Service Gateway  (Object Storage range)
```

**Traffic Decision Logic:**
*   Traffic to `google.com` (`8.8.8.8`) → Matches Rule 1 → Goes to **Internet Gateway**
*   Traffic to Object Storage (`130.61.1.5`) → Matches **BOTH** rules but:
    *   Rule 1: `/0` (very generic)
    *   Rule 2: `/16` (more specific)
    *   **Result:** Rule 2 wins → Goes to **Service Gateway**

**Key Insight:** More specific CIDR blocks (larger prefix numbers like `/24`, `/16`) override more generic ones (like `/0`).

---

#### **3. Route Evaluation and Traffic Handling**

**Route Evaluation Process:**
1.  Check for **exact match** with destination CIDR
2.  Apply **longest prefix match** for overlapping rules
3.  If **no match found** → Traffic is **dropped**

**Critical Behavior:** The implicit local route for intra-VCN communication always takes precedence for traffic within the VCN.

---

#### **4. Practical Route Table Configurations**

##### **Public Subnet Route Table:**
```
Destination CIDR    Target
0.0.0.0/0          Internet Gateway
(Implicit Local)   (Intra-VCN communication)
```
*   **Purpose:** Direct internet access for web servers, load balancers
*   **Result:** Instances can communicate bidirectionally with internet

##### **Private Subnet Route Table:**
```
Destination CIDR    Target
0.0.0.0/0          NAT Gateway
130.61.0.0/16      Service Gateway
(Implicit Local)   (Intra-VCN communication)
```
*   **Purpose:** Secure backend services (databases, app servers)
*   **Result:** 
    *   Internet access via NAT (outbound only)
    *   Object Storage access via Service Gateway (private)
    *   Intra-VCN communication enabled

---

#### **5. Comprehensive Route Target Types**

| Target Type | Purpose | Common Use Cases |
|-------------|---------|------------------|
| **Internet Gateway** | Public internet access | Public web servers, bastion hosts |
| **NAT Gateway** | Private outbound internet | Database patches, software updates |
| **Service Gateway** | Oracle services access | Object Storage, OCI services |
| **Local Peering Gateway** | Same-region VCN connectivity | Multi-VCN architectures |
| **Dynamic Routing Gateway** | On-premises connectivity | Hybrid cloud, VPN connections |
| **Private IP** | Specific instance routing | Application-specific routing |

---

#### **6. Key Observations About Default Route Table**

*   Shows **"0 rules"** in console despite having implicit local route
*   This is **normal behavior** - the local route is not counted in the rules display
*   The local route is **always active** and **cannot be modified or deleted**

---

#### **Real-World Routing Scenario**

**Architecture:**
```
VCN: 10.0.0.0/16
├── Public Subnet (10.0.1.0/24) - Web Servers
└── Private Subnet (10.0.2.0/24) - Database Servers
```

**Public Subnet Route Table:**
```
0.0.0.0/0 → Internet Gateway
```

**Private Subnet Route Table:**
```
0.0.0.0/0          → NAT Gateway
130.61.0.0/16      → Service Gateway
```

**Traffic Flow Examples:**
1.  **Web server** needs to serve public users → Uses **Internet Gateway**
2.  **Database server** needs security updates → Uses **NAT Gateway**
3.  **Database backup** to Object Storage → Uses **Service Gateway**
4.  **Web server** querying database → Uses **implicit local route**

---

#### **Key Routing Principles to Memorize**

*   **One subnet = One route table** (but one route table can serve multiple subnets)
*   **Most specific CIDR wins** in routing decisions
*   **No matching route = Traffic dropped**
*   **All route tables** have implicit local route for intra-VCN communication
*   **Default route table** shows "0 rules" but actually has local route active
*   Choose route targets based on **security requirements** and **access patterns**