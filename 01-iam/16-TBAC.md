### **Study Notes: OCI Tag-Based Access Control (TBAC)**

---

#### **1. What is Tag-Based Access Control (TBAC)?**

Tag-Based Access Control is an advanced method for managing permissions in OCI. It uses **resource tags** (metadata key-value pairs) as conditions within IAM policies, allowing you to control access based on these tags rather than just compartments or group names.

*   **Core Concept:** Write a **single policy** that grants access based on a tag's presence and value. This policy can then automatically apply to **any resource, group, or compartment** that has that tag, both now and in the future.
*   **Primary Benefit:** It solves the problem of writing and maintaining numerous repetitive policies for many compartments or groups. You manage access by **applying or removing tags**, not by constantly updating policies.

---

#### **2. How TBAC Works: The Two Perspectives**

Policies use specific variables to check tags on either the **requester** (the principal) or the **target** (the resource being accessed).

##### **A. Controlling Access Based on the REQUESTER's Tags**

This method grants permissions based on tags applied to the user's group, dynamic group, or compartment.

*   **Policy Variable:** `request.principal.<principal-type>.tag.<tag-namespace>.<tag-key>`
*   **Principal Types:** `group`, `dynamicgroup`, `compartment`

**Example 1: Group Tag**
*   **Scenario:** Allow any user in a group tagged as a "Production" group to manage instances.
*   **Tag Applied To:** The IAM Group.
*   **Policy:**
    `ALLOW ANY-USER TO MANAGE instances IN COMPARTMENT HR WHERE request.principal.group.tag.operations.project = 'Prod'`
*   **How it Works:** Any user whose group has the tag `operations.project = 'Prod'` gains this permission. If you tag a new group with this, its users automatically get access without a policy change.

**Example 2: Dynamic Group Tag**
*   **Scenario:** Allow compute instances in a dynamically grouped tagged for "BatchProcessing" to read objects.
*   **Tag Applied To:** The Dynamic Group.
*   **Policy:**
    `ALLOW DYNAMICGROUP BatchJobs TO READ objects IN TENANCY WHERE request.principal.dynamicgroup.tag.workload.type = 'Batch'`

##### **B. Controlling Access Based on the TARGET's Tags**

This method grants permissions based on tags applied to the resource being accessed or its compartment.

*   **Policy Variable (Target Resource):** `target.resource.tag.<tag-namespace>.<tag-key>`
*   **Policy Variable (Target Compartment):** `target.resource.compartment.tag.<tag-namespace>.<tag-key>`

**Example 1: Target Resource Tag**
*   **Scenario:** Allow the "Finance" group to manage only resources tagged as "Finance-Owned".
*   **Tag Applied To:** The individual resources (e.g., a compute instance, a database).
*   **Policy:**
    `ALLOW GROUP Finance TO MANAGE all-resources IN TENANCY WHERE target.resource.tag.operations.owner = 'Finance'`
*   **How it Works:** Users in the Finance group can only manage resources that have the specific tag `operations.owner = 'Finance'`.

**Example 2: Target Compartment Tag**
*   **Scenario:** Allow "Testers" to use resources in any compartment tagged as a "Test" environment.
*   **Tag Applied To:** The Compartment.
*   **Policy:**
    `ALLOW GROUP Testers TO USE all-resources IN TENANCY WHERE target.resource.compartment.tag.environment.role = 'Test'`
*   **How it Works:** The Tester group gets access to resources in **any compartment** (now or in the future) that has the tag `environment.role = 'Test'`.

---

#### **3. Practical Use Cases and Scenarios**

##### **Use Case A: Granting Cross-Compartment Access to Admin Groups**

*   **Problem:** You have three compartments (Project-A, B, C), each with its own admin group (A-Admins, B-Admins, C-Admins). You create a new "Test" compartment and want all existing and future admins to manage it without creating a new group or writing multiple policies.
*   **Solution:**
    1.  **Tag all admin groups** with a common tag, e.g., `empgroup.role = 'Admin'`.
    2.  **Write a single policy** for the Test compartment:
        `ALLOW ANY-USER TO MANAGE all-resources IN COMPARTMENT Test WHERE request.principal.group.tag.empgroup.role = 'Admin'`
*   **Outcome:** Any user in any group tagged with `empgroup.role = 'Admin'` can manage the Test compartment. To add a new admin group in the future, simply tag it; no policy update is needed.

##### **Use Case B: Granting Access to All "Test" Environments**

*   **Problem:** You have multiple projects (A, B, C), each with "prod" and "test" child compartments. You want your "Testers" group to have access to all "test" compartments across all projects.
*   **Solution:**
    1.  **Tag all "test" compartments** with a common tag, e.g., `ResourceGroup.Role = 'Test'`.
    2.  **Write a single policy:**
        `ALLOW GROUP Testers TO USE all-resources IN TENANCY WHERE target.resource.compartment.tag.ResourceGroup.Role = 'Test'`
*   **Outcome:** The Testers group can access resources in any compartment tagged as `ResourceGroup.Role = 'Test'`. To revoke access from a specific test environment, simply remove the tag.

---

#### **Key Terminology & Facts to Memorize**

*   TBAC uses **tags** as conditions in policy `WHERE` clauses.
*   `request.principal...` variables check tags on the **user/requester** (their group, etc.).
*   `target.resource...` variables check tags on the **resource or compartment** being accessed.
*   TBAC **reduces the number of policies** you need to write and manage.
*   Access is managed by **applying or removing tags**, which is more flexible than editing policies.
*   **Not all OCI services support the `target.resource.tag` variable.** You must check the official OCI documentation for the current list of supported services. The `target.resource.compartment.tag` variable is more widely supported.