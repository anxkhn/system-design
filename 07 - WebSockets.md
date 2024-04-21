# Web Sockets

## Limitations of HTTP and Introduction to Web Sockets

- HTTP has limitations when it comes to real-time communication and bi-directional data transfer.
- Web Sockets is a protocol that can fill in the gaps of HTTP and provide a more efficient way of handling real-time data streaming and bi-directional communication.

## Other Application Layer Protocols

- FTP (File Transfer Protocol): used for transferring large files between computers, runs on port 21 by default.
- SMTP (Simple Mail Transfer Protocol): used for sending emails, runs on port 25 by default.
- SSH (Secure Shell): used for remotely connecting to another machine and running a CLI on that machine.
- WebRTC (Web Real-Time Communication): used for video and audio streaming, built on UDP (User Datagram Protocol) for optimized performance.

## Trade-offs of Web Sockets

- Web Sockets are built on TCP (Transmission Control Protocol), which provides reliability and guarantees delivery of data.
- However, WebRTC uses UDP, which is optimized for video and audio streaming and provides faster transmission speeds, but may sacrifice reliability.

## Problem Statement: Building a Chat Application

- Building a chat application requires real-time communication and bi-directional data transfer.
- With HTTP, we can load the webpage and get a list of chat messages, but we need to implement a way to update the chat in real-time.
- One way to do this is by polling, where the client sends an HTTP request at regular intervals to get the latest chat messages.
- However, polling is less optimized and can be expensive in terms of network resources.

## Web Sockets Solution

- Web Sockets provide a better way to solve the problem of real-time communication and bi-directional data transfer.
- The client initiates a Web Socket connection with the server, which responds with a status code of 101, indicating that the connection is being upgraded to a Web Socket connection.
- After the handshake, a persistent connection is established between the client and server, allowing for bi-directional communication.
- The client can send data to the server, and the server can push data to the client in real-time, without the need for the client to poll the server.

## Key Features of Web Sockets

- Bi-directional communication: data can be sent in both directions, from client to server and from server to client.
- Real-time communication: data is transmitted in real-time, without the need for polling.
- Persistent connection: the connection is established once and remains open, allowing for efficient communication.
- Full-duplex connection: data can be sent in both directions simultaneously.

## Twitch Chat Example

- Twitch chat is a great example of a real-time chat application that uses Web Sockets.
- When a user types a message, it is sent to the server, which then pushes the message to all connected clients in real-time.
- The client does not need to poll the server for new messages; instead, the server pushes the messages to the client as they are received.

## Benefits of Web Sockets

- Less expensive in terms of network resources, as a single connection is established and maintained.
- Provides real-time communication and bi-directional data transfer.
- Suitable for applications that require live updates, such as live feeds, chat applications, and gaming platforms.

## Downside of Web Sockets

- A state is established between the client and server, which can be a problem if the connection breaks.
- However, this is a minor downside compared to the benefits of using Web Sockets.

## tl;dr

- Web Sockets provide a more efficient and optimized way of handling real-time communication and bi-directional data transfer.
- They are suitable for applications that require live updates and are a better alternative to polling.
- Understanding the trade-offs and benefits of Web Sockets is essential for designing scalable and efficient systems.
