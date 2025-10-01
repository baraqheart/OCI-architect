### **Study Notes: OCI Reserved Public IP Addresses - Persistent Addressing**

---

#### **1. Core Characteristics of Reserved Public IPs**

**Reserved Public IP Addresses** provide **persistent, tenant-owned** public IP addresses that exist independently of resource lifecycles.

*   **Persistence:** Exists **beyond resource lifetime** - not tied to instance termination
*   **Tenancy Ownership:** Allocated to your **OCI tenancy** until explicitly released
*   **Regional Scope:** Available across **all Availability Domains** within the region
*   **Reassignable:** Can be moved between resources as needed

---

#### **2. Reserved Public IP Lifecycle Management**

##### **A. Creation Process:**
*   **Manual Creation:** Created **one at a time** (not in bulk)
*   **IP Source:** Allocated from **Oracle's public IP pool**
*   **Compartment Scope:** Created within specific compartments for organization

##### **B. Assignment Flexibility:**
*   **Target Resources:** Can be assigned to **any resource** supporting public IPs
*   **Private IP Types:** Works with **both primary and secondary** private IPs
*   **Cross-AD Assignment:** Can assign to resources in **any Availability Domain** within the region

##### **C. Persistence Behavior:**
*   **No Automatic Deletion:** Remains in tenancy until **manually deleted**
*   **Unassignment:** Returns to **tenancy's reserved IP pool** when unassigned
*   **Reusability:** Can be repeatedly assigned/unassigned to different resources

---

#### **3. Service Limits and Constraints**

##### **A. Regional Limits:**
*   **Maximum per Region:** **50 reserved public IP addresses**
*   **Planning Consideration:** Design within this constraint for regional architectures

##### **B. Movement with Private IPs:**
*   **IP Association:** Reserved public IP is tied to the **private IP address**
*   **VNIC Migration:** If private IP moves between VNICs, the reserved public IP **moves with it**
*   **Instance Migration:** Supports seamless IP preservation during instance upgrades/replacements

---

#### **4. Practical Implementation Scenarios**

##### **Scenario A: Instance Replacement with IP Preservation**
```
Initial State:
- Instance A: Private IP 10.0.1.10 → Reserved Public IP 203.0.113.100
- Instance A is terminated

Recovery Process:
1. Reserved Public IP 203.0.113.100 returns to tenancy pool
2. Launch Instance B with Private IP 10.0.1.20
3. Reassign Reserved Public IP 203.0.113.100 to Instance B
4. DNS/Users continue using same public IP - zero impact
```

##### **Scenario B: Multi-Service Hosting**
```
Web Server Instance:
├── Primary VNIC:
│   ├── Primary Private IP: 10.0.1.10 → Reserved Public IP: 203.0.113.10 (Website)
│   ├── Secondary Private IP 1: 10.0.1.11 → Reserved Public IP: 203.0.113.11 (API)
│   └── Secondary Private IP 2: 10.0.1.12 → Reserved Public IP: 203.0.113.12 (Admin)
└── All IPs persist through instance lifecycle changes
```

##### **Scenario C: Load Balancer IP Management**
```
Application Load Balancer:
- Reserved Public IP: 203.0.113.50 (production endpoint)
- During maintenance: Unassign from old load balancer
- Assign same IP to new load balancer
- No DNS changes required for users
```

---

#### **5. Security and Operational Benefits**

##### **A. Risk Reduction:**
*   **Stable Endpoints:** Consistent IP addresses for security policies and firewall rules
*   **Controlled Exposure:** Known IP addresses for security monitoring
*   **Audit Trail:** Persistent IPs simplify security logging and analysis

##### **B. Operational Efficiency:**
*   **Disaster Recovery:** Quick IP reassignment during failover scenarios
*   **Maintenance Windows:** Seamless IP migration during upgrades
*   **DNS Management:** Stable IPs reduce DNS record updates

---

#### **6. Implementation Best Practices**

##### **A. Naming and Organization:**
*   **Descriptive Names:** Use meaningful names indicating purpose (e.g., "prod-web-ip", "api-endpoint-ip")
*   **Compartment Strategy:** Organize reserved IPs by application, environment, or team
*   **Documentation:** Maintain IP allocation records with business justification

##### **B. Lifecycle Management:**
*   **Regular Review:** Periodically audit reserved IP assignments and usage
*   **Cleanup Process:** Establish procedures for releasing unused reserved IPs
*   **Assignment Tracking:** Monitor IP utilization against the 50-IP regional limit

##### **C. Architectural Planning:**
*   **IP Conservation:** Use reserved IPs strategically for critical services
*   **Failover Planning:** Design IP reassignment procedures for high availability
*   **Capacity Planning:** Monitor regional IP usage against the 50-IP limit

---

#### **7. Cost Considerations**

*   **No Additional Cost:** Reserved public IPs are **free of charge** (as noted in previous lessons)
*   **Unassigned IPs:** No penalty for keeping reserved IPs in pool without assignment
*   **Budget Impact:** No financial constraints on reserving IPs for future use

---

#### **Key Operational Advantages**

*   **Business Continuity:** Maintain service endpoints during infrastructure changes
*   **Operational Flexibility:** Reassign IPs without service disruption
*   **Security Stability:** Consistent IP addresses for firewall and monitoring rules
*   **DNS Simplicity:** Reduce frequency of DNS record updates
*   **Cost Efficiency:** No charges for persistent IP reservation

**Remember:** Reserved public IPs provide the **foundation for stable, enterprise-grade networking** by offering persistent IP addresses that survive resource lifecycles, enabling seamless maintenance, disaster recovery, and long-term service stability.