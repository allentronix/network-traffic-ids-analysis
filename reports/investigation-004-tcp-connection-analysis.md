# Investigation 004 — TCP Connection Analysis

## Objective

I used Wireshark to follow how a TCP connection starts, focusing on the three-way handshake, TCP flags, sequence numbers, acknowledgments, and the application data that followed.

## Tools

- Wireshark
- Sample PCAP: `pcaps/http.cap`

## Evidence

- Screenshot: `evidence/screenshots/08-tcp-syn-filter.png`
- Screenshot: `evidence/screenshots/09-tcp-handshake.png`

## 1. TCP Three-Way Handshake

I looked at the first three packets to see how the client and server establish a TCP connection before sending application data.

### Observed Connection

| Field       | Value             |
| ----------- | ----------------- |
| Client IP   | `145.254.160.237` |
| Client Port | `3372`            |
| Server IP   | `65.208.228.223`  |
| Server Port | `80`              |
| Protocol    | TCP               |

### Handshake Packets

| Packet | Direction       | Flags    | Sequence | Acknowledgment |
| ------ | --------------- | -------- | -------- | -------------- |
| 1      | Client → Server | SYN      | 0        | —              |
| 2      | Server → Client | SYN, ACK | 0        | 1              |
| 3      | Client → Server | ACK      | 1        | 1              |

### What I observed

1. The client started the connection with a SYN.
2. The server replied with SYN-ACK, acknowledging the client's SYN and sending its own initial sequence number.
3. The client sent an ACK to acknowledge the server's SYN.
4. At that point, the TCP connection was established.

I also noted that SYN and FIN each use one sequence number, even when the packet carries no application data.

## 2. TCP Connection and HTTP Correlation

After the handshake, I saw the client send:

`GET /download.html HTTP/1.1`

This let me connect the TCP handshake to the HTTP exchange: the application data followed after the connection was established.

## 3. TCP Payload Analysis

I used Wireshark to inspect TCP segments carrying application data. One server segment showed:

- Frame length: `1434` bytes
- TCP payload length: `1380` bytes

The frame length includes protocol headers, while the TCP payload length counts the data carried by TCP. They describe different parts of the packet.

## 4. Security Relevance

From this capture, I can see how checking the handshake may help when investigating network traffic. For example, it can help me:

- Identify connection attempts.
- Distinguish established connections from unsuccessful attempts.
- Investigate port scanning.
- Identify connection resets.
- Analyze retransmissions and network reliability.
- Correlate network connections with application-layer activity.

## Conclusion

In this capture, I observed a successful TCP three-way handshake followed by HTTP communication. Looking at the packets in Wireshark helped me follow the connection setup, acknowledgments, and transfer of application data.
