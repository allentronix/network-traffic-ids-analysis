# PCAPs

The capture files I'm working with, plus where each one came from.

## http.cap

The HTTP sample from the [Wireshark sample captures page](https://wiki.wireshark.org/SampleCaptures). It's small (25,803 bytes), a standard pcap (version 2.4) over Ethernet. I checked the format with `file` on macOS after downloading it.

I'm using it for Investigation 001 (HTTP traffic analysis): looking at the requests and responses, the headers, the TCP side of things and the HTML that comes back.

## Note

These are all public samples or captures from my own lab. Anything I write about them sticks to what's actually in the packets.
