### **Study Notes: CIDR Block Prefixes - Advanced Concepts & Subnetting**

---

#### **1. CIDR Notation Recap**

**CIDR Format:** `A.B.C.D/x`
*   **`A.B.C.D`**: The network address (starting IP of the range).
*   **`/x`**: The network prefix length (number of fixed network bits).

This notation compactly represents a range of IP addresses from a start IP to an end IP.

---

#### **2. Network Size and Prefix Length Relationship**

The prefix length (`/x`) has an **inverse relationship** with the size of the network.

*   **Fundamental Rule:** A **smaller** prefix number means a **larger** network.
*   **Mathematical Proof:**
    *   Number of IPs = 2^(32 - x)
    *   `/16` → 2^(32-16) = 2^16 = **65,536 IP addresses** (Larger Network)
    *   `/24` → 2^(32-24) = 2^8 = **256 IP addresses** (Smaller Network)

**Key Insight:** When comparing CIDR blocks, remember: **/16 > /24** in terms of network size.

---

#### **3. Subnetting: Dividing Larger Networks**

**Subnetting** is the process of breaking a larger CIDR block into smaller, more manageable subnetworks (subnets).

*   **Purpose:** Creates multiple logical networks within a single larger network for better organization and security.
*   **Method:** Achieved by "borrowing" bits from the host portion of the IP address to create more network bits, which increases the prefix length.
*   **Example:** A company might divide a `/16` network into multiple `/24` subnets for different departments (e.g., HR, Engineering, Finance).

---

#### **4. Network Address vs. Host Address**

Within a CIDR block, the IP address is divided into two parts:

*   **Network Address:** The fixed portion defined by the prefix length. All hosts in the same subnet share this identical network address.
*   **Host Address:** The variable portion that identifies a specific device within that network.

**Example for `192.168.1.0/24`:**
*   **Prefix Length:** `/24`
*   **Network Bits:** First **24 bits** (`192.168.1`)
*   **Host Bits:** Last **8 bits** (`.0` to `.255`)
*   **Network Address:** `192.168.1.0`
*   **Usable Host Range:** `192.168.1.1` to `192.168.1.254`
*   **Broadcast Address:** `192.168.1.255`

**Example for `10.0.0.0/16`:**
*   **Prefix Length:** `/16`
*   **Network Bits:** First **16 bits** (`10.0`)
*   **Host Bits:** Last **16 bits** (`.0.0` to `.255.255`)
*   **Network Address:** `10.0.0.0`

---

#### **5. Determining the Network Address with the Logical AND**

The **network address** for any given IP is found by performing a **bitwise logical AND** operation between the IP address and the **subnet mask**.

*   **Logical AND Rule:**
    *   `1 AND 1 = 1`
    *   `1 AND 0 = 0`
    *   `0 AND 1 = 0`
    *   `0 AND 0 = 0`

*   **Subnet Mask:** A 32-bit number where the network bits are set to `1` and the host bits are set to `0`.
    *   A `/24` prefix has a subnet mask of `255.255.255.0`

**Example: Find the network of `192.168.1.2/24`**

1.  **Convert to Binary:**
    *   IP: `192.168.1.2` = `11000000.10101000.00000001.00000010`
    *   Subnet Mask (`/24`): `255.255.255.0` = `11111111.11111111.11111111.00000000`

2.  **Perform Logical AND:**
    ```
    IP:      11000000.10101000.00000001.00000010
    Mask:    11111111.11111111.11111111.00000000
    AND:     11000000.10101000.00000001.00000000
    ```
3.  **Convert Result Back to Decimal:**
    *   `11000000.10101000.00000001.00000000` = `192.168.1.0`

**Conclusion:** The network address for `192.168.1.2/24` is **`192.168.1.0`**.

---

#### **Key Terminology & Facts to Memorize**

*   **Smaller prefix number = Larger network** (e.g., `/16` is larger than `/24`).
*   **Subnetting** divides a large network into smaller subnets.
*   The IP address is split into **Network Bits** (fixed) and **Host Bits** (variable).
*   The **network address** is found by performing a **bitwise AND** between the IP and the subnet mask.
*   The **subnet mask** for a `/24` network is `255.255.255.0`.
*   The first address in a range is the **network address**, and the last is the **broadcast address**; these are not assignable to hosts.