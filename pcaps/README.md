# PCAPs

The capture files I'm working with, plus where each one came from.

## http.cap

The HTTP sample from the [Wireshark sample captures page](https://wiki.wireshark.org/SampleCaptures). It's small (25,803 bytes), a standard pcap (version 2.4) over Ethernet. I checked the format with `file` on macOS after downloading it.

I'm using it for Investigation 001 (HTTP traffic analysis): looking at the requests and responses, the headers, the TCP side of things and the HTML that comes back.

## Note

These are all public samples or captures from my own lab. Anything I write about them sticks to what's actually in the packets.

## DNS Investigation Capture

### `dns.cap`

- **Source:** [Wireshark DNS Sample Capture](https://wiki.wireshark.org/SampleCaptures)
- **Format:** PCAP 2.4
- **Link type:** Ethernet
- **Size:** 4.2 KB
- **Purpose:** DNS query and response analysis
- **Topics:** A, AAAA, MX, TXT, PTR, CNAME records, successful resolution, NXDOMAIN responses

Used for Investigation 002: DNS Traffic Analysis.
