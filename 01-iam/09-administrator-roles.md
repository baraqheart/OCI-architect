### **Study Notes: Understanding OCI IAM Administrator Roles**

---

#### **1. The Need for Administrator Roles**

When an identity domain is created, a **Domain Administrator** is established. This user is a **superuser** for the entire domain, with privileges to manage everything: users, groups, applications, system configurations, and security settings.

To avoid over-reliance on a single superuser and to follow the principle of least privilege, the Domain Administrator must **delegate responsibilities**. Administrator roles provide a predefined, secure way to grant specific administrative capabilities to other users without giving them full superuser access.

---

#### **2. Predefined Administrator Roles and Their Permissions**

OCI IAM provides a set of built-in administrator roles. These roles come with a broad set of permissions for managing the identity domain, and **no complex IAM policies need to be written** to use them.

*   **Identity Domain Administrator:**
    *   **Permissions:** The **superuser** of the domain. Has access to **all capabilities** within the domain.
    *   **Key Responsibility:** Delegating all other administrative roles to users.

*   **Security Administrator:**
    *   **Permissions:** Manages system-wide security configurations.
    *   **Specific Duties:** Configures password policies, sign-in policies, Multi-Factor Authentication (MFA), and identity providers (like federation).

*   **Application Administrator:**
    *   **Permissions:** Manages application integrations within the domain.
    *   **Specific Duties:** Creates, updates, and deletes applications. Grants or revokes user and group access to specific applications.

*   **User Administrator:**
    *   **Permissions:** Has comprehensive control over user and group management.
    *   **Specific Duties:** Creates, deletes, and manages users and groups, including all related capabilities.

*   **User Manager:**
    *   **Permissions:** Has **more limited permissions** than a User Administrator.
    *   **Specific Duties:** Can be assigned to manage all users or only users within a specific group. Handles user account updates, activations, deactivations, and password resets.

*   **Help-desk Administrator:**
    *   **Permissions:** Provides user support with limited scope.
    *   **Specific Duties:** Unlocks user accounts, resets passwords, manages authentication factors, and handles bypass codes.

*   **Auditor:**
    *   **Permissions:** Focused on compliance and monitoring.
    *   **Specific Duties:** Generates and runs audit reports to track activities within the identity domain.

---

#### **3. Critical Scope and Key Benefits**

**Critical Scope Limitation:**
All predefined administrator roles **apply only to the capabilities and data plane of the identity domain itself.** They **do not** grant any permissions to manage OCI resources in compartments (e.g., compute instances, storage buckets). Access to OCI resources is governed solely by **IAM Policies**, which is a separate system.

**Key Benefits of Using Predefined Roles:**

*   **Simplified Delegation:** Allows for easy and precise assignment of duties without needing to create and understand complex IAM policies.
*   **Aligned with Best Practices:** The roles are designed based on common security and operational patterns, making your IAM structure more secure and efficient from the start.
*   **Relevance and Specificity:** Each role is directly tied to a specific set of tasks within the domain, ensuring users have only the access they need.

---

#### **Key Terminology & Facts to Memorize**

*   The **Domain Administrator** is the initial superuser for an identity domain.
*   **Predefined Administrator Roles** are used to delegate specific administrative tasks without granting full superuser access.
*   These roles **do not require writing IAM policies**; their permissions are built-in.
*   **These roles only control access to the identity domain's management plane.** They **cannot** be used to grant access to OCI service resources like compute or storage; that is done exclusively through **IAM Policies**.
*   Key roles include: **Security Administrator** (for MFA/policies), **Application Administrator** (for app integrations), **User Administrator** (full user control), and **User Manager** (limited user control).