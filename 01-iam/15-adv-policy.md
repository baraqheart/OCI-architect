### **Study Notes: OCI Advanced Policies - Granular Permissions**

---

#### **1. Understanding Permissions: The Atomic Level**

Policies use easy-to-understand verbs like `read` and `manage`, but behind these verbs are specific, granular permissions.

*   **What are Permissions?** Permissions are the **atomic units of authorization**. Each permission corresponds directly to one or a few specific API operations.
*   **How Verbs Work:** A verb like `manage` is a **bundle of multiple granular permissions**. Using a verb grants all the permissions in that bundle at once, simplifying policy writing but potentially granting more access than needed.

**Example: Block Volume Permissions**

| Verb Used in Policy | Granular Permissions Granted (Behind the Scenes) | Example API Operations Allowed |
| :--- | :--- | :--- |
| **INSPECT** volumes | `VOLUME_INSPECT` | ListVolumes, GetVolume |
| **READ** volumes | `VOLUME_INSPECT`, `VOLUME_READ` | ListVolumes, GetVolume, GetVolumeBackup |
| **USE** volumes | `VOLUME_INSPECT`, `VOLUME_UPDATE`, `VOLUME_ATTACHMENT_WRITE` | ListVolumes, GetVolume, UpdateVolume, AttachVolume |
| **MANAGE** volumes | `VOLUME_INSPECT`, `VOLUME_READ`, `VOLUME_UPDATE`, `VOLUME_CREATE`, `VOLUME_DELETE`, `VOLUME_MOVE` | ListVolumes, GetVolume, UpdateVolume, CreateVolume, DeleteVolume, MoveVolume |

---

#### **2. The Need for Advanced Policies**

Using broad verbs like `manage` often violates the **principle of least privilege** by granting unnecessary permissions. Advanced policies allow you to:

*   Grant **specific individual permissions** instead of a broad verb bundle.
*   Restrict access to **specific resources** by name or other attributes.
*   Add **conditions** based on time, specific API operations, or other context.

---

#### **3. Techniques for Writing Advanced Policies**

##### **A. Granting Specific Permissions**

Instead of using a verb, you can list the exact permissions you want to grant using the `request.permission` condition.

*   **Policy Goal:** Allow a group to list, create, update, and move block volumes, but **NOT delete them**.
*   **Standard Verb Problem:** Using `MANAGE volumes` would include the `VOLUME_DELETE` permission.
*   **Advanced Policy Solution:**
    *   `ALLOW GROUP XYZ TO MANAGE volumes IN TENANCY WHERE request.permission IN ('VOLUME_CREATE', 'VOLUME_INSPECT', 'VOLUME_UPDATE', 'VOLUME_MOVE')`
    *   **Alternative using "NOT":** `ALLOW GROUP XYZ TO MANAGE volumes IN TENANCY WHERE request.permission != 'VOLUME_DELETE'`

##### **B. Restricting Access to Specific Resources**

Use the `target.resource.attribute` condition to limit a policy to a specific resource instance.

*   **Policy Goal:** Allow the "Audit" group to create objects, but **only in a specific bucket named "financial-records"**.
*   **Standard Policy Problem:** A policy for `manage objects` would apply to all buckets in the compartment.
*   **Advanced Policy Solution:**
    *   `ALLOW GROUP Audit TO MANAGE objects IN COMPARTMENT Acme WHERE target.bucket.name = 'financial-records' AND request.permission = 'OBJECT_CREATE'`

##### **C. Using API Operations as a Condition**

You can also restrict policies based on the specific API operation being called using `request.operation`.

*   **Policy Goal:** Allow a group to only inspect and create objects.
*   **Advanced Policy Solution:**
    *   `ALLOW GROUP Dev TO USE objects IN COMPARTMENT Acme WHERE request.operation IN ('ListObjects', 'GetObject', 'PutObject')`

##### **D. Implementing Time-Based Access**

You can restrict when a policy is active using time-based conditions.

*   **Policy Goal:** Allow a "Contractors" group to use instances, but **only during business hours on weekdays**.
*   **Advanced Policy Solution:**
    *   `ALLOW GROUP Contractors TO USE instances IN TENANCY WHERE request.time.ofDay >= "09:00" AND request.time.ofDay < "17:00" AND request.day.ofWeek NOT IN {'SATURDAY', 'SUNDAY'}`

---

#### **Key Terminology & Facts to Memorize**

*   **Permissions** are the specific, granular rights (e.g., `VOLUME_CREATE`) that correspond to API operations.
*   **Verbs** (like `manage`) are convenient bundles of permissions.
*   Use **Advanced Policies** to enforce the **principle of least privilege** by granting only the exact permissions needed.
*   **`request.permission`** is used in a `WHERE` clause to specify individual granular permissions.
*   **`target.resource.attribute`** (e.g., `target.bucket.name`) is used to restrict a policy to a specific resource.
*   **`request.operation`** can be used to allowlist specific API calls.
*   **Time-based conditions** (`request.time`, `request.day`) can restrict when a policy is active.