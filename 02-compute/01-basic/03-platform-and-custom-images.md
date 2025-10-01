### **OCI Study Notes: Compute Images**

#### **1. Core Definition**

*   An **image** is a template that determines the **operating system and other software** for a compute instance.
*   It can be thought of as a template for a virtual hard drive.

#### **2. Image Sources**

There are several sources for images when launching a compute instance:

*   **Platform Images:**
    *   **Definition:** Pre-built, standard operating system images provided by Oracle for OCI.
    *   **Examples:** Oracle Linux, Ubuntu, CentOS, Microsoft Windows Server, Oracle Autonomous Linux.

*   **Oracle Images:**
    *   **Definition:** Oracle Enterprise images and software solutions that are optimized and enabled for OCI.

*   **Custom Images:**
    *   **Definition:** Images that you create from an existing instance running in OCI.
    *   **Benefit:** The new instances you launch from a custom image will include all the **customizations, configurations, and software** that were on the original instance.

*   **Partner Images:**
    *   **Definition:** Trusted third-party images published in the Oracle Cloud Marketplace by Oracle partners.

*   **Bring Your Own Image (BYOI):**
    *   **Definition:** A feature that allows you to **import your own custom machine images** into OCI.
    *   **Limitation:** The maximum size for an imported image is **400 GB**.

#### **3. Important Details on Custom Images**

**A. Creation Process**
*   When you create a custom image from a running instance, the instance is **shut down and remains unavailable for several minutes** during the process. It restarts automatically once the image creation is complete.

**B. Windows Custom Images (Generalized vs. Specialized)**
*   **Generalized Images:**
    *   Images that have been "sysprepped" or cleaned of instance-specific information (like the security identifier - SID). They are used to create new, unique instances.
*   **Specialized Images:**
    *   Point-in-time snapshots of a running instance's disk. They are **useful for creating backups** of a specific instance but retain the original instance's unique identity.