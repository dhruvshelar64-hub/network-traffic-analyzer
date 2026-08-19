# Network Traffic Analysis Report

## 1. Project Overview

This project demonstrates the capture and analysis of normal network traffic using TShark and Wireshark on Kali Linux.

The purpose of the project was to inspect common network protocols, understand communication patterns, and document the results using an evidence-based security report.

## 2. Objectives

- Capture network traffic from an authorized personal lab environment.
- Identify protocols present in the capture.
- Analyze ICMP, DNS, TCP, and TLS traffic.
- Use command-line tools for packet analysis.
- Document observations, evidence, and limitations.

## 3. Environment

- Operating system: Kali Linux
- Network interface: `eth0`
- Local IP address: `192.168.153.129`
- Tools: TShark, Wireshark, tcpdump
- Capture duration: 120 seconds
- Capture filter: `not port 22`
- Test environment: Authorized personal lab

## 4. Methodology

The following workflow was used:

1. Identified the active network interface using `ip -br addr`.
2. Verified available capture interfaces using `tshark -D`.
3. Captured traffic from `eth0` using TShark.
4. Generated controlled traffic using ping, DNS lookup, and HTTPS requests.
5. Saved the capture as a PCAPNG file.
6. Reviewed the protocol hierarchy using TShark.
7. Opened the capture in Wireshark for visual investigation.
8. Documented the observed protocols and findings.

## 5. Commands Used

### Interface identification

```bash
ip -br addr
```

### Interface verification

```bash
tshark -D
```

### Packet capture

```bash
sudo timeout 120 tshark \
  -i eth0 \
  -f "not port 22" \
  -w /tmp/lab_capture2.pcapng
```

### Test traffic

```bash
ping -c 4 1.1.1.1
curl -I https://example.com
nslookup example.com
curl -L https://www.wireshark.org/ -o /dev/null
```

### Protocol hierarchy analysis

```bash
tshark -r lab_capture2.pcapng -q -z io,phs
```

## 6. Capture Results

The capture contained:

| Protocol | Frames | Bytes |
|---|---:|---:|
| ICMP | 8 | 784 |
| DNS | 12 | 1,184 |
| TCP | 73 | 238,235 |
| TLS | 31 | 234,729 |
| Total | 93 | 240,203 |

## 7. Findings

### Finding 1: ICMP Traffic

Eight ICMP frames were observed during connectivity testing with `1.1.1.1`.

The terminal ping test reported four packets transmitted, four packets received, and zero percent packet loss.

**Interpretation:** The ICMP traffic indicates successful network connectivity between the Kali Linux system and the destination.

### Finding 2: DNS Traffic

Twelve DNS frames were observed over UDP.

DNS resolution was used to resolve `example.com` before HTTPS communication took place.

**Interpretation:** The DNS traffic represents normal domain-name resolution activity.

### Finding 3: TCP Traffic

Seventy-three TCP frames were observed in the capture.

TCP was used to establish and maintain connections for HTTPS communication.

**Interpretation:** The TCP traffic represents the transport layer activity associated with web requests and encrypted communication.

### Finding 4: TLS Traffic

Thirty-one TLS frames were observed.

TLS traffic was generated while accessing HTTPS websites. The application data was encrypted and was not decrypted during this analysis.

**Interpretation:** The presence of TLS indicates that encrypted web communication was being used.

### Finding 5: Protocol Distribution

TCP produced the highest number of frames. TLS accounted for most of the captured bytes because an HTTPS webpage was downloaded during the capture.

**Interpretation:** The capture was primarily composed of encrypted web traffic rather than ICMP or DNS activity.

## 8. Wireshark Filters Used

The following display filters were used for investigation:

```text
icmp
```

```text
dns
```

```text
tls.handshake
```

```text
tcp
```

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

## 9. Security Analysis

The capture represented normal authorized lab traffic. No malicious activity was intentionally generated or identified.

The analysis demonstrates how network defenders can:

- Identify protocols in a packet capture.
- Review DNS activity.
- Investigate TCP connections.
- Recognize encrypted TLS traffic.
- Use packet evidence to support technical findings.

A protocol appearing in a capture is not automatically an indication of malicious activity. Further investigation, context, and baseline comparison would be required before classifying traffic as suspicious.

## 10. Limitations

- The capture represented only a short period of network activity.
- The traffic was generated from a single Kali Linux system.
- TLS payloads were encrypted and were not decrypted.
- No malware or attack traffic was included.
- External domains were used only for controlled connectivity testing.
- The results may differ in another network environment.

## 11. Conclusion

The project successfully demonstrated a repeatable network-traffic analysis workflow using Kali Linux, TShark, Wireshark, and tcpdump.

The capture contained ICMP, DNS, TCP, and TLS traffic. TShark was used for command-line capture and protocol analysis, while Wireshark was used for visual inspection and filtering.

This project improved practical skills in packet capture, protocol identification, network troubleshooting, traffic investigation, and security documentation.

## 12. Ethical Notice

This analysis was performed in an authorized personal lab environment.

Network traffic must not be captured or analyzed without permission from the system or network owner.
