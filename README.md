# RTMP Stream Handler

A TypeScript implementation of an RTMP (Real-Time Messaging Protocol) server for handling live video streaming. This server supports RTMP handshake, chunk streaming, and basic publish/subscribe functionality for live video broadcasting.

## Overview

This is a from-scratch RTMP server implementation in TypeScript that handles:
- RTMP handshake protocol (C0, C1, C2, S0, S1, S2)
- RTMP chunking protocol
- AMF0 command message encoding/decoding
- Stream publishing and playback
- Multiple concurrent connections
- Video and audio message routing

## Features

- **Full RTMP Protocol Support**: Complete implementation of RTMP handshake and chunking
- **Publish/Subscribe Model**: Supports multiple publishers and subscribers
- **Stream Management**: Automatic stream key handling and client routing
- **Audio/Video Routing**: Efficient routing of audio and video packets between publisher and subscribers
- **Metadata Handling**: Support for AAC and AVC sequence headers
- **TypeScript**: Fully typed implementation for better code quality

## Prerequisites

- Node.js 14 or later
- TypeScript 4.0 or later

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/stukenov/rtmp-stream-handler.git
   cd rtmp-stream-handler
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Build the TypeScript code:
   ```bash
   npm run build
   ```

## Usage

### Starting the Server

Run the compiled server:

```bash
npm start
```

The server will start listening on port 1935 (default RTMP port).

### Publishing a Stream

Use FFmpeg or OBS to publish a stream:

```bash
# Using FFmpeg
ffmpeg -re -i input.mp4 -c:v libx264 -c:a aac -f flv rtmp://localhost/live/streamkey

# Using OBS
# Server: rtmp://localhost/live
# Stream Key: streamkey
```

### Playing a Stream

Use FFplay or VLC to play the stream:

```bash
# Using FFplay
ffplay rtmp://localhost/live/streamkey

# Using VLC
# File -> Open Network Stream -> rtmp://localhost/live/streamkey
```

## Architecture

### RTMP Message Types

The server handles the following RTMP message types:
- Set Chunk Size (Type 1)
- Abort (Type 2)
- Acknowledgement (Type 3)
- User Control (Type 4)
- Window Acknowledgement Size (Type 5)
- Set Peer Bandwidth (Type 6)
- Audio Message (Type 8)
- Video Message (Type 9)
- AMF0 Command (Type 20)
- AMF3 Command (Type 17)

### Stream Flow

1. **Handshake**: Client and server exchange C0/S0, C1/S1, C2/S2 packets
2. **Connect**: Client sends connect command with application name
3. **Publish/Play**: Client either publishes or subscribes to a stream
4. **Data Transfer**: Audio/video data is chunked and routed to subscribers
5. **Disconnect**: Clean stream cleanup on connection close

## Configuration

The server can be configured by modifying the following in `index.ts`:

- Port: Default is 1935
- Chunk size: Default is 4096 bytes
- Stream management settings

## Development

### Building

```bash
npm run build
```

### Running in Development Mode

```bash
npm run dev
```

## Protocol Details

### RTMP Handshake

1. C0/S0: Version byte (1 byte)
2. C1/S1: Timestamp + Zero + Random data (1536 bytes)
3. C2/S2: Echo of received packet

### Chunking Protocol

- **Format 0**: Complete chunk header (11 bytes)
- **Format 1**: No message stream ID (7 bytes)
- **Format 2**: Only timestamp delta (3 bytes)
- **Format 3**: No header, continue previous chunk

## Limitations

- Basic implementation without advanced features
- No authentication/authorization
- No recording functionality
- No transcoding support
- Single-server deployment (no clustering)

## Performance

- Supports multiple concurrent streams
- Efficient buffer management
- Non-blocking I/O using Node.js streams

## Use Cases

- Live streaming platform backend
- Video conferencing infrastructure
- Broadcast automation systems
- Real-time video delivery
- Educational streaming platforms

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues.

## License

MIT License - see LICENSE file for details.

Copyright (c) 2025 Saken Tukenov

## References

- [RTMP Specification](https://rtmp.veriskope.com/docs/spec/)
- [Adobe RTMP Protocol](https://www.adobe.com/devnet/rtmp.html)
