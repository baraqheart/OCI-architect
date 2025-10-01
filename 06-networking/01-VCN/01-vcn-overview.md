### **Study Notes: OCI Virtual Cloud Network (VCN) - Core Concepts**

---

#### **1. What is a Virtual Cloud Network (VCN)?**

A **Virtual Cloud Network (VCN)** is a fundamental, software-defined networking component in OCI that provides a customizable, private network within the Oracle Cloud.

*   **Core Function:** Acts as your own isolated, virtual network in the cloud where you can launch and manage cloud resources.
*   **Analogy:** Think of a VCN as your own private **data center network** in the cloud, where you have complete control over IP address ranges, subnets, routing, and security.
*   **Scope:** A VCN is **regional**—it spans all Availability Domains (ADs) within a single OCI region.

---

#### **2. Key Components of a VCN**

A VCN is built from several interconnected components that work together to provide networking functionality.

##### **A. Subnets**
A **subnet** is a subdivision of a VCN's IP address range. It allows you to segment your network and control traffic flow.

*   **Purpose:** To group related resources and apply specific network rules to them.
*   **Scope:** A subnet exists in a **single Availability Domain** (with a special case for regional subnets).
*   **Types of Subnets:**
    *   **Public Subnet:** Has a route to the internet. Resources in a public subnet can have public IP addresses and communicate with the public internet.
    *   **Private Subnet:** Does **not** have a route to the internet. Resources in a private subnet can only communicate with other resources in your VCN or connected networks; they cannot be reached from the public internet directly. This is more secure for backend systems like databases.

##### **B. Route Tables**
**Route Tables** contain a set of rules (routes) that determine where network traffic from your subnets is directed.

*   **Function:** The "road signs" of your VCN, telling traffic how to get to its destination.
*   **Key Route:** A route to an **Internet Gateway** is what defines a subnet as **public**. Without this route, a subnet is **private**.

##### **C. Security Lists (Stateful Firewall)**
**Security Lists** act as a virtual, stateful firewall for all resources in a subnet.

*   **Scope:** Rules are applied at the **subnet level**.
*   **Stateful:** If you allow an incoming (ingress) request, the response is automatically allowed to flow back out, regardless of egress rules.
*   **Default Behavior:** The default security list has restrictive rules. You must explicitly add rules to allow desired traffic.

##### **D. Network Security Groups (NSGs - Stateful Firewall)**
**Network Security Groups** are another type of stateful firewall, but they offer more granular control than Security Lists.

*   **Scope:** Rules are applied to individual **cloud resources** (like a specific compute instance), not the entire subnet.
*   **Benefit:** Allows you to create security policies based on application function or tier (e.g., a "Web-Server" NSG, a "Database" NSG), which is more flexible than subnet-level rules.

##### **E. Internet Gateway (IGW)**
A **gateway** that provides a path for traffic between your VCN and the public internet.

*   **Prerequisite for Public Subnets:** To make a subnet public, you must attach a route table to that subnet which includes a route sending internet-bound traffic (`0.0.0.0/0`) to the Internet Gateway.

##### **F. NAT Gateway**
A **Network Address Translation (NAT) Gateway** allows resources in **private subnets** to initiate outbound connections to the internet (e.g., for software updates) while preventing unsolicited inbound connections from the internet.

*   **Use Case:** A database server in a private subnet needs to download patches but must not be directly accessible from the internet.

---

#### **3. How Routing Works in a VCN**

Routing is the process that determines the path for network traffic.

*   **Each subnet has one route table.**
*   **Each route** in a table specifies a destination CIDR block and a target (e.g., Internet Gateway, NAT Gateway, a specific DRG, or another subnet via a Local route).
*   The VCN's router automatically provides a "Local Route" that allows all subnets within the same VCN to communicate with each other.

---

#### **4. Security Constructs Summary**

| Feature | Scope | Key Characteristic |
| :--- | :--- | :--- |
| **Security Lists** | Subnet Level | Applies rules to all resources in the subnet. **Stateful.** |
| **Network Security Groups (NSGs)** | Resource Level (Virtual NIC) | Applies rules to individual resources you choose. More granular. **Stateful.** |

---

#### **Key Terminology & Facts to Memorize**

*   A **VCN** is a private, customizable network in an OCI **region**.
*   **Subnets** segment a VCN and exist in a single **Availability Domain**.
*   A **Public Subnet** has a route to an **Internet Gateway**.
*   A **Private Subnet** does **not** have a route to the internet.
*   **Route Tables** define whether a subnet is public or private and control traffic flow.
*   **Security Lists** are firewalls at the **subnet level**.
*   **Network Security Groups (NSGs)** are firewalls at the **resource level** for more granular control.
*   A **NAT Gateway** allows private resources to access the internet without being directly exposed to it.