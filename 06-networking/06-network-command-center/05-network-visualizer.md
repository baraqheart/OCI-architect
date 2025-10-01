### **OCI Study Notes: Network Visualizer**

#### **1. The Problem: Complex Network Visualization**

As OCI architectures grow, they become more complex, often including:
*   Multiple VCNs
*   Various gateways (Dynamic Routing Gateway - DRG, Service Gateway, Local Peering Gateway - LPG)
*   Numerous subnets and resources (Compute, Database, Load Balancers, OKE clusters)
*   Connections to on-premises networks

Manually tracking all these components and their interconnections is difficult. The Network Visualizer solves this problem.

#### **2. What is the Network Visualizer?**

*   **Definition:** A service that **automatically generates interactive diagrams** of your implemented network topology.
*   **Scope:** It visualizes the entire network for a selected region and tenancy, regardless of complexity.
*   **Benefit:** Provides a clear, navigable view of your architecture, making it easy to understand resource relationships and dependencies.

#### **3. Hierarchical Visualization Levels**

The tool provides three levels of detail, allowing you to drill down from a high-level overview to specific resource details.

**A. Regional Topology Map**
*   **Purpose:** A **high-level layout** of your entire virtual network in a region.
*   **Shows:** Core connectivity components like **VCNs, Dynamic Routing Gateways (DRG), Customer Premises Equipment (CPE), and various gateways**.

**B. VCN Topology Map**
*   **Purpose:** Zooms in to show the organization of a **single VCN**.
*   **Shows:** The VCN's internal structure, including its **subnets and routing configuration**.

**C. Subnet Topology Map**
*   **Purpose:** The most detailed view, focusing on a **single subnet**.
*   **Shows:** The specific resources within the subnet, such as **compute instances, load balancers, and OKE clusters**.

#### **4. Primary Use Cases**

*   **Visualizing and Troubleshooting Security Configurations:**
    *   See the relationship between **Security Lists and Network Security Groups (NSGs)** and other network resources.
    *   Understand the potential impact of security rule changes before you make them.

*   **Understanding Resource Relationships and Impact:**
    *   Gain deeper insights into how resources are connected.
    *   Visualize dependencies to assess the impact of modifications or failures within the network.

*   **Troubleshooting Transit Routing:**
    *   Get an **enhanced view of Dynamic Routing Gateway (DRG) attachments**.
    *   This makes it easier to identify and fix configuration issues in complex hub-and-spoke or transit routing setups.