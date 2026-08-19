# Network Traffic Analyzer

A Kali Linux network-traffic analysis project using TShark, Wireshark, and tcpdump.

## Objective

Capture, inspect, and document normal network traffic across multiple protocols.

## Tools Used

- Kali Linux
- TShark
- Wireshark
- tcpdump

## Workflow

```text
Capture → Inspect → Filter → Analyze → Document
```

## Capture Details

- Interface: `eth0`
- Capture filter: `not port 22`
- Environment: Authorized personal lab
- Capture duration: 120 seconds

## Results

The lab capture contained:

- 93 frames
- 240,203 bytes
- ICMP traffic
- DNS traffic
- TCP traffic
- TLS traffic

## Example Commands

```bash
sudo timeout 120 tshark \
  -i eth0 \
  -f "not port 22" \
  -w /tmp/lab_capture.pcapng
```

```bash
tshark -r lab_capture.pcapng -q -z io,phs
```

Example display filters:

```text
icmp
dns
tls.handshake
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

## Evidence

### TShark Capture



### Protocol Analysis



### DNS Investigation

![DNS analysis]

## Report

[Read the complete network traffic analysis report]

## Skills Demonstrated

- Packet capture
- Protocol analysis
- DNS investigation
- TCP/IP traffic inspection
- TLS traffic analysis
- Linux command-line usage
- Security documentation

## Ethical Notice

This project was performed in an authorized personal lab environment. Do not capture traffic from networks or devices without permission.
