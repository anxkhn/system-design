# HTTP

## Client-Server Model

- The client-server model is a fundamental concept in system design, where a client initiates a request to a server, and the server responds with the requested data.
- The client can be an end-user, a browser, or even another server.
- The server is responsible for serving requests and providing the requested data.

## Remote Procedure Call (RPC)

- RPC is a concept that allows multiple machines to communicate with each other remotely.
- It enables a client to execute code on a server, which is not locally available.
- RPC is a loosely defined term and is essentially executing code over a network.
- All application layer protocols, including HTTP, are built on top of RPC.

## HTTP (Hypertext Transfer Protocol)

- HTTP is an application layer protocol that is built on top of IP and TCP.
- It is a request-response protocol, where the client initiates a request, and the server responds with the requested data.
- HTTP is stateless, meaning that each request and response is independent of the previous one.

**HTTP Request**

- An HTTP request consists of two main components: headers and body.
- The headers provide information about the request, such as the URL, method, and request headers.
- The body contains the actual data being sent in the request.

**HTTP Methods**

- HTTP methods define the action to be performed on the server.
- The most common HTTP methods are:
  - GET: Retrieve data from the server.
  - POST: Create new data on the server.
  - PUT: Update existing data on the server.
  - DELETE: Delete data from the server.

**HTTP Status Codes**

- HTTP status codes are used to indicate the outcome of an HTTP request.
- The most common status codes are:

  - 200: OK (request was successful).
  - 201: Created (resource was created successfully).
  - 400: Bad Request (request was invalid).
  - 401: Unauthorized (request was unauthorized).
  - 404: Not Found (resource was not found).
  - 500: Internal Server Error (server encountered an error).

  Overview of some common HTTP status code ranges along with their meanings:

1. **1xx Informational Response**:

   - These status codes indicate that the request has been received and understood and that processing is continuing.
   - Example: 100 Continue, 101 Switching Protocols.

2. **2xx Success**:

   - These status codes indicate that the request was successfully received, understood, and accepted.
   - Example: 200 OK, 201 Created, 204 No Content.

3. **3xx Redirection**:

   - These status codes indicate that further action needs to be taken to complete the request.
   - Example: 301 Moved Permanently, 302 Found, 304 Not Modified.

4. **4xx Client Error**:

   - These status codes indicate that the client seems to have made an error in the request.
   - Example: 400 Bad Request, 401 Unauthorized, 404 Not Found.

5. **5xx Server Error**:
   - These status codes indicate that the server failed to fulfill a valid request.
   - Example: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable.

## SSL/TLS (Secure Sockets Layer/Transport Layer Security)

- SSL/TLS is a security protocol that provides encryption for HTTP requests and responses.
- It prevents man-in-the-middle attacks, where an attacker intercepts and reads the data being sent between the client and server.
- SSL/TLS encrypts the data being sent, making it unreadable to unauthorized parties.

## HTTPS (Hypertext Transfer Protocol Secure)

- HTTPS is an extension of HTTP that uses SSL/TLS to provide encryption and security.
- It is the standard protocol used for secure communication over the internet.
- HTTPS ensures that the data being sent between the client and server is encrypted and secure.
