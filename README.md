# JBOD Client-Server System

A robust client-server implementation for interacting with JBOD (Just a Bunch of Disks) storage systems. This library provides a clean and efficient interface for connecting to a JBOD server, sending operations, and processing responses.

## Overview

This project implements a client-side library for interacting with a JBOD storage server. The client connects to the server, sends operation commands, and processes the responses. It's designed for reliability, efficiency, and ease of use.

## Features

- **Reliable connection handling**: Establish and maintain connections to JBOD servers
- **Efficient packet processing**: Properly encode and decode packets for communication
- **Operation support**: Send various operations to the JBOD server including read and write operations
- **Error handling**: Robust error detection and reporting
- **Cross-platform compatibility**: Works across different systems with proper byte-order handling

## API Reference

### Connection Management

```c
bool jbod_connect(const char *ip, uint16_t port);
```
Establishes a connection to a JBOD server at the specified IP address and port.
- **Parameters**:
  - `ip`: The IP address of the server
  - `port`: The port number of the server
- **Returns**: `true` if connection successful, `false` otherwise

```c
void jbod_disconnect(void);
```
Disconnects from the JBOD server.

### Operation Handling

```c
int jbod_client_operation(uint32_t op, uint8_t *block);
```
Sends a JBOD operation to the server and processes the response.
- **Parameters**:
  - `op`: Operation code defining the action to perform
  - `block`: Data block (for write operations) or buffer for received data (for read operations)
- **Returns**: Return code from the server indicating success or failure

### Utility Functions

```c
bool nread(int fd, int len, uint8_t *buf);
```
Reads exactly `len` bytes from file descriptor `fd` into buffer `buf`.
- **Returns**: `true` on success, `false` on failure

```c
bool nwrite(int fd, int len, uint8_t *buf);
```
Writes exactly `len` bytes from buffer `buf` to file descriptor `fd`.
- **Returns**: `true` on success, `false` on failure

```c
bool send_packet(int sd, uint32_t op, uint8_t *block);
```
Creates and sends a packet to socket descriptor `sd`.
- **Returns**: `true` on success, `false` on failure

```c
bool recv_packet(int fd, uint32_t *op, uint16_t *ret, uint8_t *block);
```
Receives a packet from file descriptor `fd` and unpacks it.
- **Returns**: `true` on success, `false` on failure

## Packet Format

The client-server communication uses a structured packet format:

- **Header** (6 bytes):
  - Length field (2 bytes): Total packet length (header + data)
  - Operation code field (4 bytes): The JBOD operation to perform

- **Data Block** (variable):
  - For write operations: 256 bytes of data to write
  - For read operations: No data in the request, but 256 bytes in the response

## Building and Usage

### Prerequisites

- C compiler (gcc recommended)
- Network libraries

### Building

```bash
make
```

### Example Usage

```c
#include "jbod_client.h"

int main() {
    // Connect to server
    if (!jbod_connect("127.0.0.1", 8888)) {
        fprintf(stderr, "Failed to connect to server\n");
        return 1;
    }
    
    // Perform a read operation
    uint8_t buffer[256];
    uint32_t read_op = /* construct read operation code */;
    int ret = jbod_client_operation(read_op, buffer);
    
    if (ret == 0) {
        // Process read data in buffer
    } else {
        // Handle error
    }
    
    // Disconnect from server
    jbod_disconnect();
    
    return 0;
}
```

## Error Handling

The library functions return boolean values or error codes to indicate success or failure. It's recommended to check these return values and handle errors appropriately in your application.

## Implementation Details

- **Byte Order**: Network byte order (big-endian) is used for all multi-byte fields in packets
- **Socket Handling**: Standard socket APIs are used for network communication
- **Buffer Management**: Functions properly handle partial reads/writes

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Suggested Repository Names

- `jbod-client`
- `jbod-net-client`
- `jbod-networking`
- `jbod-comm-library`
- `jbod-client-server`
