# Networking Basics

### IP Addresses

- Every machine communicating over the internet needs a unique public IP address to identify it
- An IP address is a 32-bit integer, typically written as 4 octets in decimal form separated by periods (e.g. 192.168.0.1)
- With 32 bits, IPv4 can support around 4 billion unique IP addresses
- IPv6 uses 128-bit addresses to overcome IPv4 limitations, providing many more possible unique addresses

### Sending Data Over a Network

- Data is sent in packets which contain headers with metadata like source/destination IP addresses
- This metadata is analogous to the addressing information on a physical mail envelope
- The internet protocol (IP) defines the rules for sending data packets between computers

### Transmission Control Protocol (TCP)

- TCP provides additional capabilities beyond IP for sending large amounts of data reliably
- Data is further split into TCP packets which have sequence numbers in their headers
- Sequence numbers allow the recipient to reassemble the original data stream correctly
- IP handles device addressing, while TCP ensures reliable data transport and reassembly

### Application Layer Protocols

- Application protocols like HTTP operate above TCP/IP in the protocol stack
- Application data gets encapsulated in TCP packets, which get encapsulated in IP packets
- As developers, we typically work with just the application layer data

### Public vs Private IP Addresses

- Public IPs are used for internet-accessible servers that can receive incoming connections
- Private IPs are used for devices on local area networks (LANs) behind routers with public IPs
- Port forwarding is required to allow external access to services on private IP addresses

### Static vs Dynamic IP Addresses

- Static IPs don't change, commonly used for internet servers to maintain accessibility
- Dynamic IPs can change over time, which dynamic DNS helps handle for servers
- Most residential ISPs assign dynamic private IPs to consumer devices

### Ports

- Ports provide multiple communication channels per IP address for different services
- Port numbers are 16-bit values, ranging from 0 to 65535
- Well-known ports are conventionally assigned to common services (e.g. 80 for HTTP, 443 for HTTPS)
- Clients use ephemeral high-number ports for their end of TCP connections

### Local Host

- The 127.0.0.1 IP address is the localhost which refers to the local machine itself
- It is commonly used for accessing locally running server software during development

### Key Takeaways

- IP addresses uniquely identify machines on the internet
- TCP/IP provides connection, transport, and reassembly of data streams
- Application protocols like HTTP operate on top of TCP/IP
- Ports enable multiple services on a single IP address
- Understanding networking basics is crucial for system design interviews
