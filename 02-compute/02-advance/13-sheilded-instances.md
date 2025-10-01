### **OCI Study Notes: Shielded Instances**

#### **1. Core Purpose**

*   **What it Protects Against:** Defends virtual machine and bare metal instances against sophisticated, low-level threats like **Rootkits and Bootkits** that infect firmware and the operating system and are difficult to detect.
*   **Goal:** To ensure the integrity of the boot process and the underlying platform.

#### **2. How to Enable**

*   **Process:** Follow the standard instance creation workflow.
*   **Prerequisites:** Select an **image and a shape** that support shielded instances (indicated by a **shield icon** next to them).
*   **Configuration:** In the advanced options, enable **"Secure and Measured Boot"**.

#### **3. Core Security Features (The Three Pillars)**

Shielded instances combine three technologies, provided by OCI at **no additional charge**:

**A. Secure Boot**
*   **What it is:** A **UEFI (Unified Extensible Firmware Interface) feature**.
*   **How it Works:** Prevents the system from booting if any boot component (bootloader, OS kernel) is **not properly signed with a trusted key**. It blocks unauthorized or tampered software from running during boot.

**B. Measured Boot**
*   **What it is:** A process that **complements Secure Boot**.
*   **How it Works:** Takes cryptographic **measurements (hashes)** of each boot component (bootloaders, drivers, OS) and stores them.
*   **Purpose:** Verifies that these measurements **do not change from one boot to the next**, providing a verifiable record of the boot process. This feature is **only available on VM instances**.

**C. Trusted Platform Module (TPM)**
*   **What it is:** A **specialized, secure cryptographic chip**.
*   **How it Works:**
    *   The TPM is used by Measured Boot to **securely store the boot measurements**.
    *   Enabling Measured Boot for a VM **automatically enables a virtual TPM (vTPM)**.
    *   Measurements are stored in **Platform Configuration Registers (PCRs)**, which are secure memory locations inside the TPM that hold a summary of all measurement results.