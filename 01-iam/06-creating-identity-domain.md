### **Study Notes: Creating Multiple Identity Domains**

---

#### **1. Why Create Multiple Identity Domains?**

While the default domain is sufficient for basic use, creating multiple domains is a critical practice for any complex or large-scale OCI environment. The primary reasons are:

*   **Isolation of Administrative Control:** Domains act as secure containers that separate different teams, departments, or user populations. This prevents unauthorized access and interference between them.
    *   **Example:** You can have a completely separate domain for your **Production** environment and another for your **Development** environment, each with its own unique set of users and integrated applications.

*   **Tailored Security and Compliance:** Different groups have different security requirements.
    *   **Example:** Users in a **Production domain** can be configured with stricter security policies (like stronger MFA) and access controls compared to users in a **Development domain**. This ensures data security and helps meet regulatory compliance.

*   **Simplified and Structured Management:** Multiple domains provide a structured way to organize identities, making it easier to oversee and maintain a large organization.
    *   **Analogy:** Think of it like having different folders on your computer desktop to keep your files organized.

*   **Granular Administrative Delegation:** Each domain can have its own set of administrators. This allows you to delegate control without giving individuals authority over the entire tenancy.
    *   **Example:** You can have a "Domain Administrator" for the development team who can manage users and applications within their own domain but has no access to the production domain.

---

#### **2. What Happens When You Create a New Identity Domain?**

Creating a new domain automatically sets up a new, self-contained administrative environment.

*   **Default Administrator Group and User:** The domain creation process automatically creates:
    1.  A default **Administrator group** for the new domain.
    2.  One initial **Domain Administrator user**.
*   **Responsibilities of the Domain Administrator:** This user can manage all aspects *within their domain*, including:
    *   Managing users, groups, and applications.
    *   Assigning other administrative roles (e.g., Security Admin, Application Admin).
    *   Configuring Multi-Factor Authentication (MFA), password policies, and signing policies.
    *   Creating self-registration profiles for consumer users.

---

#### **3. Critical Difference: Domain Replication**

This is a crucial operational difference between the default domain and any new domains you create.

*   **Default Domain:** Is **automatically replicated to all regions** to which your tenancy is subscribed.

*   **New (Additional) Domains:** Are **NOT automatically replicated**.
    *   **Home Region:** The region you select in the console when creating the domain becomes its **Home Region**.
    *   **Manual Replication Required:** If users in this new domain need to access OCI resources in other regions, you **must manually enable replication** for the domain to those specific regions.

**Example:** If you create a new domain while your console is set to the **US East (Ashburn)** region, Ashburn becomes its home region. For a user in this domain to manage a compute instance in **UK South (London)**, you must first enable replication of the domain to the London region.

---

#### **Key Terminology & Facts to Memorize**

*   Create **multiple identity domains** for **isolation, tailored security, and delegated administration**.
*   Each new domain gets its own **Domain Administrator** user and group.
*   The **region selected during creation** is the domain's **Home Region**.
*   **Unlike the default domain, new domains are NOT replicated by default.** You must **manually enable replication** to other regions if access is needed there.