### **OCI Study Notes: Compute Service Overview**

#### **1. Core Definition**

*   The OCI Compute Service allows you to provision and manage compute hosts, which are called **instances**.
*   It is a foundational service upon which many other OCI services are built.

#### **2. Key Concepts**

**A. Shapes**
*   A **shape is a template** that defines the hardware configuration of a compute instance.
*   It determines key resources allocated to the instance, including:
    *   Number of **OCPUs** (Oracle CPUs)
    *   Amount of **Memory (RAM)**
    *   Other resources, such as network bandwidth.

**B. Instance Types**
OCI Compute offers two primary types of instances:

1.  **Bare Metal Instances:**
    *   Provides **dedicated access to a physical server**.
    *   No hypervisor is involved; the server runs your workload directly on the hardware.

2.  **Virtual Machine (VM) Instances:**
    *   An independent computing environment that runs on top of a physical bare metal host, utilizing a hypervisor.
    *   Multiple VMs can run on a single physical server.

#### **3. Data Persistence: A Critical Distinction**

Understanding where data is stored is crucial for managing your instances:

*   **Local Drives (Ephemeral Storage):**
    *   Any changes made to the instance's local drives are **lost when the instance is terminated**.
    *   This storage is temporary.

*   **Attached Volumes (Block Volumes):**
    *   Changes saved to **volumes attached** to the instance (via the Block Volume service) are **retained** even after the instance is terminated.
    *   This storage is persistent and can be reattached to other instances.