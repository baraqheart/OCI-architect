### **OCI Study Notes: Dedicated Virtual Machine Hosts**

#### **1. Core Definition and Purpose**

*   **What it is:** A **physical server that is entirely dedicated to a single tenancy**, on which you can run your virtual machine (VM) instances.
*   **Key Benefit:** Provides **physical isolation** from other customers, ensuring no "noisy neighbors" and meeting strict compliance and licensing requirements.

#### **2. Billing Model**

*   **Host Billing:** You are **billed for the entire dedicated host as soon as you create it**, based on its shape and size.
*   **Instance Billing:** You are **not billed separately** for the individual VM instances that you run on the host. The host cost covers the capacity.

#### **3. Host Shapes and Capacity**

*   **Shape Selection:** When you create a dedicated host, you select a host shape (e.g., `DVH.Standard3.64`). This shape determines the total physical capacity of the server.
*   **Usable vs. Billed Capacity:** A portion of the host's OCPUs is reserved for OCI's management overhead. Therefore:
    *   **Billed OCPUs:** The total number of OCPUs on the physical server you are paying for.
    *   **Usable OCPUs:** The number of OCPUs actually available for you to allocate to your VMs. This is less than the billed amount.
    *   **Example:** The `DVH.Standard3.64` shape provides **60 usable OCPUs** and **960 GB of usable memory** out of the total physical capacity.

#### **4. Supported VM Shapes**

*   A dedicated host shape supports a specific family of VM shapes.
*   **Example:** A `DVH.Standard2.52` host can only run VMs from the `VM.Standard2` series.

#### **5. Primary Use Cases**

*   **Regulatory Compliance:** To meet requirements that mandate single-tenant, non-shared physical infrastructure.
*   **Host-Based Licensing:** To comply with software licensing models that require licensing an entire physical server.

#### **6. Important Limitations**

The following features are **not supported** for instances running on a dedicated virtual machine host:

*   Autoscaling
*   Capacity Reservations
*   Instance Configurations
*   Instance Pools
*   Burstable Instances
*   Live Migration (Remote Migration)