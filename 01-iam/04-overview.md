### **Study Notes: Managing OCI IAM - A Practical Workflow**

---

#### **1. Module Objective & Practical Workflow**

This module provides a **hands-on, step-by-step guide** to setting up the core OCI IAM components. We will follow a logical, three-stage workflow that mirrors real-world implementation.

The entire process is structured as follows:

1.  **Foundation:** Set up the Identity Domain.
2.  **Population:** Create and organize Users and Groups within the domain.
3.  **Permission:** Use IAM Policies to grant access and define what users can do.

This workflow ensures you build your IAM structure in a correct, secure, and manageable way.

---

#### **2. Key Concepts to be Covered**

##### **A. The Default Identity Domain**
*   **Fact:** When you first create an OCI tenancy, a **Default Identity Domain** is automatically provisioned for you.
*   **Purpose:** This default domain initially contains the administrator user who created the tenancy and is used for managing core OCI resources.
*   **Implication:** You do not start from scratch. You will learn how this default domain works and when to create additional ones.

##### **B. Creating Additional Identity Domains**
*   **Purpose:** As learned in the previous lesson, you create new domains to isolate different user populations (e.g., partners, customers) with their own security settings.
*   **This module will cover the step-by-step process for creating a new, non-default identity domain.**

##### **C. Creating and Managing Groups & Dynamic Groups**
*   **Groups (Standard):** Collections of **human users** (e.g., "Developers," "Security-Admins"). This is the primary method for assigning permissions.
*   **Dynamic Groups:** A more advanced type of group where members are **not humans, but OCI resources** (like compute instances, functions). Members are defined by **rules** that match resource attributes.
    *   **Example Rule:** `All instances in compartment Prod-Compartment`
    *   **Use Case:** To allow a compute instance to call other OCI services (using Instance Principals), you must first place it in a Dynamic Group.

##### **D. Onboarding Users**
*   **Method 1: OCI Console (Manual):** You will learn how to create individual users directly through the OCI web interface.
*   **Method 2: Import Options (Bulk/Automated):** For adding many users at once, you will learn how to use import functionalities, which ties into the "Identity Lifecycle Management" capability of an identity domain.

##### **E. The Role of IAM Policies**
*   **This is the final and crucial step.** After creating domains, users, and groups, you must define what they are allowed to do.
*   **Policies** are the documents that grant **permissions** to groups (or dynamic groups).
*   You will learn the **syntax and structure** of policy statements and how to attach them to compartments or the tenancy.

---

#### **3. The Standard Implementation Workflow (Recap)**

This is the golden path for setting up IAM in OCI. Memorize this sequence:

1.  **STEP 1: Setup Identity Domain**
    *   Decide if you need a new domain or can use the default one.
    *   Create the domain.

2.  **STEP 2: Create Users & Groups**
    *   Create user accounts within the chosen domain.
    *   Organize users into logical Groups.
    *   (If needed) Create Dynamic Groups for non-human resources.

3.  **STEP 3: Define Permissions with Policies**
    *   Write IAM Policies that grant specific permissions to the Groups.
    *   Attach these policies to the relevant Compartment or the Tenancy.

**Visual Flow:**
**Identity Domain** ➡ **Users & Groups** ➡ **IAM Policies** ➡ **Access to OCI Resources**

---

#### **Key Terminology & Facts to Memorize**

*   Every OCI tenancy comes with a pre-built **Default Identity Domain**.
*   The standard IAM setup follows a three-step workflow: **Domain -> Users/Groups -> Policies**.
*   **Groups** are for grouping human users.
*   **Dynamic Groups** are for grouping OCI resources (like compute instances) based on matching rules.
*   Users can be onboarded **manually** via the Console or in **bulk** via import options.
*   **IAM Policies** are the final component that actually grants permissions; they are useless without properly structured Users and Groups first.