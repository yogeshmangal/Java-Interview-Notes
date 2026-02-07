
# HTTP, HTTP/2, HTTP/3 – Short Notes

## HTTP
**HTTP (HyperText Transfer Protocol)** is a request–response protocol used for communication between a client (browser) and a server.

---

## HTTP Versions

### HTTP/1.0
- One request per connection
- Connection closed after each response
- Slow due to repeated connections

### HTTP/1.1
- Persistent connections
- Multiple requests but sequential
- Text-based headers
- Suffers from head-of-line blocking

### HTTP/2
- Runs over TCP
- Multiple parallel streams over a single connection
- Binary framing
- Header compression
- Still affected by TCP packet loss

### HTTP/3
- Runs over QUIC (UDP-based)
- Faster connection setup
- No head-of-line blocking
- Better performance on unstable/mobile networks
- TLS 1.3 built-in

---

## Transport Protocols

### TCP
- Reliable and ordered delivery
- Retransmission of lost packets
- Congestion control
- Slower due to strict guarantees

### UDP
- User Datagram Protocol
- Fast but unreliable
- No delivery or order guarantee
- Packet loss possible

---

## QUIC
**QUIC (Quick UDP Internet Connections)**
- Built on top of UDP
- Provides reliability, retransmission, and congestion control
- Supports multiple independent streams
- Avoids TCP head-of-line blocking
- Used by HTTP/3

---

## HTTP/2 vs HTTP/3

| Feature | HTTP/2 | HTTP/3 |
|-------|-------|-------|
| Transport | TCP | QUIC (UDP) |
| Packet Loss Impact | All streams blocked | Only affected stream blocked |
| Handshake | Slower | Faster |
| Mobile Performance | Average | Excellent |

---

## One-Line Summary
**HTTP/3 uses QUIC over UDP to achieve TCP-like reliability without head-of-line blocking, making web communication faster and more resilient.**
