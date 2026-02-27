---
title: UDP Multicast Video Streaming System
description: A Python-based UDP multicast sender/receiver system for real-time video streaming with jitter buffer, file reconstruction, and live playback support.
author: hamdanalhajeri
date: 2024-11-28 16:00:00 +0000
categories: [Projects, Networking]
tags: [python, networking, udp, multicast, video-streaming, real-time]
pin: false
---

# UDP Multicast Video Streaming System

A robust Python-based UDP multicast streaming system that enables real-time video transmission over networks. This project implements chunked file streaming with a sophisticated receiver featuring jitter buffering, file reconstruction, and optional live playback capabilities.

## Quick Start 🚀

### Prerequisites
- Python 3.9+ (tested on Windows)
- ffmpeg/ffplay for live playback
- Standard Python library (no extra packages required)

### Installing ffmpeg on Windows

Using Chocolatey:
```bash
choco install ffmpeg
```

Or download a static build and add `bin` directory to your system PATH.

### Basic Usage

**1. Start the Receiver** (in one PowerShell window):

```bash
python .\src\reciver.py --pipe-stdout --log-level WARNING
```

For live playback with ffplay:
```bash
python reciver.py --pipe-stdout --log-level WARNING | ffplay -i - -fflags nobuffer -flags low_delay
```

**2. Start the Sender** (in another PowerShell window):

```bash
python sender.py --file video.ts --content-type video --chunk-size 1316 --rate 50 --meta-interval 5
```

## Project Overview

This networking project implements a complete UDP multicast streaming solution designed for real-time video transmission. The system handles packet transmission, reception, buffering, and reconstruction with built-in support for live video playback.

## Key Features

### Sender (`sender.py`)

- **Chunked File Streaming**: Efficient file segmentation and transmission
- **Configurable Transmission Rate**: Control packets per second
- **Content Type Support**: Handles text, binary, audio, and video
- **Loop Mode**: Option to repeat transmission
- **Metadata Broadcasting**: Periodic session information for late-joining receivers
- **Interface Selection**: Bind to specific network interfaces

### Receiver (`reciver.py`)

- **Jitter Buffer**: Smooths out network delay variations
- **File Reconstruction**: Automatically rebuilds transmitted files
- **Live Stdout Piping**: Real-time streaming to media players
- **Multi-Session Support**: Handles concurrent streams
- **Idle Timeout**: Auto-exit after inactivity period
- **Output Management**: Organized file saving to output directory

## Architecture

### Multicast Communication

- **Default Group**: 239.1.1.1
- **Default Port**: 5004
- **Protocol**: UDP for low-latency transmission

### Packet Structure

The system uses a custom packet format optimized for streaming:
- Session identification
- Sequence numbering
- Content type metadata
- Payload chunks
- Reconstruction markers

## Typical Workflow (Windows/PowerShell)

### Step 1: Prepare Video File

For optimal streaming, convert MP4 to MPEG-TS format:

```bash
ffmpeg -i "C:\path\to\video.mp4" -c copy -f mpegts "C:\path\to\video.ts"
```

### Step 2: Start Receiver

With auto-exit after 5 seconds of inactivity:
```bash
python .\src\reciver.py --pipe-stdout --log-level WARNING --idle-timeout 5
```

For specific network interface:
```bash
python .\src\reciver.py --pipe-stdout --iface 192.168.1.100
```

### Step 3: Start Sender

Standard video transmission:
```bash
python sender.py --file video.ts --content-type video --chunk-size 1316 --rate 50 --meta-interval 5
```

With looping:
```bash
python sender.py --file video.ts --content-type video --chunk-size 1316 --rate 50 --loop
```

## Command-Line Options

### Sender Options

| Option | Description | Default |
|--------|-------------|---------|
| `--file` | File to transmit | Required |
| `--content-type` | Type: text, binary, audio, video | Required |
| `--group` | Multicast group address | 239.1.1.1 |
| `--port` | Multicast port | 5004 |
| `--rate` | Packets per second | 50 |
| `--chunk-size` | Bytes per payload | 1316 |
| `--meta-interval` | Metadata resend interval (seconds) | 5 |
| `--iface` | Outbound interface IP | Auto |
| `--loop` | Repeat transmission | False |

### Receiver Options

| Option | Description | Default |
|--------|-------------|---------|
| `--group` | Multicast group address | 239.1.1.1 |
| `--port` | Multicast port | 5004 |
| `--pipe-stdout` | Stream to stdout | False |
| `--output-dir` | Directory for reconstructed files | output/ |
| `--idle-timeout` | Auto-stop after N seconds | 0 (never) |
| `--iface` | Interface to bind | Auto |

## Technical Details

### Chunk Size Optimization

**1316 bytes** is recommended for video streaming:
- Aligns with MPEG-TS packet size (7 × 188 bytes)
- Minimizes fragmentation
- Reduces corruption in transport streams
- Optimal for UDP payload size

### Rate Control

Adjust `--rate` based on:
- Network capacity
- Video bitrate
- Packet loss tolerance
- Target latency

### Jitter Buffer

The receiver implements a jitter buffer to:
- Smooth out network delay variations
- Reorder out-of-sequence packets
- Reduce playback stuttering
- Maintain consistent frame delivery

### Late-Joining Support

The `--meta-interval` parameter enables receivers to:
- Join mid-broadcast
- Discover active sessions
- Start reconstruction immediately
- Synchronize with ongoing streams

## Technologies Used

- **Python**: Core implementation language
- **UDP Protocol**: Low-latency datagram transmission
- **Multicast**: Efficient one-to-many distribution
- **ffmpeg/ffplay**: Media processing and playback
- **Socket Programming**: Network communication
- **Threading**: Concurrent packet handling

## Configuration Files

### Network Setup Scripts

- `pc1_sender.sh`: Sender device configuration
- `pc2_receiver.sh`: Receiver device configuration
- `pc3_receiver.sh`: Additional receiver setup
- `router1.cfg`: Router 1 configuration
- `router2.cfg`: Router 2 configuration
- `switch.cfg`: Switch configuration

These scripts enable easy deployment across multiple devices and network infrastructure.

## Troubleshooting

### Common Issues and Solutions

**ffplay not found:**
- Install ffmpeg and restart PowerShell
- Verify with: `where ffplay`

**Corrupted video/Invalid NAL unit:**
- Use MPEG-TS input (`.ts` files)
- Set chunk size to 1316 for TS alignment
- Lower transmission rate if packet loss suspected

**No packets received:**
- Verify sender/receiver use same group/port
- Check firewall rules for UDP traffic
- Ensure interface selection is correct
- Verify multicast routing on network

**Playback stuttering:**
- Increase jitter buffer size
- Reduce transmission rate
- Check network congestion
- Use wired connection instead of WiFi

## Use Cases

- **Live Video Streaming**: Real-time broadcast to multiple receivers
- **Network Testing**: Multicast infrastructure validation
- **Educational Tool**: Learning UDP and multicast concepts
- **Video Distribution**: Efficient one-to-many content delivery
- **Research Platform**: Network protocol experimentation

## Performance Characteristics

### Latency
- Low-latency transmission via UDP
- Minimal buffering for real-time playback
- Sub-second end-to-end delay possible

### Scalability
- Multicast enables efficient bandwidth usage
- Single sender supports unlimited receivers
- Network infrastructure is the bottleneck

### Reliability
- Jitter buffer handles network variations
- Sequence numbering detects packet loss
- Graceful degradation under poor conditions

## Future Enhancements

Current TODO items:

- **Mid-Broadcast Joining**: Full support for late-joining receivers (partially implemented)
- **FEC (Forward Error Correction)**: Improve reliability
- **Adaptive Bitrate**: Dynamic quality adjustment
- **Encryption**: Secure transmission
- **Congestion Control**: Network-aware rate adjustment
- **Statistics Dashboard**: Real-time performance metrics

## Project Structure

```
NetworksProj/
├── src/
│   ├── sender.py              # Multicast sender
│   └── reciver.py             # Multicast receiver
├── pc1_sender.sh              # Sender setup script
├── pc2_receiver.sh            # Receiver setup script
├── pc3_receiver.sh            # Additional receiver
├── router1.cfg                # Router configuration
├── router2.cfg                # Router configuration
├── switch.cfg                 # Switch configuration
├── multicast_setup_notes.md   # Network setup guide
└── README.md                  # Documentation
```

## Example Commands

**Standard streaming workflow:**
```bash
# Receiver
python reciver.py --pipe-stdout --log-level WARNING | ffplay -i - -fflags nobuffer -flags low_delay

# Sender
python sender.py --file video2.ts --content-type video --chunk-size 1300 --rate 100
```

**Testing with reduced logging:**
```bash
python reciver.py --pipe-stdout --log-level ERROR
```

**High-rate transmission:**
```bash
python sender.py --file video.ts --content-type video --chunk-size 1316 --rate 100 --loop
```

## Links

You can explore the full project on [GitHub](https://github.com/HamdanAlhajeri/NetworksProj).

---

This project demonstrates practical implementation of UDP multicast streaming, jitter buffering, and real-time media transmission—essential concepts in network programming and video streaming systems.
