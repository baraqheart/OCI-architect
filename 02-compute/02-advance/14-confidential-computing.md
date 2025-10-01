### **OCI Study Notes: Confidential Computing**

#### **1. Core Concept and Evolution**

*   **Traditional State:** Data in memory was accessible as **clear text**, creating a vulnerability.
*   **Confidential Computing:** A security paradigm that **encrypts data not just at rest (storage) and in transit (network), but also while it is in use (in memory)**.
*   **Key Benefit:** **Minimizes the list of trusted entities** by removing the need to trust the operating system, hypervisor, cloud administrators, or other ecosystem partners with access to the data.

#### **2. How It Works**

*   **Protection Level:** Data is protected **at the hardware level** by the processor.
*   **Mechanism:** Both the **data and the application** processing it are **encrypted and isolated in memory** while the application is running.
*   **Result:** Prevents unauthorized access or modification of the application or its data in memory.

#### **3. OCI Implementation with AMD EPYC Processors**

OCI leverages AMD EPYC processors and their security features:

*   **For Virtual Machines (VMs):** Uses **AMD Secure Encrypted Virtualization (SEV)**.
*   **For Bare Metal Servers:** Uses **AMD Secure Memory Encryption (SME)**.
*   **Supported Shapes:** These features are available in all OCI **E3 and E4** compute shapes.

#### **4. Supported Shapes and Images**

*   **Virtual Machine Shapes:**
    *   `VM.Standard.E4.Flex`
    *   `VM.Standard.E3.Flex`
    *   **Supported OS:** Oracle Linux 7.x or 8.x platform images.
*   **Bare Metal Shapes:**
    *   `BM.DenseIO.E4.128`
    *   `BM.Standard.E4.128`
    *   `BM.Standard.E3.128`
    *   **Supported OS:** Any platform image.

#### **5. Key Benefits**

*   **Enhanced Isolation with Real-Time Encryption:**
    *   A **unique encryption key is generated for each VM** during its creation.
    *   This key **resides solely within the AMD secure processor** on the CPU and is **inaccessible** to the VM's OS, the hypervisor, OCI administrators, or any application.
*   **No Application Changes Required:** Applications can run in confidential VMs without any modification.
*   **Minimal Performance Impact:** Applications experience little to no performance degradation when confidential computing is enabled.