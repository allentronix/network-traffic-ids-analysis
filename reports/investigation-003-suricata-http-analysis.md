# Investigation 003 — Suricata HTTP Traffic Analysis

## Objective

I used Suricata to analyze a captured HTTP PCAP and compared its results with what I saw in Wireshark during Investigation 001.

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

### HTTP transaction

Suricata found this HTTP request and response in the capture:

- Source IP: `145.254.160.237`
- Source port: `3372`
- Destination IP: `65.208.228.223`
- Destination port: `80`
- Method: `GET`
- Host: `www.ethereal.com`
- URI: `/download.html`
- HTTP version: `HTTP/1.1`
- Response status: `200`
- Content type: `text/html`
- Response content length: `18070` bytes

### Comparing with Wireshark

I compared Suricata's HTTP metadata with the request and response I examined in Wireshark for Investigation 001. The endpoints and HTTP details matched.

### Detection results

Suricata processed 43 packets and recorded metadata events for protocols and activity such as HTTP, DNS, flows, and files. It did not record any signature alerts in the EVE JSON output or `fast.log` for this capture.

## Conclusion

I was able to use Suricata to extract the HTTP transaction details from the offline capture, then confirm them against Wireshark. Suricata did not report an alert. That only means none of the loaded signatures matched this traffic; it does not prove the traffic is safe.

## What I learned

- Suricata can pull useful application-layer details from a packet capture.
- EVE JSON makes those details easier to search and work with using tools such as `jq`.
- Comparing Suricata's output with Wireshark helped me check that I was interpreting the transaction correctly.
- No alert by itself is not proof that traffic is benign.
