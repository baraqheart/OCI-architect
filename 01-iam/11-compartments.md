### **Study Notes: OCI Compartments - Organization and Isolation**

---

#### **1. What are Compartments?**

Compartments are **logical containers** used to organize and isolate your OCI resources. They are a fundamental building block for resource management and access control.

*   **Analogy:** Think of your OCI tenancy as the main folder on a computer (the **root compartment**). Compartments are like **sub-folders** you create inside it to organize files (your cloud resources) in a structured way.
*   **Purpose:** They allow you to group related resources together for easier management, billing, and, most importantly, security.

---

#### **2. Key Features and Rules of Compartments**

##### **A. Resource Placement and Isolation**

*   **Single Compartment Membership:** Every resource you create in OCI **must belong to one, and only one, compartment.** A resource cannot exist in multiple compartments simultaneously.
*   **Cross-Compartment Interaction:** Resources in **different compartments can interact with each other** if network connectivity and security rules allow it.
    *   **Example:** A compute instance in the `Production-Compute` compartment can use a Virtual Cloud Network (VCN) located in the `Network` compartment. They do not need to be in the same compartment to communicate.
*   **Resource Mobility:** You can **move a resource from one compartment to another** after it has been created.
    *   **Example:** A virtual machine can be moved from the `Development` compartment to the `Production` compartment.

##### **B. Access Control**

*   **Primary Mechanism for Authorization:** Compartments are the primary **scope for IAM Policies**. When you write a policy, you specify whether it applies to the entire tenancy or to a specific compartment.
    *   **Example Policy:** `ALLOW GROUP DevDomain/Developers TO manage instances IN COMPARTMENT Development`
    *   This policy grants permissions **only within the `Development` compartment**, effectively isolating the developers' access.

##### **C. Global and Hierarchical Structure**

*   **Global Nature:** Compartments are **global constructs**, not tied to a single region. When you create a compartment, it is available in **every region** your tenancy is subscribed to.
    *   **Example:** You can create a `Project-Alpha` compartment and place a compute instance in it in the `US East (Ashburn)` region and a database in it in the `UK South (London)` region.
*   **Nested Compartments:** You can create a hierarchy of compartments, nesting them up to **six levels deep**.
    *   **Example Structure:**
        *   Root Compartment
            *   `Project-X` (Level 1)
                *   `Networking` (Level 2)
                *   `Compute` (Level 2)
                    *   `Team-A` (Level 3)
    *   **Benefit:** This allows you to mirror your organizational or project structure and apply very granular access controls.

---

#### **3. Best Practices**

*   **Do Not Use the Root Compartment:** It is a **best practice to avoid placing resources directly in the root compartment**. Always create dedicated compartments.
*   **Use for Isolation:** Create separate compartments to isolate environments (e.g., `Production`, `Development`, `Testing`), teams, or projects. This is crucial for security and cost tracking.
*   **Plan Your Hierarchy:** Before creating resources, plan your compartment structure to support your organization's needs for access control, billing, and management.

---

#### **Key Terminology & Facts to Memorize**

*   The **root compartment** is the top-level container created with your tenancy.
*   A resource **can only be in one compartment** at a time.
*   Resources in **different compartments can interact**.
*   Compartments are **global** and exist across all regions.
*   You can create **nested compartments** up to **6 levels deep**.
*   **IAM Policies** are attached to compartments to control access to the resources within them.
*   **Always create and use sub-compartments** instead of using the root compartment for resources.