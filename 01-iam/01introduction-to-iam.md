### **Study Notes: OCI Identity & Access Management (IAM) - Core Concepts**


---

#### **1. What is OCI IAM?**

Oracle Cloud Infrastructure Identity and Access Management (OCI IAM) is a **cloud-native service** that controls two fundamental security aspects for your cloud resources:

*   **Identity (Authentication):** Verifying *who* a user or system is. It manages the complete lifecycle of user identities.
*   **Access Management (Authorization):** Determining *what* an authenticated user or system is allowed to do.

In simple terms: **OCI IAM controls *who* has access to *what* cloud resources.**

---

#### **2. The Two Pillars: Authentication & Authorization**

##### **Pillar 1: Authentication (AuthN)**
This is the process of **verifying an identity**. It's the first gatekeeper, ensuring only valid users and systems can enter.

*   **Analogy:** Logging into a website with a username and password. You are proving you are a valid user.
*   **Goal:** To confirm the entity (user, system, application) requesting access is who they claim to be.

**Methods of Authentication in OCI IAM:**

1.  **User Credentials:** The most common method. Username and password.
2.  **Passwordless Authentication:** A more secure login method that doesn't use a traditional password.
3.  **API Signing Keys:** A **public-private key pair** used to securely sign and authenticate **API calls** made through the OCI CLI (Command Line Interface) or SDKs (Software Development Kits). This is crucial for automation.
4.  **OAuth 2.0 Tokens:** Allows applications to access OCI resources on a user's behalf without needing the user's password.
5.  **Instance Principals:** Allows a **compute instance** (a virtual machine) to call OCI services **without storing any user credentials** (username/password or keys) on the instance itself. The instance's identity is used for authentication.
6.  **Federated Identity:** Allows users to log in using an **external identity provider** (like Microsoft Active Directory) that supports the **SAML 2.0 protocol**. Users don't need a separate OCI password.
7.  **Multi-Factor Authentication (MFA):** An **extra layer of security** on top of your password (e.g., a code from your phone). It is not a standalone method but an enhancement to others.

##### **Pillar 2: Authorization (AuthZ)**
This happens **after** successful authentication. It is the process of **verifying permissions**. It determines what actions an authenticated identity is allowed to perform.

*   **Mechanism:** Authorization in OCI is performed using **IAM Policies**.
*   **Type of Control:** **Fine-grained** or **Role-Based Access Control (RBAC)**.

**Detailed Authorization Workflow Example:**

Let's assume our OCI tenancy has these resources: Compute Instances, Object Storage Buckets, and Databases.

1.  **Authentication:** A user, `User-A`, provides their credentials and is confirmed to be a valid identity.
2.  **Policy Check:** `User-A` tries to stop a compute instance. The system checks the IAM policies associated with `User-A`.
3.  **Authorization Decision:**
    *   **Scenario 1 (Allowed):** The policies grant `User-A` permission to `start, stop, and terminate` compute instances. The action is **authorized**, and the command executes.
    *   **Scenario 2 (Denied):** `User-A` then tries to delete a storage bucket. The policies are checked again. If `User-A` only has permissions for compute instances and **no permissions** for storage, the action is **denied**.

**Key Takeaway:** Authentication answers "Are you who you say you are?". Authorization answers "Are you allowed to do what you are trying to do?".

---

#### **3. Core Components of OCI IAM**

##### **A. Identity Domain**
*   **What it is:** A **logical container** for your users, groups, and applications.
*   **Purpose:** To separate and manage different sets of identities.
*   **Example:** You can have one identity domain for your **Production team** and a separate one for your **Development team**. This provides logical isolation for identity management.

##### **B. Compartment**
*   **What it is:** A **logical container** used to isolate and organize your **cloud resources** (like compute instances, databases, block volumes) within your OCI tenancy.
*   **Purpose:** To create security and access boundaries. Policies are often applied at the compartment level to control what users can do with the resources inside.

##### **C. Users, Groups, and Policies (The RBAC Trio)**
This is the standard pattern for implementing Role-Based Access Control (RBAC) in OCI.

1.  **Users:** Individual accounts representing a person or system that needs to interact with OCI.
2.  **Groups:** A collection of users. **You assign permissions to groups, not to individual users.** This makes management scalable and consistent.
3.  **Policies:** Documents written in a simple syntax that define the **permissions** granted to groups. Policies are attached to a **scope** (either the entire tenancy or a specific compartment).

---

#### **4. The Complete IAM Workflow**

Here is the standard process for setting up access control:

1.  **Create an Identity Domain:** Set up the container for your identities.
2.  **Create Users & Groups:** Inside the domain, create your users and organize them into logical groups (e.g., "Developers," "Network-Admins").
3.  **Create Compartments:** Organize your cloud resources into logical compartments (e.g., "Prod-Compartment," "Dev-Compartment").
4.  **Write Policies:** Create policies that grant specific permissions to the groups. These policies are scoped to a compartment or the tenancy.
    *   **Example Policy Logic:** `Allow group 'Developers' to manage virtual-network-family in compartment 'Dev-Compartment'`

**Summary Flow:**
**Identity Domain** -> Manages **Users/Groups** -> **Policies** grant permissions to groups -> Policies control access to **Resources** in **Compartments**.

---

#### **Key Terminology & Facts to Memorize**

*   **OCI IAM** is the service for identity and access management.
*   **Authentication (AuthN)** = Verifying Identity.
*   **Authorization (AuthZ)** = Verifying Permissions using **IAM Policies**.
*   **Instance Principals** allow compute instances to call OCI APIs without stored credentials.
*   **Federated Identity** uses external identity providers via the **SAML 2.0** protocol.
*   The standard practice is to assign permissions to **Groups**, not individual Users.
*   **Compartments** are for isolating **resources**.
*   **Identity Domains** are for isolating **users and groups**.
*   Policies have a **scope**, which can be the **Tenancy** or a **Compartment**.