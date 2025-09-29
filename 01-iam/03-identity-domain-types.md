### **Study Notes: OCI IAM Identity Domain - Deep Dive**

---

#### **1. What is an Identity Domain?**

An Identity Domain is a **self-contained identity and access management service** within OCI IAM.

*   **Core Definition:** It represents a **user population** and all its associated configurations, security settings, and application integrations.
*   **Simple Analogy:** Think of it as a **logical container or a dedicated "workspace"** for managing a specific set of users, their authentication methods, and the applications they are allowed to access.
*   **Key Property:** An Identity Domain is itself a **resource in OCI**. This means you can control access to the domain using **IAM policies** and place it within a specific **compartment** for organization.

---

#### **2. Primary Use Cases for Multiple Identity Domains**

You create separate Identity Domains to achieve **isolation and tailored security** for different groups of users. The video provides three clear examples:

1.  **Employee Access Domain:**
    *   **Purpose:** To manage access for all your company employees.
    *   **Scope:** Access to internal cloud and on-premises applications.
    *   **Benefit:** Simplifies authentication and authorization for corporate resources.

2.  **Business Partner Domain:**
    *   **Purpose:** To provide access for external business partners.
    *   **Scope:** Isolated access to specific systems like supply chain and ordering portals.
    *   **Benefit:** The partner domain has its **own unique set of users, authentication rules, and access policies**, completely separate from your employees. This enhances security and management.

3.  **Consumer Domain:**
    *   **Purpose:** To manage identities for the general public or customers.
    *   **Scope:** Access to public-facing applications.
    *   **Benefit:** Allows you to configure specific authentication settings (like self-registration) suitable for a large number of end-users, separate from your internal and partner identities.

---

#### **3. Core Capabilities & Features of an Identity Domain**

##### **A. Identity Store & Lifecycle Management**
This is about how you **onboard and manage users** within the domain.

*   **Methods to Add Users:**
    *   **Manually:** Via the OCI Console, CLI, or API.
    *   **Self-Service Registration:** Allowing users to create their own accounts.
    *   **Automated Account Provisioning:** Systematically creating user accounts.
*   **Directory Integration:** You can synchronize users and groups from an external directory.
    *   **Primary Example: Microsoft Active Directory.** You can use the **AD Bridge** component to sync with on-premises AD.
    *   **Provisioning Bridge:** Can be used to sync with any other external directory.
*   **Management Features:** Includes managing user roles, group membership, assigning permissions, and automating account provisioning/de-provisioning.

##### **B. Inbound Authentication & Single Sign-On (SSO)**
This defines **how users log in** to the Identity Domain itself.

*   **Authentication Methods:**
    *   **Social Logins:** Using providers like Google, Facebook, etc.
    *   **Federated Logins:** Using an organization's existing identity provider (like Active Directory) via the **SAML 2.0** protocol. This means users can log in with their corporate credentials without those credentials being stored in OCI.
    *   **Delegated Authentication:** Using the **AD Bridge** to validate passwords directly against an on-premises Active Directory.
*   **Security Enhancements:**
    *   **Multi-Factor Authentication (MFA):** Enforcing a second verification step. Methods include email passcodes, mobile app passcodes, and security questions.
    *   **Passwordless Authentication:** Moving beyond traditional passwords.
    *   **Adaptive Security:** Policies that can require stronger authentication based on user behavior, location, or device.

##### **C. Outbound Authentication & Single Sign-On (SSO)**
After a user is authenticated *into* the domain, this capability allows them to seamlessly log in *to other applications*.

*   **Application Integration:** You can add applications that users will access via the domain.
*   **Pre-Integrated Apps:** The domain includes an **application catalog with over 400 pre-configured templates** for popular SaaS applications.
*   **Custom Apps:** You can integrate any application using standard protocols like **SAML** or **OpenID Connect (OAuth)**.
*   **Legacy App Support:** For applications that do not support modern protocols, you can use **proxy gateways and bridges**.
*   **Target Applications Can Be:** SaaS apps, enterprise apps, Linux hosts, or VPN clients.

##### **D. OCI Authorization via Access Policies**
This capability is separate from the application SSO. It controls what a user can do **within the OCI platform itself**.

*   **Mechanism:** Uses **IAM Policies**.
*   **Purpose:** To manage access to OCI resources like compute instances, storage buckets, and virtual networks.
*   **This is the "Authorization (AuthZ)"** pillar from the core IAM lesson, applied within the context of the Identity Domain.

---

#### **4. The Four Core Capabilities of OCI IAM (Summary)**

The functionality of an Identity Domain can be summarized into four key areas:

1.  **Inbound Authentication & SSO:** *How users log in to the domain.*
2.  **Identity Store & Lifecycle Management:** *How users are created and managed in the domain.*
3.  **Outbound Authentication & SSO:** *How users log in from the domain to other applications.*
4.  **OCI Authorization (Access Policies):** *What users can do inside your OCI tenancy once they are logged in.*

---

#### **Key Terminology & Facts to Memorize**

*   An **Identity Domain** is a logical container for a user population and its settings.
*   You use **multiple domains** to isolate different user groups (Employees, Partners, Consumers).
*   **AD Bridge** is a component used to synchronize and delegate authentication with Microsoft Active Directory.
*   The **Application Catalog** contains **400+ pre-configured application templates** for easy SSO integration.
*   Identity Domains support standard protocols: **SAML 2.0** for federation and **OAuth/OpenID Connect** for modern application authorization.
*   **Inbound SSO** is for logging *into* the domain. **Outbound SSO** is for logging *from* the domain *to other apps*.
*   **OCI Authorization** is controlled by **IAM Policies**, which is a separate capability from Application SSO.