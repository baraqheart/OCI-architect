### **OCI Study Notes: Site-to-Site VPN**

#### **1. What is Site-to-Site VPN?**

*   **Purpose:** To create a secure, encrypted connection between your on-premises data center and an OCI Virtual Cloud Network (VCN).
*   **Legacy Names:** This service was formerly known as "VPN Connect" or "IPsec VPN." These terms refer to the same thing.

#### **2. How It Works: The High-Level Architecture**

The connection involves four key components:
1.  **On-Premises Network:** Your physical data center.
2.  **Customer Premises Equipment (CPE):** A physical or software-based device in your on-premises network (e.g., a router/firewall).
3.  **Dynamic Routing Gateway (DRG):** A virtual router in OCI that provides a path for traffic between your VCN and your on-premises network.
4.  **Virtual Cloud Network (VCN):** Your privately defined network in OCI.

The **IPsec connection** is established between the on-premises CPE and the OCI DRG. "IPsec" means that all IP packets are encrypted before being sent and decrypted upon arrival.

*   **Important:** This encrypted communication travels over the **public internet**. It is secure but does not use a private, dedicated leased line.

#### **3. IPsec Encryption Modes**

IPsec has two modes of operation. It is crucial to understand the difference between a packet's **Header** (the "envelope" containing source/destination IPs) and its **Payload** (the actual data inside).

*   **Transport Mode:** Only the **payload** of the packet is encrypted. The original header remains intact and is not encrypted.
*   **Tunnel Mode:** The **entire original packet (both header and payload)** is encrypted and authenticated. A new header is added for routing.

**Key Fact:** **Oracle Cloud Infrastructure (OCI) supports Tunnel Mode.**

#### **4. Advantages of Using Site-to-Site VPN**

1.  **Cost-Effective:** Because it uses the public internet, there is no need for expensive, dedicated leased lines.
2.  **Quick to Set Up:** Ideal for Proof-of-Concepts (POCs) or when a connection needs to be established rapidly.
3.  **Secure:** All data is encrypted using the IPsec protocol.
4.  **Built-in Redundancy:** Each VPN connection is provisioned with **two tunnels** (Tunnel 1 and Tunnel 2) for high availability.

#### **5. Detailed Components and Configuration**

**A. The CPE Object**

*   The physical CPE device resides in your on-premises data center.
*   In OCI, you create a **CPE Object**. This is a *virtual representation* of your physical on-premises device.
*   You must provide the **public IP address** of your physical CPE device when creating this CPE object in OCI.
*   A single CPE device (with one public IP) can support up to **eight separate IPsec connections**.

**B. The IPsec Connection**

*   This is the resource you create in OCI that represents the Site-to-Site VPN connection itself.
*   It logically connects the **CPE Object** to the **Dynamic Routing Gateway (DRG)**.

**C. Tunnels**

*   Each IPsec connection automatically creates **two tunnels** for redundancy.
*   Each tunnel has its own configuration details, including:
    *   A unique **public IP address** (provided by OCI).
    *   A **Shared Secret** (a pre-shared key for authentication).
*   You need this configuration information to set up the tunnels on your physical on-premises CPE device.

**D. Routing**

*   You can configure routing for the VPN in two ways:
    1.  **Static Routing:** You manually define the routes.
    2.  **Dynamic Routing (BGP):** Uses the Border Gateway Protocol (BGP) to exchange routes automatically.
*   **Important Recommendation:** Oracle advises configuring the **same routing type (Static or BGP) for both tunnels** of a single connection.

#### **6. Special Scenario: CPE Behind a NAT Device**

*   Sometimes, the on-premises CPE device is not directly accessible and sits behind a Network Address Translation (NAT) device (e.g., a firewall).
*   By default, OCI uses the CPE's public IP for the Internet Key Exchange (IKE) protocol.
*   In a NAT scenario, you must change the **IKE Identifier** for the CPE to point to its **private IP address** instead of its public IP.

#### **7. Supported Protocols**

*   OCI Site-to-Site VPN supports both **Internet Key Exchange (IKE) version 1 and IKE version 2**.

### **OCI Study Notes: Site-to-Site VPN - Deep Dive**

#### **1. Core Concept Review**

*   **Service:** Site-to-Site VPN Connection.
*   **Purpose:** Establishes a secure, encrypted IPsec link between an on-premises network and an OCI Virtual Cloud Network (VCN).
*   **How it Works:** The IPsec protocol encrypts all traffic before it leaves the source and decrypts it upon arrival at the destination.
*   **Network Path:** Communication travels over the **public internet**, but the encryption ensures security.

#### **2. Key Features & Architecture**

*   **Encrypted & Secure:** All data passing between your network and OCI is encrypted.
*   **High Availability:** Every IPsec connection you create automatically provisions **two separate tunnels**. This provides redundancy because the tunnels terminate on two distinct, redundant Oracle routers.
*   **Protocol Support:** Supports both **Internet Key Exchange (IKE) version 1 and IKE version 2** for tunnel negotiation and management.

**Visualizing the Architecture:**
*   **On-Premises Side:** Your data center with a **Customer Premises Equipment (CPE)** device.
*   **OCI Side:** Your **Virtual Cloud Network (VCN)** with an attached **Dynamic Routing Gateway (DRG)**.
*   **The Connection:** The **IPsec connection** links the CPE and the DRG over the internet.

#### **3. Primary Use Cases**

1.  **Proof of Concept (POC):**
    *   **No long-term contracts or commitments.**
    *   **Fast setup** as it doesn't require provisioning dedicated physical lines.
    *   Ideal for testing connectivity and application access quickly.

2.  **Connecting Enterprise Sites:**
    *   Securely connect headquarters, branch offices, and private data centers to OCI.
    *   Enables all connected sites to access cloud-based applications.

3.  **Hybrid Cloud & Multi-Cloud:**
    *   Connect existing on-premises infrastructure to OCI.
    *   Establish secure connections between different cloud providers.

4.  **Redundancy for FastConnect:**
    *   Can be used as a **backup encrypted path** for the primary, private Oracle FastConnect connection, providing a robust disaster recovery solution.

#### **4. Component Details**

**A. Customer Premises Equipment (CPE)**

*   This is the physical or software-based device located in your on-premises data center (e.g., a router/firewall).
*   It can be from various verified vendors, including **Cisco, Fortinet, Check Point, Juniper, and Libreswan**.
*   **CPE Object in OCI:** This is a *virtual representation* of your physical CPE. When creating it, you must provide:
    *   A name for the object.
    *   The **public IP address** of your physical CPE device.
*   **Scalability:** A single CPE device (with one public IP) can support up to **eight separate IPsec connections**.

**B. IPsec Connection & Tunnels**

*   The **IPsec Connection** is the OCI resource that defines the link between the **CPE Object** and the **Dynamic Routing Gateway (DRG)**.
*   **Each IPsec connection creates two tunnels** (Tunnel 1 and Tunnel 2).
*   **Each tunnel has its own configuration:**
    *   A unique **public IP address** (assigned by OCI).
    *   A **Pre-Shared Key (PSK)** for authentication.

**C. Dynamic Routing Gateway (DRG)**

*   Think of this as the **VPN headend on the OCI side**. It is the virtual router that directs traffic between your VCN and the VPN tunnels.

#### **5. Configuration Modes & Routing**

**A. IPsec Tunnel Mode (OCI Supported Mode)**

*   OCI exclusively supports **Tunnel Mode**.
*   **Tunnel Mode:** Encrypts and authenticates the **entire original IP packet, including both the header and the payload**. This provides a higher level of security.
*   **Transport Mode (Not Supported by OCI):** Only encrypts the **payload** of the packet, leaving the original header intact.

**B. Routing Options**

You can configure how traffic is directed through the tunnels using:
*   **Static Routing:** You manually define the network paths.
*   **Dynamic Routing (BGP):** Uses the Border Gateway Protocol to automatically learn and exchange routes.
*   **Policy-Based Routing:** Routes traffic based on defined policies (e.g., source IP, protocol).

#### **6. Special Configuration: CPE Behind NAT**

*   **Default Behavior:** OCI uses the CPE's **public IP address** as its IKE identifier.
*   **Problem:** If your physical CPE sits **behind a NAT device** (like a firewall), its public IP is not directly reachable.
*   **Solution:** You must manually change the **IKE Identifier** in the OCI configuration to use the CPE's **private IP address** instead of its public IP.