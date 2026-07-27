# Bear Robot API

## Introduction
Welcome to the Bear's **Base** repository. This repository contains:
- Protocol Buffers (`.proto` files) defining the API specifications.
- Documentation for integrating and using the Robot API service.

The Bear Robot API Service is a gRPC-based API for third-party clients to:
- **Send commands** to control the robot base.
- **Receive status updates** from robot base, such as odometry and battery status.
- **Retrieve robot information**.

For a comprehensive overview of the API capabilities, refer to the [Robot API public docs](https://bearrobotics-public.github.io/base/). While currently using gRPC, future plans include adding corresponding REST APIs.

---

## Overview
The Bear Robot API facilitates communication between third-party clients and robots via the gRPC framework. It employs:
- **Unary RPC**: For request/response-type communications.
- **Server-streaming RPC**: For continuous updates from robots.

### Key Features of Server-streaming RPCs
1. **Metadata for Responses**:
   - Each response contains:
     - A **timestamp**: Indicates when the response was generated (local clock-based, not a global clock).
     - A **sequence number**: Incremental for ordering responses and detecting duplicates.
   - Note: Sequence numbers may reset to `0` under rare circumstances, such as service restarts.

2. **Handling Streaming Queries**:
   - Queries have a **60-minute timeout**. If needed beyond this duration, reissue the query.
   - **Message delivery guarantees**: "Best effort," meaning messages may be dropped due to:
     - Packet loss.
     - Network congestion.
     - Processing errors.
   - **Duplicate messages**: Clients must handle duplicates appropriately, considering both sequence numbers and timestamps for strict detection.

3. **gRPC Status Codes**:
   - A successful call returns an **OK** status with the expected response.
   - Errors result in a gRPC status code and an error message for debugging.

---

## Getting Started
### Prerequisites
- **A robot base**: For details and how to get one, [visit our website](https://www.bearrobotics.ai/base).
- **gRPC Framework**: Ensure your development environment supports gRPC.
- **Protocol Buffers Compiler**: Install `protoc` for compiling `.proto` files into your desired language.

### Cloning the Repository
```bash
git clone https://github.com/bearrobotics-public/base.git
cd base
```

### Compile & Develop
The [Robot API Reference](https://bearrobotics-public.github.io/base/v1.3/resources/Mission/) has documentation for available API.

Client code generation instructions and usage examples for Python can be found [HERE](https://bearrobotics-public.github.io/base/guides/setup/examples_python/)

## License

This project is licensed under the Mozilla Public License version 2.0 (MPLv2). See the [LICENSE](./LICENSE) file for details.

