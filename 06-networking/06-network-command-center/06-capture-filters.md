### **OCI Study Notes: Capture Filters**

#### **1. Purpose and Context**

*   **What it is:** A **capture filter** is a set of rules that defines **which specific network traffic** should be captured.
*   **Where it's Used:** Capture filters are used with two Network Command Center services:
    *   **Flow Logs**
    *   **Virtual Test Access Points (VTAP)**
*   **Why it's Needed:** Capturing *all* network traffic can cause performance issues and generate excessive data. Capture filters allow you to **selectively capture only the traffic you need** for monitoring, security analysis, or troubleshooting.

#### **2. Key Characteristics and Rules**

*   **Mandatory Association:** A VTAP **must** have a capture filter associated with it.
*   **Rule Limits:** Each capture filter must have **at least one rule** and can contain **up to 10 rules**.
*   **Rule Actions:** Each rule can be set to either **Include** or **Exclude** matching traffic.
*   **Filtering Criteria:** Rules can be defined based on:
    *   **Source and Destination CIDR** (IP address blocks)
    *   **IP Protocol** (e.g., TCP, UDP, ICMP)
    *   **Source and Destination Ports**
    *   **Traffic Direction** (Ingress or Egress)

#### **3. Critical: Rule Processing Order**

The order of rules in a capture filter is **critically important** because they are processed sequentially.

*   **Process:** The filter evaluates each packet against the rules **in the order they are listed**.
*   **First Match Applies:** As soon as a packet matches a rule, the corresponding action (Include/Exclude) is taken, and **the remaining rules are skipped for that packet**.
*   **Implication:** Changing the order of the rules can completely change the behavior of the filter.

#### **4. Practical Example and Rule Order Impact**

**Goal:** Capture all traffic from the `10.1.0.0/16` network **except** traffic from the specific IP `10.1.1.1`.

**Correct Rule Order:**
1.  **Rule 1:** Source CIDR = `10.1.1.1/32`, Action = **Exclude**
2.  **Rule 2:** Source CIDR = `10.1.0.0/16`, Action = **Include**

**How it Works:**
*   A packet from `10.1.1.1` matches Rule 1, is **excluded**, and no further rules are checked.
*   A packet from `10.1.1.2` does not match Rule 1, so it is checked against Rule 2. It matches and is **included**.

**Incorrect Rule Order:**
1.  **Rule 1:** Source CIDR = `10.1.0.0/16`, Action = **Include**
2.  **Rule 2:** Source CIDR = `10.1.1.1/32`, Action = **Exclude**

**How it Fails:**
*   A packet from `10.1.1.1` matches Rule 1 first, is **included**, and Rule 2 is never evaluated. The exclusion intent is bypassed.

#### **5. Architecture Example: Filtering Web Traffic**

*   **Scenario:** A Load Balancer distributes web traffic to backend servers in Subnet A.
*   **Goal:** Use a VTAP to capture only suspicious web traffic from the internet for analysis.
*   **VTAP Setup:**
    *   **Source:** The public Load Balancer.
    *   **Target:** A Network Load Balancer leading to a security analysis tool.
    *   **Capture Filter Rule:**
        *   **Traffic Direction:** Ingress
        *   **Source CIDR:** `0.0.0.0/0` (represents the entire internet)
        *   **Destination CIDR:** Subnet A's CIDR block
        *   **IP Protocol:** TCP
        *   **Destination Port:** `80` (HTTP)
*   **Result:** Only inbound HTTP traffic from the internet to the web servers is mirrored for analysis.