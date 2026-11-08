# Wireshark Notes

My notes from working through captures in Wireshark. Mostly HTTP so far.

## Display filters

Filters only hide packets in the view. They don't change the capture file, so you can try things without worrying about losing anything.

The ones I've used most:

| Filter                     | What it shows               |
| -------------------------- | --------------------------- |
| `http`                     | HTTP traffic                |
| `tcp`                      | TCP traffic                 |
| `dns`                      | DNS traffic                 |
| `ip.addr == 192.168.0.110` | Anything to or from that IP |
| `dns.qry.type == 1`        | DNS A record lookups        |

## The three panes

- **Packet list** (top): one line per packet with the number, time, source, destination, protocol, length and a short summary.
- **Packet details** (middle): the packet broken down by layer (Ethernet, IPv4, TCP, HTTP and so on). You can expand each one to see its fields.
- **Packet bytes** (bottom): the raw data in hex and ASCII.

## HTTP

A request looks like:

```
GET /download.html HTTP/1.1
```

Request headers worth looking at: Host, User-Agent, Accept, Accept-Language, Accept-Encoding, Referer, Connection.

A response looks like:

```
HTTP/1.1 200 OK
```

Response headers worth looking at: Server, Content-Type, Content-Length, Last-Modified, Connection.

### Follow HTTP Stream

Right-click a packet and go to Follow > HTTP Stream. Wireshark puts the whole conversation back together, so you can read the request, the response and any HTML that was sent without jumping between packets. It's much quicker than going packet by packet.

## Things to keep in mind

- Plain HTTP isn't encrypted, so headers and page content show up in full in the capture. With HTTPS, TLS encrypts all of that.
- The User-Agent is whatever the client says it is, and it's easy to fake.
- The same goes for the Server header. Don't treat it as proof of what the server is actually running.

## What I've picked up so far

- Filtering early saves a lot of scrolling.
- You'll see the TCP handshake before any HTTP traffic, which helps when you're trying to find where a conversation starts.
- You can match each request to its response, which makes it easier to follow what happened.
- Keep track of what you actually saw in the capture versus what you're assuming.
