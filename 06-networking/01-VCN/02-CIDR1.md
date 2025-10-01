### **Study Notes: CIDR Block Prefixes - Networking Fundamentals**

---

#### **1. What is CIDR?**

**CIDR** stands for **Classless Inter-Domain Routing**.

*   **Core Definition:** A method for allocating IP addresses and routing Internet Protocol packets.
*   **Primary Purpose:** To denote a **range of IP addresses** in a more flexible and efficient way than the older classful networking system.
*   **Key Benefit:** Allows for more efficient use of IP address space.

---

#### **2. CIDR Notation**

CIDR notation combines an IP address with a suffix that indicates the number of bits used for the network prefix.

*   **Format:** `A.B.C.D/x`
    *   `A.B.C.D`: The starting IP address of the range.
    *   `/x`: The **prefix length** (a decimal number from 0 to 32), which defines how many bits in the address are fixed for the network.

**Example:**
*   The range `10.0.0.0` to `10.0.255.255` is written in CIDR notation as **`10.0.0.0/16`**.

---

#### **3. Calculating the Number of IP Addresses in a CIDR Block**

The prefix length directly determines the size of the IP address range.

*   **Formula:** `Number of IP Addresses = 2^(32 - x)`
    *   `32` is the total number of bits in an IPv4 address.
    *   `x` is the prefix length from the CIDR notation (`/x`).

**Example Calculation for `10.0.0.0/16`:**
*   Prefix Length `x` = 16
*   Number of Addresses = 2^(32 - 16) = 2^16 = **65,536 IP addresses**

---

#### **4. Understanding the Binary Foundation**

To fully grasp CIDR, you must understand that IP addresses are 32-bit binary numbers, typically represented in the dotted-decimal format for readability.

**Binary to Decimal Conversion (8-bit Octet):**

| Bit Position (from right) | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 | 2^0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Decimal Value** | **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** |

**Example 1: Converting the number `10` to binary**
*   Find the combination of bits that adds up to 10: `8 + 2 = 10`
*   Set the corresponding bits to `1`: bits for 8 and 2 are turned on.
*   **Binary Representation:** `00001010`

**Example 2: Converting the IP address `192.168.0.2` to binary**
*   **`192`:** `128 + 64 = 192` → **`11000000`**
*   **`168`:** `128 + 32 + 8 = 168` → **`10101000`**
*   **`0`:** `0` → **`00000000`**
*   **`2`:** `2` → **`00000010`**
*   **Full Binary IP:** `11000000.10101000.00000000.00000010`

---

#### **5. How the Prefix Length Works**

The prefix length (`/x`) specifies how many **leading bits** in the 32-bit IP address are fixed and define the network. The remaining bits are variable and define the specific hosts within that network.

*   For `10.0.0.0/16`:
    *   The first **16 bits** (the `10.0` portion) are the **network address**.
    *   The last **16 bits** (the `.0.0` to `.255.255` portion) are for **host addresses** within that network.
    *   This creates a single network containing 65,536 possible host addresses.

---

#### **Key Terminology & Facts to Memorize**

*   **CIDR** stands for **Classless Inter-Domain Routing**.
*   The notation `A.B.C.D/x` defines a **range of IP addresses**.
*   The number of usable IP addresses in a block is calculated as **2^(32 - x)**.
*   An IPv4 address is a **32-bit number** divided into four **8-bit octets**.
*   The **prefix length** (`/x`) defines the size of the network; a smaller `x` means a larger network.
*   **Common CIDR Blocks:**
    *   `/32` = 1 IP address
    *   `/24` = 256 IP addresses
    *   `/16` = 65,536 IP addresses
    *   `/8` = 16,777,216 IP addresses