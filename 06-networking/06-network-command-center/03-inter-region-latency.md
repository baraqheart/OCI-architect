### **OCI Study Notes: Inter-Region Latency Dashboard**

#### **1. Foundation: The OCI Backbone Network**

*   OCI operates a **global private IP network** known as the OCI backbone.
*   This backbone is used for all **inter-region communication** between OCI workloads.
*   The Inter-Region Latency Dashboard reflects the traffic and performance of this global backbone.

#### **2. What the Dashboard Provides**

*   **Purpose:** Displays the **average inter-region round-trip latency** for all pairs of regions within an OCI realm (e.g., the commercial realm).
*   **Scope:** The data is **not specific to your tenancy**. It provides aggregate, global latency statistics for the OCI backbone itself.
*   **Availability:** This dashboard is **not available in realms that contain only one region**.

#### **3. The Two Key Charts**

**A. Current Inter-Region Round-Trip Time**
*   **What it is:** A **real-time snapshot** of latency, expressed in milliseconds (ms).
*   **Data Calculation:** The value shown is an **average of measurements taken over the last five minutes**.
*   **Update Frequency:** The view **updates every minute**.
*   **How to Read:** The dashboard presents a matrix. You find the source region (e.g., Germany Central - Frankfurt) and the destination region (e.g., US East - Ashburn), and the intersecting cell shows the current average latency (e.g., **97.24 ms**).

**B. Inter-Region Round-Trip Time (Historical)**
*   **What it is:** A **historical view** of latency over a defined period.
*   **Time Range:** Shows data for the **last 30 days**.
*   **How to Use:** You select a specific source and destination region pair to see a trend graph of their latency over the past month. This helps in validating long-term network performance.

#### **4. Primary Use Cases**

*   **Network and Region Planning:**
    *   When expanding your business or deploying new applications, you can use the dashboard to **evaluate the performance impact** of selecting different regions.
    *   It helps in making an **optimal region selection** based on latency, complementing other factors like disaster recovery, compliance, and data residency requirements.

*   **Troubleshooting Performance Issues:**
    *   Allows you to **validate network performance** in real-time and historically.
    *   If users report slowness, you can check the dashboard to see if there is a corresponding increase in backbone latency between the relevant regions.

*   **Proactive Monitoring and Integration:**
    *   The latency metrics are available via the OCI **Monitoring API**.
    *   You can **integrate these metrics into your on-premises or custom monitoring platforms** to set up alerts and be notified automatically when latency exceeds defined thresholds.

    Welcome to this demonstration on inter-region latency. So let's get started. I'll take you to the OCI Console, and I'll click on the Navigation menu. Under Networking, if you see here, there is something called Network Command Center, so I'll click on it. So this is where you will find all the services related to the Network Command Center. Under Visualizations and Dashboards, you will see Inter-region latency. So let me click on it.

So this is the Inter-Region Latency dashboard, and it basically consists of two charts. The first chart is this one, Current Inter-Region Round Trip Time in milliseconds. So this is the first one. And let me show you the second one as well, Inter-Region Round Trip Time milliseconds for the last 30 days.

So first, let me take you to this particular chart. And this is where you provide or select the region. For example, let me select Phoenix and Mumbai. So I want to see the average network round-trip latency between Phoenix and Mumbai. The moment I click on Show-- so this is your Phoenix and this is your Mumbai.

And you can see that currently, it is 247.82. So this 247.82 is an average of the values over the last 5 minutes. And if you see here, you also see that the new measurements are captured every minute. All right, so this is the first chart.

Let me go to the second chart, which is Inter-Region Round Trip Time for the last 30 days. So this is going to show you the historical view of the last 30 days. So again, I will just search for Phoenix. And here, I will type Mumbai. And I can click on Show.

So you see, this is the historical view of the last 30 days. So that's pretty much it about this dashboard. Thanks for watching.