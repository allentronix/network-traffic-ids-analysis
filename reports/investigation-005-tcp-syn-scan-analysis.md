# Investigation 005 — TCP SYN Scan Analysis

## 1. Overview

| Field          | Details                     |
| -------------- | --------------------------- |
| Investigation  | 005 — TCP SYN Scan Analysis |
| Capture        | `03-tcp-syn-scan.pcapng`    |
| Capture format | PCAPNG                      |
| Packets        | 22                          |
| Scanner        | `10.10.10.1`                |
| Target         | `10.10.10.10`               |
| Tools          | Wireshark, TShark           |
| Analysis type  | Offline packet analysis     |

## 2. What I wanted to find out

I wanted to see how a TCP SYN scan looks in a packet capture, work out which ports responded as open or closed, and check what the missing responses to other probes tell me.

## 3. Methodology

I opened the capture in Wireshark and used display filters to focus on the relevant packets. I also used TShark to print the packet list:

```bash
tshark -r 03-tcp-syn-scan.pcapng
```

Relevant Wireshark display filters:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.flags.syn == 1 && tcp.flags.ack == 1
tcp.flags.reset == 1
icmp
tcp.port == 3389
```

The SYN-only filter shows the initial probes, while SYN-ACK packets show ports that replied as open. I checked which way the RST packets travelled so I could tell resets from the scanner apart from resets sent by the target.

## 4. TCP SYN Scan Fundamentals

A TCP SYN scan is sometimes called a **half-open scan**. For an open port, the scanner starts the TCP handshake but does not finish it.

Typical packet sequences:

### Open port

```text
Scanner -> Target: SYN
Target  -> Scanner: SYN, ACK
Scanner -> Target: RST
```

A SYN-ACK means the target accepted the connection attempt on that port. The scanner then sends a reset instead of completing the handshake.

### Closed port

```text
Scanner -> Target: SYN
Target  -> Scanner: RST, ACK
```

A reset response indicates that the TCP port is closed.

### Unanswered probe

```text
Scanner -> Target: SYN
(no response observed)
```

No reply by itself does not tell me the port's state. The traffic could have been filtered, lost, or missed because the host did not respond or the capture was incomplete.

## 5. Packet Evidence and Findings

### 5.1 Host discovery / address resolution

Packets 2–3 show the scanner looking up the target's hardware address with ARP:

```text
Who has 10.10.10.10? Tell 10.10.10.1
10.10.10.10 is at d2:32:9f:df:ff:37
```

Packet 1 is an ARP request for `10.10.10.50`. I can't tell from this capture how it relates to the scan of `10.10.10.10`.

### 5.2 Open ports

The capture shows the target replying with SYN-ACK to probes for the following ports:

| Packet(s) | Target port | Common service          | Evidence                    |
| --------- | ----------: | ----------------------- | --------------------------- |
| 4–6       |         139 | NetBIOS Session Service | SYN → SYN-ACK → scanner RST |
| 7–9       |          80 | HTTP                    | SYN → SYN-ACK → scanner RST |
| 10–12     |         445 | SMB                     | SYN → SYN-ACK → scanner RST |
| 14–16     |          21 | FTP                     | SYN → SYN-ACK → scanner RST |
| 17–19     |          22 | SSH                     | SYN → SYN-ACK → scanner RST |

I counted these five ports as **open** because each replied with a SYN-ACK. The service names are the usual associations for these port numbers; the capture does not show which software or version is actually running.

### 5.3 Closed port

Packets 20–21 show the probe and response for TCP/9999:

```text
10.10.10.1:45555  -> 10.10.10.10:9999  [SYN]
10.10.10.10:9999  -> 10.10.10.1:45555 [RST, ACK]
```

The target's RST-ACK shows that TCP port **9999 is closed**.

### 5.4 Unanswered RDP probes

Packets 13 and 22 show two SYN probes to TCP/3389:

```text
10.10.10.1:45555 -> 10.10.10.10:3389 [SYN]
10.10.10.1:45557 -> 10.10.10.10:3389 [SYN]
```

I don't see a SYN-ACK, RST, or ICMP response to either probe. The second probe was sent about 1.10 seconds after the first.

**Finding:** There was no response to the TCP/3389 probes in this capture, so I can't say whether the port is open or closed. Filtering could explain the silence, but packet loss, the host's behavior, or limits in the capture could also be responsible.

### 5.5 Scanner-generated resets

For the open-port probes, RST packets 6, 9, 12, 16, and 19 come from `10.10.10.1`, the scanner. Each follows a SYN-ACK from the target, which fits the scanner ending the half-open connection attempt.

These resets come from the scanner, so they are not evidence that those ports are closed.

## 6. Port-State Summary

| TCP port | Conventional service | Observed state | Evidence                             |
| -------: | -------------------- | -------------- | ------------------------------------ |
|       21 | FTP                  | Open           | SYN-ACK                              |
|       22 | SSH                  | Open           | SYN-ACK                              |
|       80 | HTTP                 | Open           | SYN-ACK                              |
|      139 | NetBIOS              | Open           | SYN-ACK                              |
|      445 | SMB                  | Open           | SYN-ACK                              |
|     3389 | RDP                  | Unconfirmed    | Two SYN probes; no response captured |
|     9999 | Unknown/custom       | Closed         | Target RST-ACK                       |

## 7. Limitations

- This is a small, pre-recorded capture containing 22 packets, not a live scan performed by the analyst.
- No response to a probe does not by itself prove that a port is filtered.
- The capture does not establish the exact reason TCP/3389 did not respond.
- Conventional port numbers suggest services but do not prove service identity, implementation, or version.
- Packet 1's ARP request for `10.10.10.50` is not explained by the available evidence.

## 8. Conclusion

In this capture, SYN-ACK responses showed five open TCP ports: 21, 22, 80, 139, and 445. The target replied to the TCP/9999 probe with a RST-ACK, so I marked that port closed. Two probes to TCP/3389 had no reply, so I could not determine its state.

I also learned to check who sent each RST. The scanner's resets followed SYN-ACKs and ended those attempts; the target's RST-ACK for port 9999 indicated a closed port. This was a useful reminder to base conclusions on what the packets actually show.

## 9. Evidence

- Capture: `pcaps/investigation-005/03-tcp-syn-scan.pcapng`
- Source archive: `pcaps/investigation-005/session-03-captures.zip`
- Original scan capture: `pcaps/investigation-005/nmap_standard_scan`
- Wireshark screenshots: I still need to save relevant screenshots under `evidence/screenshots/` and link them here.
