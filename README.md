# Network Traffic & IDS Analysis

I'm using this repo to practise network traffic analysis and intrusion detection. The plan is to work through packet captures in Wireshark, run them through Suricata, and then check whether the alerts actually line up with what's in the packets.

## What I want to get out of it

- Get comfortable reading traffic in Wireshark
- Understand TCP, UDP, DNS, HTTP and TLS better at the packet level
- Learn how Suricata alerts work and how to check whether they're real or false positives
- Practise writing up an investigation properly (timeline, findings, what I couldn't confirm)

## Tools

- Wireshark
- Suricata
- Ubuntu
- Python, for small scripts if I need them

## Layout

```text
network-traffic-ids-analysis/
├── docs/          notes and references
├── evidence/
│   ├── alerts/       Suricata output
│   └── screenshots/  Wireshark screenshots
├── pcaps/         capture files
├── reports/       write-ups for each investigation
└── README.md
```

## How I'm approaching each investigation

Roughly:

1. Work out what the scenario is and which hosts are involved
2. Go through the traffic in Wireshark and note anything odd
3. Run the pcap through Suricata and look at the alerts
4. Match alerts to the actual packets
5. Put together a timeline
6. Write it up, including what I'm unsure about

## Notes on safety

All the traffic here comes from public sample pcaps or my own isolated lab. I'm not capturing traffic on networks I don't own, scanning anything I don't have permission to scan, or running any malware I come across. In the reports I try to keep what I actually saw separate from what I'm guessing.

## Status

Just getting started, still setting up the lab.

## http.cap

- **Source:** [Wireshark Sample Captures](https://wiki.wireshark.org/SampleCaptures)
- **Download:** https://wiki.wireshark.org/uploads/27707187aeb30df68e70c8fb9d614981/http.cap
- **Format:** PCAP 2.4
- **Link type:** Ethernet
- **Size:** 25,803 bytes
- **Purpose:** Analyse HTTP requests and responses in a sample network capture.
- **Verification:** Confirmed as a valid PCAP using the `file` command.
