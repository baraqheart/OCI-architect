### **Study Notes: OCI Network Sources for IP-Based Access Control**

---

#### **1. What are Network Sources?**

A Network Source is an IAM component that acts as a **whitelist of permitted IP addresses**. It is a security mechanism that controls access to OCI resources based on the **originating IP address** of the request.

*   **Analogy:** It functions like a gatekeeper that only allows entry to requests coming from IP addresses you have explicitly approved.
*   **Purpose:** To protect critical resources by restricting access to specific, trusted locations such as corporate offices, specific Virtual Cloud Networks (VCNs), or trusted public IPs.

---

#### **2. How Network Sources Work: A Two-Step Process**

##### **Step 1: Define the List of Trusted IP Addresses**
Create a **Network Source object** and populate it with the specific IP addresses or CIDR blocks you want to allow.

*   **Sources Can Include:**
    *   **Public IP Addresses/Ranges:** (e.g., `203.0.113.1` or `203.0.113.0/24`)
    *   **IP Addresses from your OCI Virtual Cloud Networks (VCNs):** (e.g., `10.0.1.0/24`)
*   **Flexibility:** A single Network Source can contain a long list of IPs from both public and private (VCN) ranges, or just a single IP address.

##### **Step 2: Write a Policy with a Network Condition**
Create an IAM policy that uses the `request.networkSource.name` variable in a `WHERE` clause to check if the request comes from an approved IP.

*   **Policy Syntax:**
    `ALLOW <group> TO <actions> IN <placement> WHERE request.networkSource.name = '<network-source-name>'`

---

#### **3. Practical Example**

**Scenario:** You want to allow your "Database-Admins" group to manage databases, but **only when they are connected to the corporate office network or the office VPC**.

**Step 1: Create the Network Source**
*   **Name:** `Office-Networks`
*   **IP Addresses:**
    *   Corporate Office Public IP: `203.0.113.50/32`
    *   Office VCN CIDR: `10.0.1.0/24`

**Step 2: Write the Conditional Policy**
```plaintext
ALLOW GROUP Database-Admins TO MANAGE databases IN COMPARTMENT Prod
WHERE request.networkSource.name = 'Office-Networks'
```

**Result:** A user in the `Database-Admins` group will only be able to manage databases if their request originates from the IP `203.0.113.50` or from within the `10.0.1.0/24` VCN subnet. If they try to access the database from a coffee shop Wi-Fi (a different IP), the policy condition evaluates to **false** and access is denied.

---

#### **Key Terminology & Facts to Memorize**

*   **Network Sources** are whitelists of approved IP addresses and CIDR blocks.
*   The process is: **1. Create Network Source -> 2. Reference it in a policy's `WHERE` clause**.
*   The critical policy variable is **`request.networkSource.name`**.
*   This mechanism provides a powerful layer of **context-aware security**, ensuring access is granted only from trusted network locations.
*   Use this to enforce access from specific locations like **office networks, VPN endpoints, or trusted cloud subnets**.