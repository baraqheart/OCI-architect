### **OCI Study Notes: Virtual Test Access Points (VTAP)**

#### **1. Core Function and Purpose**

*   **What it is:** A service that allows you to **mirror (copy) network traffic** from a source resource and send that copy to a target resource for analysis.
*   **Primary Use Cases:**
    *   **Troubleshooting** application performance issues.
    *   **Security Analysis** and deep packet inspection to detect anomalous behavior.
    *   **Data Monitoring** for compliance and auditing mandates.

#### **2. Key Components of a VTAP**

A VTAP configuration consists of three mandatory parts:

1.  **VTAP Source:** The resource whose traffic you want to monitor and mirror.
    *   **Supported Sources:** A compute instance's VNIC, Load Balancer, Database System, Exadata VM Cluster, or Autonomous Database.
2.  **VTAP Target:** The destination that receives the mirrored traffic.
    *   **Requirement:** The target **must be a Network Load Balancer (NLB)** configured with a **UDP listener on port 4789**.
3.  **Capture Filter:** A set of rules that defines **exactly which traffic** from the source is mirrored (e.g., only HTTP traffic, only traffic from a specific IP range). A VTAP must have a capture filter.

#### **3. Critical Configuration Rules**

*   **VCN Constraint:** The **VTAP source and VTAP target must be located in the same Virtual Cloud Network (VCN)**.
*   **Overlapping Sources:** If you have multiple VTAPs monitoring overlapping sources, the traffic will be mirrored by the VTAP with the **more specific source configuration**.
*   **Encapsulation:** The mirrored traffic is encapsulated using the **VXLAN protocol** and sent to the target NLB via **UDP port 4789**.

#### **4. How It Works: Example Workflow**

1.  A virtual machine (VM) in **Subnet A** sends traffic to a VM in **Subnet B**.
2.  A **VTAP is configured on the source VM's VNIC** in Subnet A.
3.  The VTAP's **capture filter** inspects the outbound traffic. If the traffic matches the filter rules (e.g., specific protocol or port), it is mirrored.
4.  The mirrored traffic is sent to the **VTAP Target**, which is a Network Load Balancer in **Subnet C**.
5.  The NLB distributes the mirrored traffic to its **backend set** (e.g., a cluster of security or monitoring appliances).
6.  The backend servers perform the necessary **analysis, inspection, or logging** on the copied traffic, without interfering with the original data flow.