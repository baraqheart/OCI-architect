### **Study Notes: OCI IAM Policies - Authorization and Syntax**

---

#### **1. Introduction to Policies and Authorization**

Authorization in OCI is the process of granting granular permissions to users and systems, determining **what actions they can perform on which resources**. This is achieved through **IAM Policies**, which enforce Role-Based Access Control (RBAC).

*   **Core Principle:** By default, **everything in OCI is denied**. Policies are used to explicitly **allow** specific access. There is no such thing as a "deny" policy.
*   **Security Foundation:** Policies follow the **principle of least privilege**, meaning you only grant the minimum permissions necessary for a user to perform their job.

---

#### **2. The Policy Syntax Structure**

A policy statement is composed of four key parts: **Subject, Action, Placement, and Condition**.

The basic structure is:
`ALLOW <subject> TO <action> IN <placement> WHERE <condition>`

---

#### **3. Detailed Breakdown of Policy Components**

##### **A. The SUBJECT Clause: "Who" is being granted access.**

The subject is the principal (user or system) that will receive the permissions.

*   **For Groups of Users:**
    *   `ALLOW GROUP <domain-name>/<group-name> TO...`
    *   **Example:** `ALLOW GROUP ProductionDomain/Network-Admins TO...`
*   **For Dynamic Groups (Non-human resources):**
    *   `ALLOW DYNAMICGROUP <domain-name>/<dynamic-group-name> TO...`
    *   **Example:** `ALLOW DYNAMICGROUP ProductionDomain/Compute-To-Storage TO...`
*   **Using OCIDs:** You can use the unique OCID of a group or dynamic group instead of its name.
*   **For Multiple Groups:** You can chain multiple groups in a single policy statement.
    *   **Example:** `ALLOW GROUP DomainA/Admins, DomainB/Admins TO...`
*   **Critical Best Practice:** **Always prefix the group name with its domain** (e.g., `default/MyGroup`). If you omit the domain, the system implies the default domain, but explicitly stating it makes the policy more readable and less error-prone.

##### **B. The ACTION Clause: "What" actions they can perform.**

The action defines the permitted operations on OCI resources. Permissions are bundled into easy-to-understand "verb-resource" pairs instead of individual API calls.

*   **Verbs (Levels of Access):** Verbs define the level of power, from least to most privileged:
    1.  **INSPECT:** Allows listing and viewing basic details/metadata about a resource, but **not its content**. (e.g., see that a storage bucket exists, but not what's inside it).
    2.  **READ:** Allows reading the actual content and data of a resource. (e.g., read the contents of a file in object storage).
    3.  **USE:** Allows performing actions on **preexisting resources** but not creating new ones or deleting them. (e.g., attach a volume to an instance, restart a database).
    4.  **MANAGE:** Allows full control, including **create, update, and delete** operations. This is the highest level of access.

*   **Resource Types:** Resources are grouped for easier management.
    *   **`all-resources`:** Grants the verb permission on **every resource type in OCI**. This is the broadest permission.
    *   **Aggregate Resource Types (Families):** A group of related resources within a service.
        *   **Examples:** `virtual-network-family`, `database-family`, `instance-family`.
        *   A policy for `manage virtual-network-family` grants manage permissions on all networking components (VCNs, subnets, route tables, etc.).
    *   **Individual Resource Types:** A specific resource within a service.
        *   **Examples:** `vcns`, `subnets`, `databases`, `users`.
        *   A policy for `read buckets` grants read permission only on object storage buckets, and nothing else.

*   **Critical Exception - Identity Service:** The Identity service (which includes users, groups, policies, compartments) **does not have aggregate resource types**. You must always specify individual resource types like `users`, `groups`, `policies`, or `compartments` when writing policies for IAM.

##### **C. The PLACEMENT Clause: "Where" the permissions apply.**

The placement defines the scope within your tenancy where the subject is allowed to perform the actions.

*   **Tenancy-Wide:** `IN TENANCY`
    *   Grants the permission across the entire tenancy.
*   **Compartment-Scoped:** `IN COMPARTMENT <compartment-name>` or `IN COMPARTMENT <compartment-OCID>`
    *   Restricts the permission to a specific compartment and its sub-compartments.
    *   **Example:** `... IN COMPARTMENT Sandbox` or `... IN COMPARTMENT id:ocid1.compartment.unique_id`

---

#### **4. Using the OCI Documentation for Policies**

The official OCI documentation provides a complete list of all services, their aggregate resource families, and individual resource types. When writing policies, you must refer to this documentation to use the correct verb and resource type names.

---

#### **Key Terminology & Facts to Memorize**

*   The default security stance is **"default deny"**. Policies are used to **ALLOW** access.
*   Always follow the **principle of least privilege**.
*   Policy Syntax: **ALLOW `subject` TO `action` IN `placement`**.
*   **Verbs** are: INSPECT, READ, USE, MANAGE (in increasing order of privilege).
*   **Resource Types** can be: `all-resources`, an aggregate `-family`, or an individual resource.
*   The **Identity service has no aggregate resource families**; you must use individual types like `users` and `groups`.
*   **Always specify the domain name** for groups in the subject clause for clarity.
*   Use the **PLACEMENT** clause to scope permissions to the **tenancy** or a specific **compartment**.