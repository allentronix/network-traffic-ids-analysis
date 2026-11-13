# Network Traffic & IDS Analysis

This is my hands-on project for learning **network traffic analysis, intrusion detection, and security investigation** with Wireshark and Suricata.

I use public sample PCAPs and an isolated lab environment for the investigations documented here. I examine what happened in the traffic, compare Suricata's output with packet evidence where relevant, and record what I can and cannot conclude from each capture.

## Objectives

- Build my packet analysis skills with Wireshark.
- Learn how TCP, UDP, DNS, HTTP, and TLS appear in packet captures.
- Investigate network activity and look for traffic that may need a closer look.
- Learn how Suricata analyzes traffic and produces alerts.
- Check IDS findings against the packets in the capture.
- Separate what I observed from what I am assuming.
- Practise writing clear investigation reports.

## Tools & Technologies

| Tool         | Purpose                                          |
| ------------ | ------------------------------------------------ |
| Wireshark    | Network packet analysis and protocol inspection  |
| Suricata     | Network intrusion detection and traffic analysis |
| Ubuntu       | Linux security and lab environment               |
| Python       | Network analysis automation and scripting        |
| Git & GitHub | Version control and investigation documentation  |

## Repository Structure

```text
network-traffic-ids-analysis/
├── docs/
│   ├── investigation-report.md
│   ├── lessons-learned.md
│   ├── network-fundamentals.md
│   ├── suricata-analysis.md
│   └── wireshark-analysis.md
├── evidence/
│   ├── alerts/
│   │   └── investigation-003/
│   │       ├── eve.json
│   │       ├── fast.log
│   │       ├── stats.log
│   │       └── suricata.log
│   └── screenshots/
├── pcaps/
│   ├── README.md
│   ├── http.cap
│   └── dns.cap
├── reports/
│   ├── README.md
│   ├── investigation-001-http-traffic.md
│   ├── investigation-002-dns-traffic.md
│   ├── investigation-003-suricata-http-analysis.md
│   └── investigation-004-tcp-connection-analysis.md
└── README.md
```

## Investigations

| ID  | Investigation                            | Focus                                                                | Status    |
| --- | ---------------------------------------- | -------------------------------------------------------------------- | --------- |
| 001 | HTTP Traffic Analysis                    | HTTP requests, responses, headers, and unencrypted traffic           | Completed |
| 002 | DNS Traffic Analysis                     | DNS queries, responses, record types, and NXDOMAIN                   | Completed |
| 003 | Suricata HTTP Analysis                   | IDS execution, EVE JSON events, and alert validation                 | Completed |
| 004 | TCP Connection & Retransmission Analysis | TCP handshake, sequence numbers, retransmissions, and duplicate ACKs | Completed |
| 005 | Network Port Scan Analysis               | TCP scanning behavior and port identification                        | Planned   |
| 006 | Suspicious HTTP Traffic Analysis         | Investigating potentially malicious HTTP behavior                    | Planned   |
| 007 | TLS Traffic Analysis                     | TLS handshakes, certificates, and encrypted communication            | Planned   |
| 008 | Suspicious Network Communication         | Investigating unusual communication patterns                         | Planned   |

## How I approach an investigation

I generally work through these steps:

1. I check what the capture contains and decide what I want to find out.
2. I inspect packets, protocols, endpoints, and conversation patterns in Wireshark.
3. I put relevant events in order so I can follow what happened.
4. When it fits the investigation, I run the PCAP through Suricata.
5. I compare any IDS alerts with the packet evidence.
6. I save useful screenshots, logs, and command output.
7. I document my findings, interpretation, and any limits of the analysis.
8. I track the investigation files with Git.

## Completed Investigation Highlights

### Investigation 001 — HTTP Traffic Analysis

I examined HTTP communication between a client and web server.

Key observations:

- HTTP GET requests and server responses.
- HTTP request and response headers.
- Server information and response metadata.
- Unencrypted HTTP application data.

### Investigation 002 — DNS Traffic Analysis

I examined DNS queries and responses.

Key observations:

- DNS A, AAAA, MX, TXT, LOC, and PTR records.
- DNS response codes.
- Successful name resolution.
- NXDOMAIN responses.
- DNS response timing and TTL values.

### Investigation 003 — Suricata HTTP Analysis

I ran Suricata against an HTTP sample PCAP and looked through the output it generated.

Key observations:

- Suricata offline PCAP analysis.
- EVE JSON event inspection.
- HTTP event extraction.
- DNS, flow, and file information events.
- Validation of IDS output against Wireshark.
- No alerts generated for the analyzed capture.

### Investigation 004 — TCP Connection & Retransmission Analysis

I followed TCP connection setup and looked at transport-layer behavior in the capture.

Key observations:

- TCP three-way handshake.
- SYN, SYN-ACK, and ACK flags.
- TCP sequence and acknowledgement numbers.
- TCP payload versus frame size.
- Suspected spurious retransmission.
- Duplicate acknowledgement analysis.
- Wireshark TCP analysis flags.

## PCAP Inventory

| Capture    | Source                    | Purpose                                      |
| ---------- | ------------------------- | -------------------------------------------- |
| `http.cap` | Wireshark Sample Captures | HTTP traffic analysis and TCP investigations |
| `dns.cap`  | Wireshark Sample Captures | DNS query and response analysis              |

PCAP metadata, download sources, and verification information are maintained in [`pcaps/README.md`](pcaps/README.md).

## Evidence & Documentation

- **Reports:** Investigation findings, timelines, technical analysis, conclusions, and limitations.
- **Screenshots:** Wireshark packet analysis and investigation evidence.
- **Alerts:** Suricata EVE JSON and supporting logs.
- **Documentation:** Network fundamentals, Wireshark techniques, Suricata analysis, and lessons learned.

I keep the evidence alongside the reports so I can show what supports my findings and revisit the analysis later.

## Security & Ethical Considerations

I use public sample PCAPs or an isolated lab environment for this project.

- I do not scan networks or access systems without authorization.
- I do not intercept network traffic without permission.
- I do not execute malware that might be found in a capture.
- I base my reports on the available evidence and label assumptions as such.

## Project Status

**Current stage:** Working through network traffic and IDS investigations.

I have completed and documented four investigations so far:

- HTTP Traffic Analysis
- DNS Traffic Analysis
- Suricata HTTP Analysis
- TCP Connection & Retransmission Analysis

Next, I plan to look at **Network Port Scan Analysis**, then continue with other protocols and IDS exercises.

## Learning Outcomes

Through this project, I am building experience with:

- Network protocol analysis.
- TCP/IP troubleshooting.
- DNS and HTTP investigation.
- Wireshark display filters.
- Suricata IDS operation.
- IDS alert validation.
- Network anomaly investigation.
- Evidence collection and reporting.
- Security investigation methodology.

---

**What I am learning:** Network Security | Traffic Analysis | Intrusion Detection | SOC Investigation
