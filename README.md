# Network Traffic & IDS Analysis

I started this project to practise **network traffic analysis, intrusion detection, and security investigation** using Wireshark and Suricata.

I use public sample PCAPs and an isolated lab for the investigations here. I look through the traffic, use Suricata when it fits the investigation, and write down what I found and what I couldn't confirm.

## Objectives

- Get more practice analyzing packets in Wireshark.
- Learn to recognize TCP, UDP, DNS, HTTP, and TLS traffic in captures.
- Look into network activity that seems unusual or needs more checking.
- Learn how Suricata reads traffic and creates alerts.
- Check IDS alerts against the packets that triggered them.
- Be clear about what I saw and what I am assuming.
- Get better at writing up my investigations.

## Tools & Technologies

| Tool         | Purpose                                             |
| ------------ | --------------------------------------------------- |
| Wireshark    | Network packet analysis and protocol inspection     |
| TShark       | Command-line packet analysis and capture inspection |
| Suricata     | Network intrusion detection and traffic analysis    |
| Ubuntu       | Linux security and lab environment                  |
| Python       | Network analysis automation and scripting           |
| jq           | JSON parsing and EVE log analysis                   |
| Git & GitHub | Version control and investigation documentation     |

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
│   ├── dns.cap
│   └── investigation-005/
│       └── 03-tcp-syn-scan.pcapng
├── reports/
│   ├── README.md
│   ├── investigation-001-http-traffic.md
│   ├── investigation-002-dns-traffic.md
│   ├── investigation-003-suricata-http-analysis.md
│   ├── investigation-004-tcp-connection-analysis.md
│   └── investigation-005-tcp-syn-scan-analysis.md
└── README.md
```

## Investigations

| ID  | Investigation                            | Focus                                                                                  | Status    |
| --- | ---------------------------------------- | -------------------------------------------------------------------------------------- | --------- |
| 001 | HTTP Traffic Analysis                    | HTTP requests, responses, headers, and unencrypted traffic                             | Completed |
| 002 | DNS Traffic Analysis                     | DNS queries, responses, record types, and NXDOMAIN                                     | Completed |
| 003 | Suricata HTTP Analysis                   | IDS execution, EVE JSON events, and alert validation                                   | Completed |
| 004 | TCP Connection & Retransmission Analysis | TCP handshake, sequence numbers, retransmissions, and duplicate ACKs                   | Completed |
| 005 | TCP SYN Scan Analysis                    | TCP scanning behavior, SYN-ACK responses, RST responses, and port-state classification | Completed |
| 006 | Suspicious HTTP Traffic Analysis         | Investigating potentially malicious HTTP behavior                                      | Planned   |
| 007 | TLS Traffic Analysis                     | TLS handshakes, certificates, and encrypted communication                              | Planned   |
| 008 | Suspicious Network Communication         | Investigating unusual communication patterns                                           | Planned   |

## How I Approach an Investigation

For each investigation, I usually:

1. Check what is in the capture and decide what I want to find out.
2. Look at the packets, protocols, addresses, and conversations in Wireshark.
3. Put the relevant packets in order to see what happened.
4. Run the PCAP through Suricata if it is useful for that investigation.
5. Compare any alerts with the packet evidence.
6. Save screenshots, logs, and command output that help explain my findings.
7. Write down what I found and any limits to what I can conclude.
8. Keep the investigation files in Git.

## Completed Investigation Highlights

### Investigation 001 — HTTP Traffic Analysis

I looked at HTTP traffic between a client and a web server.

Key observations:

- HTTP GET requests and server responses.
- HTTP request and response headers.
- Server information and response metadata.
- Unencrypted HTTP application data.

### Investigation 002 — DNS Traffic Analysis

I looked at DNS queries and the responses they received.

Key observations:

- DNS A, AAAA, MX, TXT, LOC, and PTR records.
- DNS response codes.
- Successful name resolution.
- NXDOMAIN responses.
- DNS response timing and TTL values.

### Investigation 003 — Suricata HTTP Analysis

I ran Suricata on an HTTP sample capture and went through the output it produced.

Key observations:

- Suricata offline PCAP analysis.
- EVE JSON event inspection.
- HTTP event extraction.
- DNS, flow, and file information events.
- Validation of IDS output against Wireshark.
- No alerts generated for the analyzed capture.

### Investigation 004 — TCP Connection & Retransmission Analysis

I followed the TCP connections in the capture and looked at how the packets behaved.

Key observations:

- TCP three-way handshake.
- SYN, SYN-ACK, and ACK flags.
- TCP sequence and acknowledgement numbers.
- TCP payload versus frame size.
- Suspected spurious retransmission.
- Duplicate acknowledgement analysis.
- Wireshark TCP analysis flags.

### Investigation 005 — TCP SYN Scan Analysis

I looked through a TCP SYN scan capture to see how the scan appeared in the traffic.

Key observations:

- I identified the scanner and target IP addresses.
- I followed the TCP SYN probes and the target's replies.
- I found five ports that replied with SYN-ACK packets.
- I found one port that replied with a RST-ACK, indicating it was closed.
- I checked that the RST packets after the SYN-ACKs came from the scanner.
- I found two probes to TCP/3389 with no response in the capture, so I couldn't confirm its state.
- I used Wireshark and TShark to inspect the packets.

## PCAP Inventory

| Capture                  | Source                             | Purpose                                      |
| ------------------------ | ---------------------------------- | -------------------------------------------- |
| `http.cap`               | Wireshark Sample Captures          | HTTP traffic analysis and TCP investigations |
| `dns.cap`                | Wireshark Sample Captures          | DNS query and response analysis              |
| `03-tcp-syn-scan.pcapng` | CEH educational capture collection | TCP SYN scan and port-state analysis         |

I keep PCAP details, sources, and verification notes in [`pcaps/README.md`](pcaps/README.md).

## Evidence & Documentation

- **Reports:** My findings, packet timelines, conclusions, and notes about what I couldn't confirm.
- **Screenshots:** Wireshark views that support my analysis.
- **Alerts:** Suricata EVE JSON output and related logs.
- **Documentation:** Notes I have made about networking, Wireshark, Suricata, and what I have learned.

I keep the evidence with the reports so I can refer back to the packets behind my findings.

## Security & Ethical Considerations

I use public sample PCAPs or work in an isolated lab for this project.

- I only scan networks or access systems when I have permission.
- I do not capture other people's network traffic without permission.
- I do not run malware that might be present in a capture.
- I base my reports on the packets I can see and say when something is an assumption.

## Project Status

**Current stage:** I am working through network traffic and IDS investigations.

I have completed and documented five investigations:

- HTTP Traffic Analysis
- DNS Traffic Analysis
- Suricata HTTP Analysis
- TCP Connection & Retransmission Analysis
- TCP SYN Scan Analysis

Next, I plan to look at suspicious HTTP traffic, TLS communication, and other network activity.

## Learning Outcomes

By working through these captures, I am getting practice with:

- Network protocol analysis.
- TCP/IP troubleshooting.
- DNS and HTTP investigation.
- Wireshark display filters.
- Command-line packet analysis using TShark.
- Suricata IDS operation.
- IDS alert validation.
- Network port scan investigation.
- Network anomaly investigation.
- Evidence collection and reporting.
- Security investigation methodology.

---

**What I am learning:** Network security | Traffic analysis | Intrusion detection | SOC investigations
