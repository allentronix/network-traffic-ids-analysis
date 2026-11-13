# Investigation 004: TCP Connection and Retransmission Analysis

## Objective

I used Wireshark to follow a TCP conversation in `http.cap`. I looked at the connection handshake, sequence and acknowledgment numbers, packet sizes, and the retransmission and duplicate ACK events Wireshark flagged.

## Tools and environment

- Wireshark on macOS
- PCAP: `http.cap`
- Transport protocol: TCP
- Application protocol: HTTP

## Network overview

| Component | Value |
| --- | --- |
| Client IP | `145.254.160.237` |
| Client port | `3372` |
| Server IP | `65.208.228.223` |
| Server port | `80` |
| Transport protocol | TCP |
| Application protocol | HTTP |

## TCP three-way handshake

I used this display filter to find SYN packets:

```text
tcp.flags.syn == 1
```

The first three packets showed the connection setup:

| Step | Flags | Direction | Sequence | Acknowledgment |
| --- | --- | --- | ---: | ---: |
| 1 | SYN | Client → Server | 0 | — |
| 2 | SYN, ACK | Server → Client | 0 | 1 |
| 3 | ACK | Client → Server | 1 | 1 |

The client started with a SYN. The server replied with SYN-ACK, and the client sent an ACK back. After those three packets, the connection was ready for application data. The SYN and FIN flags each use one sequence number, even when no application payload is included.

This handshake is useful context when I inspect a connection. It can help me see whether a connection was established and investigate patterns such as repeated SYNs or failed connections. A handshake by itself does not show that a connection is malicious.

## Sequence numbers and acknowledgments

During the handshake, Wireshark showed:

```text
SYN       Seq=0
SYN, ACK  Seq=0 Ack=1
ACK       Seq=1 Ack=1
```

After the handshake, TCP segments carried HTTP data and acknowledgments. Sequence numbers track positions in the byte stream. An acknowledgment tells the sender which byte the receiver expects next. For example, `Ack=1591` means the receiver has received bytes up to 1590 and is expecting byte 1591.

## TCP payload and packet size

Wireshark showed TCP segments with a payload length of `1430` bytes. Some corresponding frames had a total length of `1484` bytes. The frame includes the link, IP, and TCP headers as well as the TCP payload.

For a typical Ethernet II frame with IPv4 and TCP headers and no options, those headers total 54 bytes:

| Component | Size |
| --- | ---: |
| Ethernet header | 14 bytes |
| IPv4 header | 20 bytes |
| TCP header | 20 bytes |
| Total headers | 54 bytes |

That accounts for the difference between the 1484-byte frame and the 1430-byte TCP payload in this example. It helped me distinguish the data carried by TCP from the size of the whole frame.

## Retransmission

I used this filter to look for retransmissions:

```text
tcp.analysis.retransmission
```

Wireshark marked packet 36 as a `TCP Spurious Retransmission`:

```text
80 → 3371 [PSH, ACK]
Seq=1
Ack=722
Len=1430
```

Packet 26 had matching sequence and acknowledgment numbers and the same payload length. Wireshark labels a packet a spurious retransmission when its analysis suggests the data may already have reached the receiver. Retransmissions can be related to packet loss, delayed or lost acknowledgments, congestion, or timer behavior, so this label alone does not identify the cause. A retransmission also does not, by itself, mean the traffic is malicious.

## Duplicate acknowledgment

I used this filter to find duplicate ACKs:

```text
tcp.analysis.duplicate_ack
```

Wireshark marked packet 37 as a `TCP Dup ACK`:

```text
3371 → 80 [ACK]
Seq=722
Ack=1591
Win=8760
Len=0
```

Packet 28 also had `Ack=1591`. A duplicate ACK repeats the same acknowledgment number, which indicates that the receiver is still waiting for the same next byte. This can happen when data is missing or arrives out of order. The duplicate ACK is a clue about TCP behavior, but it does not establish why it happened or indicate malicious activity on its own.

## Following the TCP stream

I used these filters to inspect the conversation and its analysis events:

```text
tcp.stream == 1
tcp.stream == 1 && tcp.analysis
```

The stream showed the HTTP GET request, TCP acknowledgments, the server response, data segments, the suspected spurious retransmission, and the duplicate ACK. Following the stream helped me connect the TCP events to the HTTP exchange.

## Evidence

| Screenshot | What it shows |
| --- | --- |
| `08-tcp-syn-filter.png` | TCP SYN packet identification |
| `09-tcp-handshake.png` | TCP three-way handshake |
| `10-tcp-retransmission-analysis.png` | Retransmission and duplicate ACK analysis |

## Conclusion

I followed the TCP connection from its handshake through the HTTP exchange and looked at the retransmission and duplicate ACK Wireshark identified. This gave me practice reading sequence numbers and packet sizes and showed how Wireshark's TCP analysis can help explain what is happening in a captured stream. The retransmission and duplicate ACK are observations from this capture; on their own, they do not tell me what caused them.

**Investigation status:** Complete
