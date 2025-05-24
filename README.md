<p align="center">
  <a href="https://github.com/Samuel-Cavada" target="_blank">
    <img src="https://img.shields.io/badge/Back_to_Main_Page-000000?style=for-the-badge&logo=github&logoColor=white" alt="Back to Main Page"/>
  </a>
</p>

<h1 align="center">Inspecting NSG Logs</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Cloud Platform" />
  <img src="https://img.shields.io/badge/OS-N/A-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="OS" />
  <img src="https://img.shields.io/badge/Tool-Azure%20Network%20Watch-00B388?style=for-the-badge&logo=windows&logoColor=white" alt="Tool" />
  <img src="https://img.shields.io/badge/Tool-PowerShell-2C5EA8?style=for-the-badge&logo=powershell&logoColor=white" alt="Tool" />
  <img src="https://img.shields.io/badge/Focus-NSG%20Traffic%20Analysis-orange?style=for-the-badge" alt="Focus Area" />
</p>

---

## 📌 Project Objective
> Query and analyze NSG (Network Security Group) flow logs in Azure using KQL to detect potentially malicious traffic targeting a virtual machine. This project focuses on interpreting network-level telemetry data for security monitoring.

---

## 🧰 Tools & Technologies
- **Platform:** Azure
- **OS:** N/A
- **Tools:** Azure Network Watcher, Azure Monitor, Log Analytics, PowerShell
- **Languages/Scripts:** KQL

---

## 🧠 Skills Gained / Focus Areas
- Queried NSG flow logs using the AzureNetworkAnalytics_CL table
- Identified flows classified as "MaliciousFlow"
- Filtered logs by VM IP and summarized by source IP
- Built a query to count and rank suspicious traffic

---

## 🧪 Environment Setup
> NSG flow logs were enabled and sent to a Log Analytics workspace. The AzureNetworkAnalytics_CL table was queried to analyze network events for a specific virtual machine.

![Environment Setup](assets/images/setup.jpg)

---

## 🛠️ Walkthrough
1. [Step 1: Enable NSG Logs](#step-1-enable-nsg-logs)
2. [Step 2: Run Malicious Flow Query](#step-2-run-malicious-flow-query)
3. [Step 3: Interpret Results](#step-3-interpret-results)

---

### ✅ Step 1: Enable NSG Logs
> - Enabled NSG flow logs for relevant NSGs via Azure Network Watcher  
> - Configured log analytics workspace as the destination  
> - Confirmed data was appearing in AzureNetworkAnalytics_CL table

![Step 1](assets/images/step1.jpg)

---

### ✅ Step 2: Run Malicious Flow Query
> Executed the following KQL query to identify suspicious traffic:

```kql
let my_vm_ip_address = "10.0.0.5";
AzureNetworkAnalytics_CL
| where FlowType_s == "MaliciousFlow" and TimeGenerated > ago(5h) and DestIP_s in (my_vm_ip_address)
| summarize flow_count = count() by SrcIP_s, DestIP_s, FlowType_s
| order by flow_count
```

> **Purpose:**
> - Focus on traffic marked as "MaliciousFlow"
> - Narrow scope to a specific destination IP (VM)
> - Count and rank flows by source IP

---

### ✅ Step 3: Interpret Results
> - Identified top offending source IPs targeting the VM  
> - Used data for further investigation or alerting  
> - Practiced adjusting time window and flow type filters

![Step 3](assets/images/step3.jpg)

---

## 📝 Outcomes and Lessons Learned
- **Technical Insight:** NSG flow logs provide visibility into network behavior and can identify malicious actors.
- **Configuration Skills:** Enabled flow log diagnostics and queried using Azure Monitor.
- **Troubleshooting:** Refined filters to ensure data accuracy for the target IP and timeframe.
- **Takeaway:** Monitoring flow logs is essential for detecting lateral movement, port scanning, or brute-force attempts.

---

## 📎 References
- [NSG Flow Logs Overview](https://learn.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview)
- [KQL Query Language Documentation](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Azure Network Watcher](https://learn.microsoft.com/en-us/azure/network-watcher/)
