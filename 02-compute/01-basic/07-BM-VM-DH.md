### **OCI Study Notes: Bare Metal, Virtual Machines, and Dedicated Hosts**

#### **1. Bare Metal Instances**

*   **Definition:** Provides **direct, dedicated access to a physical server**. There is no hypervisor; your workload runs directly on the server hardware.
*   **Key Characteristics:**
    *   **Highest Performance:** No virtualization overhead.
    *   **Strong Isolation:** The entire physical server is dedicated to your tenancy.
*   **Ideal Use Cases:**
    *   Workloads that are **not virtualized**.
    *   **Performance-intensive** applications (e.g., high-performance computing - HPC).
    *   Workloads requiring **Bring Your Own License (BYOL)** for software that is licensed per physical socket or core.
    *   Workloads requiring a **specific, non-standard hypervisor**.

#### **2. Virtual Machine (VM) Instances**

*   **Definition:** An independent computing environment that runs on top of a physical bare metal server, utilizing a **hypervisor** to virtualize the resources.
*   **Key Characteristics:**
    *   Multiple VMs share the underlying physical hardware.
    *   More cost-effective for workloads that do not need an entire physical machine's resources.
*   **Ideal Use Cases:** Running general-purpose applications that do not require the full performance and resources of a dedicated physical server.

#### **3. Dedicated Virtual Machine Hosts**

*   **Definition:** A **physical server (bare metal) that is entirely dedicated to your tenancy**, on which you can then run and manage your own **virtual machines**.
*   **Key Characteristics:**
    *   **Combines the isolation of bare metal with the flexibility of VMs.**
    *   You have full control over the placement and configuration of VMs on the host.
*   **Primary Use Case:** To meet **compliance and regulatory requirements** that mandate single-tenant, non-shared physical infrastructure, while still needing the operational flexibility of virtual machines.