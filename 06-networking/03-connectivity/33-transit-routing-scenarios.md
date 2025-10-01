### **OCI Study Notes: Transit Routing & Hub-and-Spoke Topology**

#### **1. The Problem: Multiple Connections Are Inefficient**

*   **Scenario:** Your organization has an on-premises data center and three departments, each with its own VCN (VCN A, B, and C). The on-premises network needs access to all three VCNs.
*   **Inefficient Solution:** Creating separate Site-to-Site VPN or FastConnect connections from on-premises to each individual VCN.
    *   **Consequences:** This leads to a significant **increase in cost** and creates an **administrative overhead** from managing multiple connections.
*   **Business Need:** A way for the on-premises network to communicate with all VCNs through a **single, centralized connection**.

#### **2. The Solution: Transit Routing via a Hub-and-Spoke Topology**

Transit routing solves this by using one VCN as a central hub to route traffic between the on-premises network and all other "spoke" VCNs.

**Core Components:**
*   **Hub VCN:** A single, central VCN that acts as a transit point for all network traffic between on-premises and the spoke VCNs.
*   **Spoke VCNs:** The VCNs for each department (VCN 1, 2, 3) that need to connect to the on-premises network.
*   **Dynamic Routing Gateway (DRG):** A virtual router attached to the Hub VCN. This is the termination point for the **single Site-to-Site VPN or FastConnect connection** from on-premises.
*   **Local Peering Gateways (LPGs):** These are used to create a **Local Peering Connection** between the Hub VCN and each Spoke VCN. This is a private, high-bandwidth connection within the same region.
*   **Key Regional Constraint:** For this architecture to work, the **Hub VCN and all Spoke VCNs must be in the same OCI region**. They can, however, be in different tenancies.

**How Traffic Flows:**
*   Traffic from on-premises to a Spoke VCN travels to the Hub VCN via the DRG, and is then routed to the correct Spoke VCN via the Local Peering Gateway.
*   This is "transit routing" because the traffic transits through the Hub VCN.

#### **3. Detailed Architectural Example & Route Table Configuration**

To make transit routing functional, you must configure route tables to explicitly tell traffic where to go. This is the most critical part of the setup.

**Example Setup:**
*   **On-Premises CIDR:** `172.16.0.0/12`
*   **Hub VCN CIDR:** `10.0.0.0/16`
*   **Spoke VCN 1 CIDR:** `192.168.0.0/24`
*   **Spoke VCN 2 CIDR:** `192.168.1.0/24`

You need to configure **six route tables** for this to work correctly.

**1. Route Table for Spoke VCN 1's Subnet:**
*   **Rule 1:** Destination: `172.16.0.0/12` -> Target: **Local Peering Gateway 1** (the LPG in Spoke VCN 1)
*   **Rule 2:** Destination: `10.0.0.0/16` -> Target: **Local Peering Gateway 1**

**2. Route Table for Spoke VCN 2's Subnet:**
*   **Rule 1:** Destination: `172.16.0.0/12` -> Target: **Local Peering Gateway 2** (the LPG in Spoke VCN 2)
*   **Rule 2:** Destination: `10.0.0.0/16` -> Target: **Local Peering Gateway 2**

**3. Route Table for the Hub VCN's Subnet:**
*   **Rule 1:** Destination: `172.16.0.0/12` -> Target: **Dynamic Routing Gateway**
*   **Rule 2:** Destination: `192.168.0.0/24` -> Target: **Local Peering Gateway Hub 1** (the LPG in the Hub VCN for Spoke 1)
*   **Rule 3:** Destination: `192.168.1.0/24` -> Target: **Local Peering Gateway Hub 2** (the LPG in the Hub VCN for Spoke 2)

**4. VCN Route Table Attached to the DRG:**
*   This is a special route table created in the Hub VCN and then attached to the DRG attachment.
*   **Rule 1:** Destination: `192.168.0.0/24` -> Target: **Local Peering Gateway Hub 1**
*   **Rule 2:** Destination: `192.168.1.0/24` -> Target: **Local Peering Gateway Hub 2**

**5. Route Table for Local Peering Gateway Hub 1:**
*   **Rule:** Destination: `172.16.0.0/12` -> Target: **Dynamic Routing Gateway**

**6. Route Table for Local Peering Gateway Hub 2:**
*   **Rule:** Destination: `172.16.0.0/12` -> Target: **Dynamic Routing Gateway**

#### **4. Console Configuration Steps**

**A. Attaching a VCN Route Table to a DRG:**
1.  Navigate to the **Dynamic Routing Gateway** details page.
2.  Go to **Resources** and select **VCN Attachments**.
3.  Click on the specific VCN attachment and click **Edit**.
4.  Click **Show Advanced Options**.
5.  Under **Route Table**, select the VCN route table you created for the DRG (e.g., the one with routes to the spoke VCNs).
6.  **Note:** This is an advanced feature specifically for setting up transit routing. Once a route table is attached, you cannot set it back to "None".

**B. Attaching a Route Table to a Local Peering Gateway:**
1.  When creating a **Local Peering Gateway**, click **Show Advanced Options**.
2.  In the **Route Table** dropdown, select the route table you created for the LPG (e.g., the one that sends `172.16.0.0/12` traffic to the DRG).
3.  **Critical Validation:** The route table you assign to an LPG must have rules whose target is a **Dynamic Routing Gateway** or a **Private IP**. If you select a route table with an invalid target (like an Internet Gateway), the console will throw an error: "Rules in the route table must use a dynamic routing gateway as a target."