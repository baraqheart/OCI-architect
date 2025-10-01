### **OCI Study Notes: Public Load Balancer Architecture & High Availability**

#### **1. Core Definition and Placement**

*   **What it is:** A load balancer that is accessible from the public internet.
*   **Subnet Requirement:** **Must be created within a public subnet.** It cannot be deployed in a private subnet.
*   **IP Address:** It is assigned a **public IP address**, allowing it to directly receive traffic from the internet.
*   **Scope:** The service has a **regional scope**, meaning it is designed to be highly available across an entire OCI region.

#### **2. Subnet Requirements for High Availability (HA)**

The high availability of a public load balancer depends on the type of subnet it is deployed in. A VCN can have two subnet types:

*   **AD-Specific Subnet:** Tied to a single Availability Domain (AD).
*   **Regional Subnet:** Spans across all Availability Domains (ADs) in a region.

The HA requirement for a public load balancer is as follows:

*   **Option 1: Using a Regional Subnet (Preferred for Multi-AD Regions)**
    *   The load balancer service automatically places a **primary (active) load balancer** in one AD and a **standby load balancer** in a different AD within the same regional subnet.
    *   This provides automatic failover if an entire Availability Domain fails.

*   **Option 2: Using AD-Specific Subnets (If no regional subnet exists)**
    *   You must select **two different AD-specific subnets** (e.g., one in AD1 and one in AD2).
    *   The service places the primary load balancer in one subnet and the standby in the other.

#### **3. High Availability (HA) Architecture and Failover**

*   **Load Balancer Pair:** OCI always provisions a load balancer as a pair: an **active (primary)** instance and a **standby (secondary)** instance in a different fault domain (a different AD).
*   **Technical Equivalence:** From a service perspective, **both load balancers are technically equivalent**. You cannot manually designate which one is primary or standby; the service manages this automatically.
*   **Floating Public IP:** The public IP address assigned to the load balancer is a **floating IP**. It is attached to the active load balancer.
*   **Automatic Failover:** If the active load balancer or its entire Availability Domain fails, the **public IP address automatically "floats" or switches over to the standby load balancer**. This process ensures continuous service availability with minimal disruption.

#### **4. IP Address Requirements**

Each load balancer instance in the HA pair requires its own IP addresses:

*   **Private IP Address:** Each load balancer (active and standby) requires **one private IP address** from its host subnet. This is used for internal communication and management.
*   **Public IP Address:** The service assigns a **single floating public IP address** to the pair, which is always associated with the active instance.

#### **5. Visualizing the Architecture**

**Scenario with a Regional Subnet:**
1.  A VCN spans multiple Availability Domains (AD1, AD2, AD3).
2.  A public, **regional subnet** exists within this VCN.
3.  The load balancer service automatically deploys the **active instance** in AD1 and the **standby instance** in AD2 within this regional subnet.
4.  The floating public IP is associated with the active instance in AD1.
5.  **During Normal Operation:** All internet traffic flows to the public IP, is received by the active load balancer in AD1, and is distributed to backend servers.
6.  **During an AD1 Outage:** The floating public IP automatically fails over to the **standby load balancer in AD2**. This standby instance now becomes active and continues to distribute traffic to the backend servers, ensuring service continuity.