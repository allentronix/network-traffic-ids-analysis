# Investigation 001 — HTTP Traffic Analysis

## 1. Investigation Overview

- **Investigation ID:** 001
- **Title:** HTTP Traffic Analysis
- **Category:** Network Traffic Analysis
- **Tool:** Wireshark
- **Capture File:** `http.cap`
- **Capture Source:** [Wireshark Sample Captures](https://wiki.wireshark.org/SampleCaptures)
- **Objective:** Analyse a normal HTTP client-server communication and understand TCP connections, HTTP requests, response headers, and transmitted content.

## 2. Scope

This investigation examines a publicly available packet capture containing HTTP communication.

The analysis focuses on:

- Identifying client and server IP addresses.
- Understanding TCP connection establishment.
- Inspecting HTTP requests and responses.
- Examining HTTP headers and response content.
- Understanding the visibility implications of unencrypted HTTP.

## 3. Tools and Methodology

- Wireshark
- Display filter: `http`
- Packet inspection
- HTTP header analysis
- Packet-to-response correlation

## 4. Observations

### 4.1 Network Communication

| Attribute            | Observation       |
| -------------------- | ----------------- |
| Client IP            | `145.254.160.237` |
| Server IP            | `65.208.228.223`  |
| Client TCP Port      | `3372`            |
| Server TCP Port      | `80`              |
| Transport Protocol   | TCP               |
| Application Protocol | HTTP              |

### 4.2 TCP Connection Establishment

The capture shows a TCP three-way handshake:

1. Client sends SYN.
2. Server responds with SYN-ACK.
3. Client sends ACK.

This establishes the TCP connection before HTTP communication begins.

### 4.3 HTTP Request

The client sends:

`GET /download.html HTTP/1.1`

Request headers observed:

- Host: `www.ethereal.com`
- User-Agent: Mozilla/5.0 (Windows NT 5.1; ...)
- Accept: Multiple content types
- Accept-Language: en-us
- Accept-Encoding: gzip, deflate
- Connection: keep-alive
- Referer: `http://www.ethereal.com/development.html`

The User-Agent identifies itself as a Windows XP-era Gecko-based browser. This is a client-provided value and is not independently verified.

### 4.4 HTTP Response

The server returns:

`HTTP/1.1 200 OK`

Response headers observed:

- Server: Apache
- Content-Type: `text/html; charset=ISO-8859-1`
- Content-Length: `18070`
- Connection: Keep-Alive
- Last-Modified: 20 Apr 2004

The response contains HTML content that can be inspected in Wireshark's decoded UTF-8 text view.

## 5. Security Observations

- HTTP traffic is transmitted without TLS encryption.
- Request paths and HTTP headers are readable in the packet capture.
- The response HTML is visible in the capture.
- Client User-Agent values can provide clues about software but can be spoofed.
- Server headers identify claimed server software but are not definitive proof of the actual implementation.

## 6. Conclusion

This investigation demonstrated how to analyse normal HTTP communication using Wireshark.

The analysis identified the TCP connection, HTTP request, corresponding server response, request and response headers, and readable HTML content.

The capture provides a baseline for understanding normal network traffic before investigating suspicious communications and IDS alerts.

## 7. Limitations

- The analysis is based on a provided sample PCAP, not live traffic from a production environment.
- The capture represents a historical HTTP exchange.
- Observations are limited to information available in the captured packets.
- Identified software versions and client attributes are not independently verified.

## 8. Evidence

Screenshots will be stored in:

`evidence/screenshots/`

Planned evidence:

- HTTP packet list filtered using `http`.
- HTTP request headers.
- HTTP response headers.
- Reconstructed HTTP stream.
