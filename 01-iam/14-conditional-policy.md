### **Study Notes: OCI Conditional Policies**

---

#### **1. What are Conditional Policies?**

Conditional policies are advanced IAM policies that include a `WHERE` clause to add specific conditions for when the policy should grant access. This allows for fine-grained, dynamic access control beyond simply specifying who can do what and where.

*   **Basic Policy Syntax:** `ALLOW <subject> TO <action> IN <placement>`
*   **Conditional Policy Syntax:** `ALLOW <subject> TO <action> IN <placement> WHERE <condition>`
*   **Evaluation:** Conditions are evaluated as **True**, **False**, or **Not Applicable**. The policy only grants access if the condition evaluates to **True**.

---

#### **2. Why Use Conditions?**

Conditions enable you to implement **complex and precise access control** scenarios that are not possible with basic policies. They are essential for enforcing the principle of least privilege in dynamic environments.

---

#### **3. Policy Variables: The Building Blocks of Conditions**

Conditions are built using variables that provide context about the access request.

##### **A. The `request` Variable**
This variable contains attributes **about the incoming request itself**, such as who is making it and what they are trying to do.

*   **`request.user.id`:** The OCID of the specific user making the request.
*   **`request.permission`:** The specific, granular permission being requested (e.g., `OBJECT_INSPECT`, `VOLUME_CREATE`).
*   **`request.region`:** The OCI region where the request is being made (e.g., 'us-phoenix-1').

##### **B. The `target` Variable**
This variable contains attributes **about the resource** that is the target of the request.

*   **`target.bucket.name`:** The name of the object storage bucket being accessed.
*   **`target.compartment.id`:** The OCID of the compartment where the target resource resides.
*   **`target.workloadType`:** The type of workload for a specific service (e.g., for Autonomous Database: 'OLTP', 'DW' for Data Warehouse, 'AJD' for Autonomous JSON Database).

---

#### **4. Writing Conditions: Syntax and Logic**

##### **Single Conditions**
Use basic comparison operators for a single rule.
*   **Equals:** `=`
*   **Not Equals:** `!=`
*   **Example:** `WHERE request.region = 'us-phoenix-1'`

##### **Multiple Conditions (Combining Logic)**
Use the `any` and `all` keywords to combine multiple conditions.

*   **`any` { ... } - Logical OR**
    *   The overall condition is **True** if **ANY one** of the sub-conditions inside the braces is true.
    *   **Example:**
        ```plaintext
        WHERE any {request.permission = 'OBJECT_INSPECT', request.permission = 'OBJECT_CREATE'}
        ```
        This allows the action if the requested permission is *either* `OBJECT_INSPECT` *or* `OBJECT_CREATE`.

*   **`all` { ... } - Logical AND**
    *   The overall condition is **True** only if **ALL** of the sub-conditions inside the braces are true.
    *   **Example:**
        ```plaintext
        WHERE all {target.compartment.name != 'Secure-Compartment', request.region = 'uk-london-1'}
        ```
        This allows the action only if the target is *not* in 'Secure-Compartment' *and* the request is for the 'uk-london-1' region.

##### **Working with Values**
*   **String Values:** Must be enclosed in **single quotes** (e.g., `'us-phoenix-1'`, `'HR-Projects'`).
*   **Pattern Matching:** You can use wildcards for string matching (e.g., `'HR-*'` to match any string starting with "HR-").

---

#### **5. Practical Policy Examples**

##### **Example 1: Restrict Access by Region**
*   **Scenario:** Allow a group to manage resources, but only in the Phoenix region.
*   **Policy:**
    ```plaintext
    ALLOW GROUP Phoenix-Admins TO MANAGE all-resources IN TENANCY
    WHERE request.region = 'us-phoenix-1'
    ```
*   **Effect:** Users in this group cannot manage resources in any other region, even if they are in a permitted compartment.

##### **Example 2: Restrict Access to a Specific Compartment**
*   **Scenario:** Allow network admins to manage virtual networks across the tenancy, but **explicitly deny** them access to a highly secure compartment.
*   **Policy:**
    ```plaintext
    ALLOW GROUP Network-Admins TO MANAGE virtual-network-family IN TENANCY
    WHERE target.compartment.id != 'ocid1.compartment.unique_id_here'
    ```
*   **Effect:** The group has manage permissions on VCNs in all compartments *except* the one with the specified OCID.

##### **Example 3: Limit Permissions with `request.permission`**
*   **Scenario:** Grant a group broad "manage" verb on objects, but restrict it to only the `create` and `inspect` permissions, preventing them from deleting or updating.
*   **Policy:**
    ```plaintext
    ALLOW GROUP Data-Uploaders TO MANAGE objects IN COMPARTMENT Data-Lake
    WHERE request.permission IN ('OBJECT_CREATE', 'OBJECT_INSPECT')
    ```
    *Alternatively using `any`:*
    ```plaintext
    WHERE any {request.permission = 'OBJECT_CREATE', request.permission = 'OBJECT_INSPECT'}
    ```

##### **Example 4: Allow Specific Workload Types**
*   **Scenario:** Allow DB admins to manage Autonomous Databases, but only of certain workload types.
*   **Policy:**
    ```plaintext
    ALLOW GROUP DB-Admins TO MANAGE autonomous-database IN TENANCY
    WHERE target.database.workloadType IN ('OLTP', 'AJD')
    ```

---

#### **Key Terminology & Facts to Memorize**

*   The `WHERE` clause is used to add conditions to a policy.
*   **`request`** variables relate to **who is making the request and how**.
*   **`target`** variables relate to **what resource is being accessed**.
*   **`any`** acts as a logical **OR**.
*   **`all`** acts as a logical **AND**.
*   String values in conditions **must be in single quotes**.
*   Use `request.permission` to grant specific granular permissions instead of the full verb bundle.