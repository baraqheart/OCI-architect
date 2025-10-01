### **Study Notes: OCI Network Security Groups (NSG) - Granular Firewall Control**

---

#### **1. What are Network Security Groups (NSGs)?**

**Network Security Groups** are virtual firewalls that provide **granular, resource-level security** for your cloud resources.

*   **Scope:** Applied at the **virtual network interface card (vNIC) level**
*   **Function:** Define ingress (inbound) and egress (outbound) security rules
*   **Supported Resources:** Compute instances, load balancers, mount targets, API Gateways, and more
*   **Association:** One NSG can be associated with multiple VNICs; one VNIC can have up to **5 NSGs**

---

#### **2. NSG Rule Components**

##### **A. Ingress Rules (Inbound Traffic)**
*   **Source Type Options:**
    *   **CIDR Block:** Specific IP range (e.g., `10.0.1.0/24`)
    *   **Service:** Oracle service (e.g., Object Storage)
    *   **NSG:** Another Network Security Group
*   **Protocol:** TCP, UDP, ICMP, or protocol number
*   **Source Port Range:** Optional - specific source ports
*   **Destination Port Range:** Optional - specific destination ports

##### **B. Egress Rules (Outbound Traffic)**
*   **Destination Type Options:**
    *   **CIDR Block:** Specific IP range
    *   **Service:** Oracle service
    *   **NSG:** Another Network Security Group
*   **Protocol:** TCP, UDP, ICMP, or protocol number
*   **Source Port Range:** Optional
*   **Destination Port Range:** Optional

---

#### **3. Stateful vs Stateless Rules**

##### **A. Stateful Rules (Recommended)**
*   **Behavior:** Automatically tracks connections and allows response traffic
*   **Example:** If you allow inbound SSH (port 22), the outbound responses are **automatically permitted**
*   **Use Case:** **Most common scenario** - simplifies rule management

##### **B. Stateless Rules**
*   **Behavior:** Does not track connection state - requires explicit rules for both directions
*   **Example:** If you allow inbound SSH, you must **explicitly allow outbound responses**
*   **Use Case:** Specialized scenarios requiring manual control of both directions

**Visual Example:**
```
Stateful Rule:
Ingress: Allow SSH from 0.0.0.0/0 → Response automatically allowed

Stateless Rule:
Ingress: Allow SSH from 0.0.0.0/0
Egress: Must explicitly allow SSH responses
```

---

#### **4. Practical NSG Implementation Example**

**Multi-Tier Application Architecture:**

```
VCN: 10.0.0.0/16
├── Web Tier NSG
│   ├── Ingress: Allow HTTP/HTTPS from 0.0.0.0/0
│   └── Egress: Allow to App-Tier-NSG on port 8080
├── App Tier NSG  
│   ├── Ingress: Allow from Web-Tier-NSG on port 8080
│   └── Egress: Allow to DB-Tier-NSG on port 1521
└── DB Tier NSG
    ├── Ingress: Allow from App-Tier-NSG on port 1521
    └── Egress: Restricted outbound rules
```

**NSG Rule Configuration Examples:**

**Web-Tier-NSG Ingress Rule:**
```
Source Type: CIDR
Source: 0.0.0.0/0
Protocol: TCP
Destination Port: 80,443
Rule Type: Stateful
```

**App-Tier-NSG Ingress Rule:**
```
Source Type: NSG  
Source: Web-Tier-NSG
Protocol: TCP
Destination Port: 8080
Rule Type: Stateful
```

---

#### **5. NSG Association Strategy**

*   **Functional Grouping:** Create NSGs based on application roles
    *   `Web-Server-NSG`, `App-Server-NSG`, `Database-NSG`
*   **Environment-Based:** Create NSGs based on environments
    *   `Production-NSG`, `Development-NSG`, `Testing-NSG`
*   **Combination Approach:** Use both functional and environmental NSGs

**Association Example:**
```
Web Server Instance:
├── VNIC Associations:
│   ├── Web-Server-NSG (functional)
│   └── Production-NSG (environmental)
└── Combined security rules from both NSGs
```

---

#### **6. Advanced NSG Features**

##### **A. NSG as Source/Destination**
*   Allows security rules based on **security group membership**
*   Enables **dynamic security policies** - rules automatically apply to new instances added to NSG
*   **Benefit:** No need to update rules when IP addresses change

##### **B. Service-Based Rules**
*   Reference Oracle services by name instead of IP ranges
*   **Example:** Allow outbound traffic to `oci.phx.objectstorage`
*   **Benefit:** Automatically adapts to service IP changes

---

#### **7. Implementation Best Practices**

*   **Start with Stateful:** Use stateful rules unless you have specific stateless requirements
*   **NSG-Based Rules:** Prefer NSG references over CIDR blocks for internal communications
*   **Least Privilege:** Only allow necessary ports and protocols
*   **Documentation:** Maintain clear naming conventions and rule descriptions
*   **Testing:** Validate rules work as expected before production deployment
*   **Monitoring:** Regularly review and audit NSG configurations

---

#### **Key Advantages of NSGs**

*   **Granular Control:** Security at the individual resource level
*   **Dynamic Membership:** Rules automatically apply to all NSG members
*   **Simplified Management:** Group-based security policies
*   **Flexible Scaling:** Easy to add new instances to existing security groups
*   **Improved Security:** Fine-grained control over traffic flows

**Remember:** NSGs provide the **most granular firewall control** in OCI, allowing you to create security policies based on application function, environment, or any other logical grouping that makes sense for your architecture.