### **Study Notes: Managing Users in OCI IAM**

---

#### **1. The User Lifecycle**

A user account in OCI IAM progresses through several distinct stages, mirroring an employee's journey in an organization.

*   **1. Non-Existent:** The user profile has not been created yet.
*   **2. Activated:** The user account is created and active. The user can sign in and access OCI resources and applications based on the permissions assigned to them.
*   **3. Deactivated:** The user account is temporarily disabled. A user in this stage **cannot sign in or access any OCI resources.** This is used for scenarios like sabbatical leave or temporary access restriction. A deactivated account can be **reactivated** to restore all previous access.
*   **4. Deleted:** The user account is permanently removed. This is the final stage for users who have left the company. **All group memberships, application access, and admin role privileges associated with the account are permanently revoked.**

---

#### **2. Who Can Manage Users?**

Administrative control over users is delegated through a hierarchy of roles.

*   **Tenancy Administrator:** A user in the default domain's `Administrators` group. This super user has permission to manage **all resources across the entire tenancy**, including all users and groups in all identity domains.
*   **Domain Administrator:** A user in a domain's `Domain Administrators` group. This user has full control **within their specific identity domain**, including the ability to manage all users and groups in that domain.
*   **User Manager Administrator:** A specialized administrative role that a Domain Administrator can assign to other users. A User Manager can manage users and groups **within the domain** but typically has fewer privileges than a full Domain Administrator.
*   **Users with IAM Policies:** It is possible to write a specific IAM policy that grants a user or group the permission to manage users.
    *   **Example Policy Concept:** `Allow group 'HR-Admins' to manage users in tenancy`
    *   This would allow any user in the `HR-Admins` group to create and manage users.

---

#### **3. Methods for Onboarding Users**

OCI IAM provides multiple methods to create users, catering to different scales and use cases.

##### **For Individual or Small-Scale Creation:**

*   **OCI Console/API/CLI:** Manually create users one by one through the web console, or automate it via API/CLI calls.

##### **For Bulk Onboarding (Large Numbers of Users):**

*   **CSV Import:** Use a Comma-Separated Values (CSV) file to import many users and their details at once through the console.

##### **For Self-Service Registration:**

*   **IAM Self-Registration:** Allow external users (e.g., customers) to create their own accounts through a provided registration link.

##### **For Automating from External Sources:**

*   **Application Catalog:** Pre-integrated connectors to bring users from systems like **Oracle HCM (Human Capital Management)**, Oracle Unified Directory, etc.
*   **SCIM APIs:** Use System for Cross-domain Identity Management (SCIM) standards for automated user provisioning and de-provisioning between systems.
*   **Bridges (For On-Premises Directories):**
    *   **Active Directory (AD) Bridge:** A software component you install on-premises to synchronize and provision users from Microsoft Active Directory to OCI IAM.
    *   **Provisioning Bridge:** A more general bridge for synchronizing users from other on-premises directories.
*   **Just-In-Time (JIT) Provisioning:** Used with federated identity (e.g., SAML 2.0). A user is automatically created in OCI IAM the first time they log in using an external identity provider.

---

#### **Key Terminology & Facts to Memorize**

*   User states are: **Non-Existent -> Activated <-> Deactivated -> Deleted**.
*   A **Deactivated** user cannot sign in but can be **reactivated**. A **Deleted** user is permanently gone.
*   The **Tenancy Admin** has the highest level of access across all domains.
*   The **Domain Admin** has full control within a single domain.
*   Use **CSV Import** for adding many users at once.
*   Use **AD Bridge** or **Provisioning Bridge** to sync users from an on-premises directory like Microsoft Active Directory.
*   **Just-In-Time Provisioning** automatically creates users during their first federated login.