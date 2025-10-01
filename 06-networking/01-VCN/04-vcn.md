### **Study Notes: OCI Virtual Cloud Network (VCN) - Deep Dive**

---

#### **1. Core Concept of a VCN**

A **Virtual Cloud Network (VCN)** is a software-defined, private network that you create within an OCI region.

*   **Analogy:** Think of a VCN as a private, isolated **auditorium or meeting hall** within your company's larger premises (the OCI region). It provides a controlled, secure space for your cloud resources to communicate.
*   **Scope:** A VCN is a **regional resource**—it exists within a single OCI region but can **span all Availability Domains (ADs)** within that region.
*   **Critical Limitation:** A single VCN **cannot span multiple regions**. Each region requires its own separate VCN.

---

#### **2. VCN CIDR Blocks and IP Addressing**

##### **A. CIDR Block Allocation**
*   **Mandatory:** At least **one IPv4 CIDR block** must be assigned when creating a VCN.
*   **Maximum:** You can assign up to **five non-overlapping IPv4 CIDR blocks** and up to **five non-overlapping IPv6 CIDR blocks** to a single VCN.
*   **Flexibility:** CIDR blocks can be **resized or modified** after the VCN is created.
*   **Size Range:** The VCN's primary CIDR block can range from **/16 to /30**.

##### **B. Reserved IP Addresses in a Subnet**
Within any subnet CIDR range, three IP addresses are **reserved and cannot be assigned to resources**:

*   **First IP:** **Network Address** (e.g., `192.168.0.0` in `192.168.0.0/24`)
*   **Second IP:** **Subnet Default Gateway Address** (e.g., `192.168.0.1`)
*   **Last IP:** **Broadcast Address** (e.g., `192.168.0.255`)

**Example for `192.168.0.0/24`:**
*   **Total IPs:** 256
*   **Usable IPs for Hosts:** 253 (256 - 3 reserved)
*   **Usable Range:** `192.168.0.2` to `192.168.0.254`

##### **C. IPv6 Addressing Options**
*   **Oracle-Allocated Prefix:** Oracle automatically assigns a **/56 prefix**.
*   **Bring Your Own (BYO) Prefix:** You can provide your own IPv6 prefix.

---

#### **3. VCN Architecture and Scope**

**Regional and Multi-AD Span:**
*   A VCN is created within a specific **OCI Region**.
*   In a **multi-AD region**, the VCN automatically spans **all Availability Domains** (e.g., AD1, AD2, AD3).
*   Subnets within the VCN can be created in specific Availability Domains or as regional subnets.

**Visual Representation:**
```
OCI Region US-East (us-ashburn-1)
└── Virtual Cloud Network (VCN: 10.0.0.0/16)
    ├── Availability Domain 1
    │   └── Subnet (10.0.1.0/24)
    ├── Availability Domain 2
    │   └── Subnet (10.0.2.0/24)
    └── Availability Domain 3
        └── Subnet (10.0.3.0/24)
```

---

#### **Key Terminology & Facts to Memorize**

*   A **VCN** is a **regional** construct that can span **multiple Availability Domains** but **cannot cross regions**.
*   **Minimum one CIDR block** is required to create a VCN.
*   **Maximum five IPv4** and **five IPv6** CIDR blocks per VCN (all must be **non-overlapping**).
*   CIDR blocks can be **modified/resized** after VCN creation.
*   VCN CIDR size range: **/16 to /30**.
*   **Three IPs are reserved** in every subnet: **Network Address**, **Default Gateway**, and **Broadcast Address**.
*   For IPv6, Oracle provides a **/56 prefix** if using Oracle-allocated addresses.
*   The **first usable host IP** is always the **third address** in the subnet range.