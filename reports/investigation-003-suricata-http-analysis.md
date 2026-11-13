# Investigation 003 — Suricata HTTP Traffic Analysis

## Objective

Analyze a previously captured HTTP PCAP using Suricata IDS, identify extracted protocol metadata, and correlate findings with manual Wireshark analysis.

## Tools

- Suricata 8.0.8
- Wireshark
- jq
- macOS

## Evidence

- Input PCAP: `pcaps/http.cap`
- EVE JSON: `evidence/alerts/investigation-003/eve.json`
- Fast log: `evidence/alerts/investigation-003/fast.log`
- Statistics: `evidence/alerts/investigation-003/stats.log`
- Suricata log: `evidence/alerts/investigation-003/suricata.log`

## Findings

### HTTP Transaction

Suricata identified the following HTTP transaction:

- Source IP: `145.254.160.237`
- Source port: `3372`
- Destination IP: `65.208.228.223`
- Destination port: `80`
- HTTP method: `GET`
- Host: `www.ethereal.com`
- URI: `/download.html`
- HTTP version: `HTTP/1.1`
- Response status: `200`
- Content type: `text/html`
- Response content length: `18070` bytes

### Wireshark Correlation

The HTTP transaction metadata extracted by Suricata matched the corresponding request and response observed during Investigation 001 in Wireshark.

### Detection Results

Suricata processed 43 packets and generated protocol metadata events, including HTTP, DNS, flow, and file information events.

No signature alerts were recorded in the EVE JSON output or fast.log for this capture.

## Conclusion

Suricata successfully analyzed the offline PCAP and extracted HTTP transaction metadata. Correlation with Wireshark confirmed the identified endpoints, HTTP request, and response.

The absence of signature alerts indicates that no loaded detection signature matched this traffic. It does not independently establish that the traffic is safe.

## Lessons Learned

- Suricata can extract application-layer metadata automatically.
- EVE JSON provides structured data suitable for investigation and automation.
- Wireshark and Suricata complement each other in network investigations.
- Absence of alerts is not proof of benign activity.
