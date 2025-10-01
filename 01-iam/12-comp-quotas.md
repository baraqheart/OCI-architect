### **Study Notes: OCI Network Sources for IP-Based Access Control**

---

#### **1. Core Concept and Purpose**

**Network Sources** provide IP-based access control in OCI, functioning as a configurable whitelist of approved source IP addresses. This mechanism ensures that access to cloud resources is granted only when requests originate from predefined, trusted network locations.

*   **Security Function:** Acts as a network-level gatekeeper
*   **Primary Use Cases:** Restricting access to corporate offices, specific VCNs, VPN endpoints, or trusted public IP ranges
*   **Operational Benefit:** Adds a critical layer of context-aware security beyond standard identity-based permissions

---

#### **2. Implementation Methodology: Two-Phase Process**

##### **Phase 1: Network Source Definition**
Create and configure Network Source objects containing specific IP addresses and CIDR ranges:

*   **Supported IP Types:**
    *   **Public IP addresses/ranges** (IPv4 and IPv6)
    *   **Private IP ranges from OCI Virtual Cloud Networks (VCNs)**
*   **Configuration Flexibility:**
    *   Single IP addresses (e.g., `203.0.113.50/32`)
    *   IP ranges using CIDR notation (e.g., `203.0.113.0/24`, `10.0.1.0/24`)
    *   Mixed public and private IP addresses in the same Network Source
*   **Naming Convention:** Use descriptive names that identify the purpose or origin of the IP ranges

##### **Phase 2: Policy Integration**
Reference the created Network Sources in IAM policies using conditional statements:

*   **Policy Variable:** `request.networkSource.name`
*   **Syntax Pattern:**
    ```plaintext
    ALLOW [SUBJECT] TO [ACTIONS] IN [PLACEMENT] 
    WHERE request.networkSource.name = '[NETWORK-SOURCE-NAME]'
    ```

---

#### **3. Comprehensive Implementation Example**

**Business Requirement:** Database administrators should only manage production databases when connected through secure corporate networks.

**Implementation Steps:**

**Step 1: Network Source Configuration**
*   **Resource Name:** `Corporate-Secure-Networks`
*   **Approved IP Ranges:**
    *   Corporate HQ Public IP: `203.0.113.50/32`
    *   Backup Site Public Range: `198.51.100.0/28`
    *   Primary VCN Subnet: `10.0.1.0/24`
    *   DR VCN Subnet: `10.1.1.0/24`

**Step 2: Conditional Policy Creation**
```plaintext
ALLOW GROUP Prod-DB-Admins TO MANAGE autonomous-database IN COMPARTMENT Production
WHERE request.networkSource.name = 'Corporate-Secure-Networks'
```

**Security Outcome:**
*   **Access Granted:** Database administrators can manage databases only when connecting from corporate HQ, backup site, or approved VCN subnets
*   **Access Denied:** All connection attempts from unauthorized locations (home networks, public WiFi, etc.) are automatically blocked
*   **Compliance:** Ensures database management occurs only through secure, monitored network paths

---

#### **4. Operational Advantages and Best Practices**

*   **Dynamic Security:** Network-level restrictions work independently of user authentication
*   **Centralized Management:** Single Network Source can be referenced across multiple policies
*   **Audit Trail:** Clear visibility into which network paths are authorized for specific operations
*   **Scalability:** Easily modify IP ranges in Network Source without changing individual policies

**Implementation Recommendations:**
1.  Create separate Network Sources for different security zones (e.g., `Corporate-Networks`, `Partner-Access`, `VPN-Users`)
2.  Use descriptive naming conventions that identify the purpose and scope of each Network Source
3.  Regularly review and update IP ranges to maintain security posture
4.  Combine with other conditional policy elements (time-based restrictions, MFA requirements) for defense-in-depth

---

#### **Key Technical Specifications**

*   **Policy Variable:** `request.networkSource.name`
*   **IP Format Support:** IPv4 and IPv6 addresses with CIDR notation
*   **Scope:** Applies to all OCI services that support IAM policies
*   **Evaluation:** Real-time IP validation against defined Network Sources
*   **Hierarchy:** Network restrictions are evaluated after authentication but before resource authorization