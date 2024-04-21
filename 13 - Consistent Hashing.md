Here are the detailed and verbose notes on Consistent Hashing:

## Introduction to Consistent Hashing

Consistent hashing is a technique used to distribute data or requests across multiple servers in a way that minimizes the impact of server additions or removals on the system. It is particularly useful in load balancing, content delivery networks (CDNs), and distributed databases.

## Basic Hashing

Before diving into consistent hashing, let's review basic hashing. In basic hashing, we use a hash function to map a user's IP address to a server. The hash function takes the IP address as input and outputs a server index. For example, if we have three servers, we can use the modulo operator to map the IP address to a server: `server_index = IP_address % 3`.

## Limitations of Basic Hashing

Basic hashing has a major limitation. When a server is added or removed, the hash function changes, and all users are remapped to a new server. This can lead to inefficient use of caches and increased latency.

## Consistent Hashing

Consistent hashing addresses the limitations of basic hashing by using a circular space to map user requests to servers. Each server is assigned a position on the circle, and user requests are mapped to a point on the circle using a hash function. The user is then routed to the first server encountered moving clockwise from the mapped point.

## How Consistent Hashing Works

1. Hash Function: A hash function is used to map a user's IP address to a point on the circle.
2. Server Placement: Servers are placed at specific points on the circle, ensuring that each server is responsible for a roughly equal portion of the circle.
3. Request Routing: When a user request is received, the hash function maps the IP address to a point on the circle. The user is then routed to the first server encountered moving clockwise from the mapped point.

## Benefits of Consistent Hashing

1. Minimizes Impact of Server Additions/Removals: When a server is added or removed, only a small portion of users are affected, and the system can continue to operate efficiently.
2. Improves Cache Efficiency: By consistently mapping users to the same server, caches can be effectively utilized, reducing latency and improving performance.

## Use Cases for Consistent Hashing

1. Load Balancing: Consistent hashing can be used to distribute incoming requests across multiple servers, ensuring that each server receives a roughly equal amount of traffic.
2. Content Delivery Networks (CDNs): Consistent hashing can be used to route requests to different CDN servers, ensuring that users are consistently routed to the same server.
3. Distributed Databases: Consistent hashing can be used to distribute data across multiple database nodes, ensuring that each node is responsible for a roughly equal portion of the data.

## Rendezvous Hashing

Rendezvous hashing is a similar technique to consistent hashing. It tries to accomplish the same goal of consistently mapping requests to the same node, but with a slightly different approach.

## Key Components of Consistent Hashing

1. Hash Key: The input to the hash function, which can be a user's IP address or any other unique identifier.
2. Hash Function: A function that maps the hash key to a point on the circle. This function should be deterministic and consistent.
3. Nodes: The servers or nodes that are responsible for handling user requests.

In summary, consistent hashing is a powerful technique for distributing data or requests across multiple servers in a way that minimizes the impact of server additions or removals on the system. It is particularly useful in load balancing, CDNs, and distributed databases.
