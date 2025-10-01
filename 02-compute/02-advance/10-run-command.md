### **OCI Study Notes: Run Command**

#### **1. Core Definition and Purpose**

*   **What it is:** A feature that allows you to **remotely execute scripts and commands** on your compute instances.
*   **Key Advantage:** It works **even if the instance has no SSH access or open inbound ports**, providing a crucial out-of-band management capability.

#### **2. Supported Operating Systems**

*   **Supported:**
    *   Oracle Autonomous Linux
    *   Oracle Linux
    *   CentOS
    *   Windows Server
*   **Not Supported:** Ubuntu

#### **3. Execution Environment**

*   **Linux Instances:** Scripts are executed in a **bash shell**.
*   **Windows Instances:** Scripts are executed as a **batch file**.

#### **4. Script and Output Limitations**

*   **Script Size Limit (Direct Input):** The maximum size for a script pasted directly into the console is **4 KB**.
*   **Workaround for Larger Scripts:** Store the script file in **OCI Object Storage** and reference it from the Run Command.
*   **Output Size Limit (Console):** The console output is truncated to the **last 1 KB** of plain text.
*   **Workaround for Full Output:** Configure the script to save the complete output directly to an **OCI Object Storage** bucket.

#### **5. Common Use Cases**

*   **Automating Configuration:** Automating tasks like configuring secondary Virtual Network Interface Cards (vNICs).
*   **Troubleshooting:** Diagnosing issues such as SSH connectivity problems without needing network access to the instance.