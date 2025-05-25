# 🛰️ DNS Failure Investigation with Tcpdump & ICMP Analysis  
_A hands-on incident report demonstrating network-traffic analysis_

---

## 1&nbsp; Project Context  

When several customers were unable to reach **`www.yummyrecipesforme.com`** (error: **“destination port unreachable”**), a quick triage with **tcpdump** revealed ICMP errors pointing to **UDP port 53**.  
This project walks through:

1. Capturing DNS / ICMP traffic with tcpdump  
2. Interpreting the log  
3. Writing an incident-response summary and recommending next steps  

---

## 2&nbsp; Scenario (condensed)

| Role | Detail |
|------|--------|
| **Analyst** | Cybersecurity analyst for a managed-services provider |
| **Symptom** | Users receive “destination port unreachable” when loading the client’s site |
| **Tool** | `tcpdump` packet capture |
| **Key Finding** | ICMP errors: `udp port 53 unreachable` from DNS server `203.0.113.2` |

---

## 3&nbsp; Summary of tcpdump Analysis (Part 1)

- **Protocols observed:**  
  - **UDP → DNS** request (client → 203.0.113.2:53)  
  - **ICMP Destination Unreachable** response (203.0.113.2 → client)
- **Error string:** `udp port 53 unreachable`
- **Interpretation:**  
  - DNS service on the authoritative server is not listening or is blocked.  
  - HTTPS never initiates because name resolution fails.

---

## 4&nbsp; Incident Timeline & Findings (Part 2)

| 📅 Time (24-h) | Event |
|---------------|-------|
| **13:24:32**  | First customer reports error; analyst reproduces issue |
| **13:26**     | tcpdump capture started; multiple ICMP port-unreachable replies logged |
| **13:30**     | Preliminary conclusion: DNS port 53 unreachable |

**Likely causes considered**

1. **DNS service outage** (software crash or maintenance)  
2. **Firewall rule change / misconfiguration** blocking UDP 53  
3. **DDoS or DoS** exhausting DNS resources

**Next steps**

1. Verify DNS service status on `203.0.113.2`.  
2. Check firewall or ACL changes affecting UDP 53.  
3. Inspect upstream network devices for rate-limiting or drops.  
4. If service is up, capture on the DNS host to confirm whether packets arrive.

---

## 5&nbsp; Recommendations

| Priority | Recommendation | Rationale |
|----------|----------------|-----------|
| 🔒 P1 | Restore DNS service or open UDP 53 | Re-enable name resolution & site access |
| 🛡️ P2 | Implement monitoring on critical DNS ports | Early detection of service drops |
| 📈 P3 | Capture baseline DNS traffic & alert on anomalies | Faster incident response next time |

---

## 6&nbsp; Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | Project overview (this file) |
| `Cybersecurity_incident_report_template.DOCX` | Formal incident-response report template |
| `Example_of_a_Cybersecurity_Incident_Report.DOCX` | An Example of a Cybersecurity Incident Report |
| `tcpdump_log.png` | Annotated screenshot of packet capture |




---

## 7&nbsp; License  

Content provided for portfolio demonstration only. Scenario and data are fictional.
