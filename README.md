My markdown notes for System Design by Neetcode.

[00 - Computer Architecture](00%20-%20Computer%20Architecture.md)

[01 - Application Architecture](01%20-%20Application%20Architecture.md)

[02 - Design Requirements](02%20-%20Design%20Requirements.md)

[03 - Networking Basics](03%20-%20Networking%20Basics.md)

[04 - TCP and UDP](04%20-%20TCP%20and%20UDP.md)

[05 - DNS](05%20-%20DNS.md)

[06 - HTTP](06%20-%20HTTP.md)

[07 - WebSockets](07%20-%20WebSockets.md)

[08 - API Paradigms](08%20-%20API%20Paradigms.md)

[09 - API Design](09%20-%20API%20Design.md)

[10 - Caching](10%20-%20Caching.md)

[11 - CDNs](11%20-%20CDNs.md)

[12 - Proxies and Load Balancing](12%20-%20Proxies%20and%20Load%20Balancing.md)

[13 - Consistent Hashing](13%20-%20Consistent%20Hashing.md)

[14 - SQL](14%20-%20SQL.md)

[15 - NoSQL](15%20-%20NoSQL.md)

[16 - Replication and Sharding](16%20-%20Replication%20and%20Sharding.md)

[17 - CAP Theorem](17%20-%20CAP%20Theorem.md)

[18 - Object Storage](18%20-%20Object%20Storage.md)

[19 - Message Queues](19%20-%20Message%20Queues.md)

<details>
  <summary>RAW Notes</summary>
  
## 0 - Computer Architecture
Disk = storage = hard drive(HDD hard disk drive, nowadays SSD is more common)

Memory = RAM(random access memory)

Reading and writing(done by CPU) from RAM is measured in terms of microseconds(1/10^6) (latency) and it's a lot faster than reading and
writing from disk which is measured in terms of milliseconds(1/10^3). So RAM is a lot faster than disk.

What if we want to communicate between RAM and disk? These components can't directly talk to each other, that's why we have to introduce
another component which is CPU(brain of computer).

The main things that CPU is capable of:

- reading and writing info to RAM or disk
- executing our code

While reading and writing from RAM are quick(microseconds) what if we wanna go even faster?

CPUs have another component called the cache which is a lot smaller than RAM(RAM itself is a lot smaller than disk). It's typically measured
as megabytes and is used to speed up read and write operations from RAM. It's measured in nanoseconds, so it's a lot faster.
Cache is mainly used as a substitution for the RAM.

What CPU will do is it'll take a subset of RAM, one portion or multiple portions that can fit within our cache and we want to choose
these portions intelligently. It will choose portions of the RAM that we're reading from or writing to, very frequently. Because we have
a limited amount of space in cache.

It's not necessary to have a cache but it will make things a lot faster.

If the CPU wants to read some data from RAM, it can first check if that data is in cache, if not, it will perform the more expensive operation
which is reading from RAM. But maybe it will throw it in cache so next time we have to read it, we can directly go to cache for it.

RAM and cache don't have persistent data.

The reason we have a cache, is to lower the latency which can increase the overall throughput of system. So our CPU can handle more
operations per time.

## 1 - Application Architecture

An application architecture for a production app.

The problem we introduce when we have multiple servers handling reqs is that when a user makes a req, how do we know which server
that req should go to? We use load balancer. It'll forward req to the server that has the minimum amount of traffic.

We don't store **log statements** on our server. Because we don't need to because our server isn't really gonna be interacting with them,
instead, we as developers are going to interact with the logs, so we will have external service that is gonna store our logs for us.

What if one of our servers isn't running well, maybe it has a faulty CPU. We want to know about the resources that our server is using.
Is our server responding to all reqs? Or some reqs are failing? To get that insight, we would have a **metrics service**.

Some of the metrics might directly come from logs. So log-based metrics.

We could use logs to create metrics.

Metrics are usually displayed in terms of time series charts. Logs-based metrics can also be used because logs are naturally time series data.
Everytime we log sth, it has some timestamp associated with it.

As devs, we can look at the metrics and get insight into how the app is running, but if sth goes wrong, you wouldn't want to have to **manually**
go and look at the metrics to realize what's going on. Or even worse, a user would tell us like emailing us that sth went wrong with our app.
We don't want this to happen. We(as devs) want to know immediately when sth goes wrong, so we want a push notification from the metrics.
To accomplish this, we have our metrics feed data into an alerting service which will for example tell us when a metric has reached
some threshold.

logging -> metrics -> alerts

These components are running on different computers. So there's some network component between them.

## 2 - Design Requirements

### design requirements

1. move data: Can be moving data from a component of machine to another component or from one data center to another data center.
   Moving data isn't so simple when machines are located at different data centers. We have to move data across networks
2. store data
3. transform data: For example, given a bunch of server response logs data, maybe we want to aggregate that data and transform it to find out
   what percentage of them was successful? and what percentage was failed?

These 3 operations encapsulate all of the functionalities of most apps.

How do we know if our app is designed well? Because there's not always a correct answer.

We have to think in terms of tradeoffs. How do we measure tradeoffs?

Availability: uptime / (uptime + downtime)

We measure availability in terms of 9s. Five 9s is a great availability. But for example 99% is not very good in a year it means 3.65 days it's
down.

Availability is used to define SLOs(service level objective). As devs we define some goals like availability, these are called SLOs.
SLA is different than SLO. SLO is sub portion of SLA.

As AWS, we would define SLA for our DB. It would be the SLO which is we want our DB to have 5 nines of availability. But as SLA, we say
if we don't reach the level of availability(determined by SLO), we will give you a partial refund. It's a AWS policy actually.

SLA is what customers can expect of our service and what's they pay for. It's a customer agreement. Service level agreement. It's not just a goal,
it's an agreement with a customer and says: this is what you can expect or these will be the consequences.

---

### Reliability

Systems can have reliability, full tolerance and redundancy. These contribute to each other. They have some differences.

If a user makes a req to our server and it responds, that means our server was available but doesn't necessarily mean our server is reliable.

Reliability is the probability that our system won't fail. We know if we just have a single server that's responding to users, we have a
higher probability of failing than if we have 2 servers. So by adding a server, we can increase the reliability of our server, but we're also
increasing the availability of system because for example if we have a single server, a lot of reqs could be sent to it or we can have
people maliciously trying to take our server down(DDOS attack - distributed denial of service attack).

**Q:** Vertical scaling increases availability because a higher percentage of users would get a response. What about reliability?

We still only have a single server, so reliability doesn't change. This is why horizontal scaling can have benefits compared to vertical scaling.

### fault tolerance

If one portion of our system has a fault, the system continues to operate successfully. One server goes down but the second server still works.
So fault tolerance is related to reliability.

### redundancy

We add a server that's running the exact same code that we don't need it in order to respond to users. Because the other server is capable
of doing everything. But it helps to have both because in the event of a fault, we have multiple copies, so our system continues to function because
of redundancy.

By having redundancies in our system, we're able to have fault tolerance and with fault tolerance, we can provide higher reliability.

Having the redundant server in same datacenter, we could have a disaster that goes down the both servers. So it would be better to have
the second server in a different datacenter or maybe other part of the world.

---

### Throughput

Means the amount of operations or data or sth we can handle over some period of time. In the context of communicating with a server, we would measure
the throughput of this, in terms of **reqs per second**(request/second). This means how many concurrent users, could our system
handle per second.

If we can horizontally or vertically scale, we can handle more reqs.

But when we only scale vertically, that server is a **single point of failure**. If it goes down, our entire system
goes down. So we need more reliability, fault tolerance and redundancy.

If we scale it horizontally, the downside is adding more complexity. Because we have to balance the reqs between servers. So we need a
load balancer which increases the complexity.

Horizontal is less limited.

When we talk about how many reqs a DB can handle(throughput of DB), we usually measure it in terms of queries per second(queries/second) or
QPS and it's similar to servers. We can even call it reqs/second ! Query is just more used when talking about DBs.

DBs have the same problems as servers. Where a single DB can be a single point of failure. Horizontal scaling vs vertical scaling.
In horizontal scaling of DB we need to keep data in multiple DBs in sync.

A third measurement of throughput is **amount of data per second** usually measured in bytes/second . This is a variation of reqs/second or
queries/second.

In servers, it makes more sense to measure it in terms of reqs/second because each req is mapped to a single user.

But in a data pipeline, where we given data and we wanna process data into some other format and it's not necessarily related to a single
user, it's better to measure throughput in terms of bytes/second. Because maybe we don't even have a req.

### Latency

It's just some period of time. In other words, amount of time it takes for an operation to complete. In req-res model,
the latency would be the amount of time it takes for that operation to complete(until we get a res back).

Latency is how long each **individual** req takes. But throughput is related to multiple reqs.

To lower the latency of a distributed system, we could introduce a second server and we could have these servers on different sides of world.
This increases the availability and reliability and throughput, but it also reduces the latency for users located on different sides of world.

Another technique for reducing latency is having CDN.

## 3 - Networking Basics

An IP address is a 32 bit integer and you have 12 digits in total: 012.345.678.910

Since it's 32 bit, we can't have all of the digits go up to 999. Because that wouldn't fit in a 32 bit integer. So the max value for each of these
is gonna be 256. So the biggest IP address we could contain: 256.256.256.256 (at least when you look at it as a single integer)

With 32 bits we can store up about to 4 billion unique public IP addresses. This is limiting. This limitation is related to IPV4.

IPV6 allows up to 128 bits.

Every machine is gonna be uniquely identifiable with an IP address.

What are the rules of communication between computers?

An analogy would be sending mail in real life. We need to specify From and To(metadata).

So the data itself(packets) has to include which IP address it's going to and which IP address this data came from?

A portion of a packet is gonna be reserved for that From and To and ... info which we call them metadata. That metadata is what describes
that entire packet. We call that metadata a IP(internet protocol) header. So a portion of packet is gonna be **IP header**.

The rules of sending data over internet is called the internet protocol(IP).

IP allows us to send data(packets) between machines but it doesn't do everything we need it to do.

What if instead of just sending one letter in the mail, we wanted to send an entire book in the mail? Let's assume we want to do so in an
envelope instead of a box.

One way to do is to rip every single page of the book and put it in an envelope and let's say it takes us 10 envelopes to contain the entire book
and we send those 10 envelops from same person to other person.

We can do that with internet protocol. But how does the other person reassembles the other pages(assume there is no page number)?

In IP there is no such thing for that. This is what another protocol called TCP solves that and other problems.

In TCP, in a packet, the portion that is not the IP header, is actually further broken up into 2 portions:

- TCP header: Some metadata about the TCP packet
- data

Recap till here: We're sending IP packets which have an IP header which allows us to communicate between computers. But when we're sending
large amounts of data we can't put all of that data within a single packet. So we have to split it up to multiple packets.

What kind of info would the TCP header includes that the IP header wouldn't?

One thing is sequence number of that packet. When the server receives all the packets, it's able to reassemble them in the correct order.

### Public vs private IP address

For a server to be publicly accessible, it needs to have a public IP address. But our client machine doesn't need to have a public address,
nobody's necessarily gonna be making reqs to it and actually the internet traffic that we use as clients, will go through some router.
So if we have multiple devices connected to a router, each of the devices doesn't need to have a public IP address, only our router does and
that's what happens. Router will in our local network(LAN), assign each of the machines connected to it, a private IP address which won't be
accessible on the internet.

### Static vs dynamic IP addresses

Static IP addresses are important on server. Because if clients are making a req to a machine, that IP address of server should be static.
But servers can also have dynamic IP addresses and things will continue working and that's because of **dynamic DNS**.

But for clients we don't need static IP address(in our local network, we don't have static IP address).

### Ports

It's a 16 bit value so 2^16 which is about 65000 ports for a machine.

Many application layer protocols have a default port. So when we use https to communicate with a server, we don't need to specify the port(443 in this
case). Because the default port for https is 443.

So from perspective of client, we send req to a specific port of server, but when server send res back using a port, that doesn't necessarily need
to be port 443, actually from client's perspective, it doesn't matter which port we receive that info on. So the protocol will randomly assign
a port and our browser will reserve that port to receive the response.

localhost is special name for IP address of 127.0.0.1 . This is sort of a reserved IP address which points at your local machine. So this is a
way of accessing a server that's running locally on your own machine.

## 4-4 - TCP and UDP

Internet protocol suite is commonly referred to as TCP/IP(TCP is running on top of internet protocol). But internet protocol suite also includes
other things that are not just TCP and IP, which includes UDP.

TCP is more common than UDP.

### TCP

When breaking data into pieces and sending it and then re-assembling it on other side, the order matters. This process is a property of
TCP and doesn't come with internet protocol. So TCP handles the ordering of packets.

Another benefit of TCP is it's reliable. The reason this is important is that networks in general are not reliable.

Internet protocol doesn't have a solution for when a packet doesn't arrive at destination. But TCP does. With TCP, those packets(and not
the successful ones) will be re-sent(re-transmission of lost packets).

You don't get reliability or re-transmission with UDP.

Part of the reason of why this is(re-transmission) possible, is because TCP establishes a connection between the two machines that are communicating.
It's a 2-way connection. So when we establish a TCP connection, we can send data in both directions. To establish this connection, we need
3-way handshake which is part of why TCP can be reliable, why we can send data in both directions and why we can reassemble data at the destination,
but this is also expensive. So TCP has a lot of overhead because of guarantees it give. TCP is reliable but slower compared to UDP.

### UDP(user datagram protocol)

**Datagram** is the word for packet that's used with UDP.

The benefit of UDP is we don't need to establish a connection between the client and server, so there's no handshake.
There's no guarantee every packet will arrive. Also the data might arrive out of order.

UDP is a lot faster than TCP.

UDP is great for live streaming, online games. UDP is also used in DNS.

## 5-5 - DNS (Domain name system)

DNS is a decentralized system

```shell
# to get IP address of google.com
nslookup google.com
```

Now paste the IP address in browser and you will go to google.com .

So we have a decentralized system that takes a domain name and translate it into an IP address.

ICANN is owner of all domains in world. Why I can't steal google.com ? Who owns google.com? Well it has to go through ICANN. ICANN
doesn't sell domains since it's non-profit.

The sellers of domains are called domain registrars like google domains, namechip, godaddy and ... . These resellers host servers which have
the DNS records. So eventually a user request should land to one of these servers which will do the translating for us.

DNS record stores info about resolving a DNS request and there are many types of DNS records.

A record(address record): specifies the domain points to which IP address.

Since the IP address usually aren't gonna change for servers(usually they're static IP addresses), though static IPs are not required(in this case,
dynamic DNS takes care of this), the client can cache these IP addresses. So we don't need to go through the DNS query every single time. We
already have the IP address cached.

A server is a computer with a public IP address and typically that IP address has some domain name associated with it.

.com is top level domain(TLD).

The domain owner controls the subdomain entirely.

## 6-6 - HTTP

Client means the one that's making the req(initiates req). Client could be another server but still it initiates the req.

### RPC

### HTTP

HTTP is a request-response protocol. Client makes a req and server sends back response. HTTP is built on TCP(so under the hood there's a TCP
connection).

GET is expected to be idempotent. We're expecting that resource to not change when reading it. It's cachable. Also it means if we pass in
the exact same params and query params, we'll get the exact same results. It should not change as a result of GET req. This is important for
caching. But POST is not idempotent, so not cachable(after the first time issuing a POST req, it will continue to
do some action on server side). DELETE usually is idempotent, so it's more cachable.

### SSL/TLS

SSL came before TLS. These, encrypt everything we send over HTTP.

HTTP is not necessarily secure. It can't mitigate man in middle attacks.

## 7-7 - WebSockets

HTTP default port is 80.

### Limitations of HTTP

Some of the application layer protocols:

- websocket
- FTP(file transfer protocol): by default runs on port 21
- SMTP(secure mail transfer protocol): port 25
- SSH
- webRTC

All of these protocols are built on TCP(so reliably is important for these protocols) but webRTC is built on UDP.

In chat, we can send HTTP req to get back the new data between the last time we got back the response and now. Like sending HTTP req every 1 minute.
This is called polling.

How do we time these intervals?

60seconds is a lot. Maybe we can pull every 1 second. This would work but it's not optimized. We have to create a new HTTP connection(built on
top of TCP) everytime we want the new messages(every 1 second).

A better way is to use websocket that is built to solve this problem.

First we do websocket handshake which is we send an HTTP req like GET and that req is to establish a websocket connection and after that the server will
respond with 101 status code which means it's taking this connection and upgrading it to a websocket connection(101 means switching protocols).
After this handshake, there's a **persistent** connection established between client and server. We don't have to keep checking(polling) the
server for new content, everytime there's a new message, we immediately receive that from server, the server pushes them.
It's a bi-directional(full duplex) communication.

HTTP can't quite do this until HTTP2 which introduces streaming which kinda make websockets obsolete.

The tweet feed can be implemented with polling but in twitch chat, websocket is better.

The downside of websocket is there's a state established between the client and server. Maybe the connection would break at some point and
then the server would continue to send data!

## 8-8 - API Paradigms

3 modern paradigms:

- REST
- GraphQL
- gRPC

### REST

**REpresentation State Transfer.** REST is not a protocol. Built on top of HTTP typically, but this doesn't mean REST is a protocol, it's
more a set of loose restrictions(standardizations).

REST apis are stateless. Because when we send reqs, we want to send everything that we need to know about that req and res.

An example of having state:

When going to next page, we need another 10 results. There are 2 ways we could implement that with our client-server architecture:

- server could have some info for every user, for example storing a session for every user on the server, so it will remember user saw
  the first 10 results, so now it should send 10 next results. What if we had multiple servers? Maybe the other server doesn't
  have the session info of a user. This is stateful. But we want REST apis to be stateless.
- for server to not have to know anything about ever user. The req should have all the info we need.

Note: In REST, the state is not stored on server but on client, there's definitely going to be some state in the browser. Like cookies,
session storage or ... .

When we make it stateless, so the servers don't need to store any state info, we allow ourselves to scale horizontally. We can add many servers
we want with a RESTful API. That's the big benefit of REST apis.

We don't add verbs to REST endpoints. Because the HTTP method like GET or ... is what we use as verb or what action are we taking on this resource.

The most common data format for REST APIs is JSON. It's readable but performance-wise, we can do better(gRPC).

### GraphQL

GraphQL is not a protocol, it's built on top of HTTP. But it only uses HTTP POST method, because we need to send data in body of req and there,
we include a query which tells which resource we want from server.

POST reqs are not idempotent in HTTP. So the downside is we can't cache graphql reqs as precisely as we can with REST APIs.

The problem it's trying to solve that we have in REST APIs:

- over-fetching: for example we just want the profile picture and username but when fetching the user, it will have a lot of info. This can be
  fixed in REST apis by adding custom logic on server to only send back the exact fields. With GraphQL we can specify exactly which fields of
  that resource, we want. So our app would be faster.
- under-fetching: Let's say for a page, we need to get the video and comments and the users that written those comments. So we need to make a
  bunch of consecutive reqs. It's easier to group all of these reqs in a single req.

With graphql we would only have a single endpoint but there are 2 operations that we can do with graphql endpoints:

- query(aka doing reading, we don't modify any data)
- mutation(creating, updating, deleting, that would all fall under mutation)

Unlike REST apis, graphql does have schema for endpoints. We have a schema that we have to follow. We can only choose what's available. But in
REST apis which uses JSON, we can add anything to the body of req.

### gRPC

Not a protocol.

Built on HTTP/2 . Because it needs certain features of http/2. GRPC can not be used natively from a browser. If you want to make reqs from a
browser using GRPC, you need a proxy. Like grpc-web. The reason we can't use it directly from a browser is grpc needs some fine grained control
over http/2 which browsers typically don't provide. So this is a downside. So grpc is used in server to server communication.

It's faster than REST APIs because instead of sending raw json(string), in grpc, it sends info in protocol buffers(schema objects) and
are serialized into binary. So we're sending a smaller amount of data.

GRPC also provides streaming, we can stream from client to server or from server to client or bidirectional streaming like websockets.
So grpc can do what websocket can do(http2 makes websockets obsolete).

The downside is a lot less tooling and less standardized.

Even though GRPC is built on top of HTTP and we know HTTP does have status codes, but in GRPC we don't have that, instead we have error messages.
So you have to have your own custom error handling based on error messages that you define server side.

In GRPC, every rpc or endpoint has an action or verb associate with it. So it's not just resource. Even though it's built on top of http(it has
http methods), we don't care about verbs and status codes.

## 9-9 - API Design

When you introduce non-backward compatible changes, you need to increase the version. So we don't break the apps of our clients using our APIs.

## 10-10 - Caching

With caching we lower latency and increase throughput(because it's faster to read and write from cache so we can read and write more data at
a faster rate).

`cache ratio = total number of hits / (total # of hits + total # of misses)`

Client caching and server caching

Getting data from DB is reading data from disk, so it's slow. We can add caching to server. We can take a subset of data on disk and put it
in memory(we can use redis). First we look at memory and if it was not there, get it from disk and also store the data in memory, so next time
somebody asks for the exact data, we look at memory first and it's there, that's a cache hit.

Even though our in-memory cache is much smaller than disk, since the vast majority of reqs want the data we stored in memory, this
speeds uo our server performance and increases the throughput. Because maybe reading from disk is 10000 reads / second but from memory
we can do a lot more. This can scale much higher.

There are different algorithms in caching that can accomplish different things:

- write-around caching: When there's a write req, it skips the cache entirely, we just write the data to primary storage(DB data that are stored in disk)
  and then when we wanna read(we have a read req), we'll check the cache, data is not there for the first time, so we expect to be a cache miss the
  first time we're trying to read data and then we find it on disk, throw it in cache and return it to user. Subsequent reads will read from cache.
  In this approach, there's gonna be a significant number of cache misses, but at the same time, we're not gonna put anything in the cache, unless
  it's actually being read.
- write-through caching: When we're creating some resource(write op), we immediately write it to cache **and** after that we write it to primary storage
  which would be on disk. We're caching everything
- write-back caching: much faster way of doing things when possible, but it can be less reliable and cause some inconsistencies. It's a lazy
  way of doing things. When we wanna create sth(create req), we would throw it in memory(cache) and we would skip the disk. Now the reads
  will immediately go to the memory. It ignores the existence of disk(like database which is our persistent storage). If the
  server crashes, all the data in memory would not be written to disk(persisted). To fix this, we periodically dump the data from memory to disk(copy
  it). This is OK if you're fine with some amount of data loss. But for example, we wouldn't do this in twitter. For example liking and
  disliking is not that important, so we can use this approach.

Note: We want to never read from cache(or reduce this as much as possible).

### Eviction policy

We need these policies because memory(cache) is limited in size.

- random
- FIFO: We're gonna remove the first one that was added and then add a new to end of it. But what if the one we're deleting is read a lot?
  Like a tweet from popular celebrity. So we need other policies.
- LRU(least recently used): When a cache entry is read, we move it to the end, so it becomes the most recently used. So we won't delete it for now.
  We want to remove the least recently used or read.
- LFU(least frequently used): we would have key-value pairs and the key maybe would be the tweet itself and then the value would be
  the number of times that tweet was read. We remove the one that has the lowest value. So the most popular tweets are gonna stay in cache. But
  we still allow new tweets to be added to cache and if they'll be read a lot, it won't be evicted.

In case of twitter we would prefer LRU. Because there could be really popular tweets but maybe it stopped. It was popular a tweet a year ago and
it reached high cache hits but after a while, it wasn't read a lot. With LFU, it would stay in our cache because of it's used count(value) since
it's so high. **So with LFU we don't care how recently it was read, we just care about the total count**.

In caching we wanna speed things up and we're willing to make some tradeoffs(like sacrificing consistency).

## 11-11 - CDNs (content delivery network)

CNDs are a way that we can cache data closer to end users, so we don't have to make reqs across the world.

We have origin server and CDN servers.

We can't put everything on CDN servers, CDN servers are dumb. We can only put static content that is not changing. We can't have application code
running on CDN servers(backend code). The content on CDN should be the same for every user.

There are edge servers and they allow you to run code and they're close to end users.

We can have for example the JS code on CDN because that JS code **is gonna be the same for every user**. So JS is hosted on a CDN and since it's
not changing, it doesn't need to hit the origin server every single time. We can also put img, video.

CDNs:

- lower the demand on origin server, because we're distributing data on CDN server
- decreases the latency because of shorter distance
- increases the reliability and availability of our entire system because what if one of the CDNs server goes down, it's OK we can go to the next
  closest CDN server

Types of CDNs:

- push CDN: For example in twitter profile image, as a user uploads a new image for profile, it's gonna be on our origin server initially but
  we're also gonna spread it out or push the img to every single CDN server immediately after it's been added to the origin server
- pull CDN: similar to traditional caching. When a user uploads a new profile img, it will be on the origin server and we're **not** gonna
  immediately push it to all the CDN servers. Now if a client wanna see that img, first it's gonna hit the closes CDN server to check if that
  profile img already cached at my nearest CDN server? For the first time, no. Now the CDN server is gonna act as a **proxy** because it will
  on behalf of the client, gonna say: This is a cache miss, I gotta find that img on origin server. Origin server is gonna return it to CDN
  and then CDN will cache the img and return it to user. Now any other user close to this CDN will also get that img from CDN as a cache hit. Because
  CDN has it. With this approach, maybe people on other side of world they don't care about that profile img, so we never had to push that img
  to all CDN servers around the world, because it isn't necessary. So CDNs will only have the copies of data for users around them that are
  actually **using** that data, we don't want unnecessary data.
  So a pull CDN would be better if users in certain regions of world have different interests and they're using different data of origin server.

For serving content on CDN, the `Cache-Control` header should have `public` value. But if it was `private`, it indicates that response(like a
JS file) should not be cached by a CDN server.

![](../img/11-11-1.png)

## 12-12 - Proxies and Load Balancing

Two main types of proxies:

- forward proxy: a middle server that takes our req and then forward it to the actual server, on our behalf. It hides the client from destination
  server. For example your country or computer is not allowed to access the destination server, but you are allowed to access the proxy server.
  So proxy server on behalf of you, does it for you, it goes to destination server and then respond to you. A forward proxy can also block things.
  For example on a corporate or school network, it's common to have all devices that are connected to that corporate network go through some proxy.
  For example forward proxy blocks youtube. So VPN is a forward proxy.
- reverse proxy: instead of hiding the client or user which is what happens with a forward proxy, a reverse proxy hides the destination server.
  The client doesn't know about the existence of destination server. It just knows about the reverse proxy and everything we do goes through
  reverse proxy. The destination though will probably know about the existence of the client, not necessarily but usually that's the case. An example
  is CDN. Load balancer is another example of reverse proxy.

Q: CDN is a forward proxy or reverse?

As users, we directly go to CDN but possibly if that CDN doesn't have data, it will make another req to the origin server that the client doesn't
know about. So CDN is reverse proxy.

When we horizontally scale the servers since we want to distribute traffic evenly among the servers(aka we wanna balance the load of user traffic).
So we use a load balancer.

### Load balancing strategies

With round-robin each of the servers gets even amount of load. A problem with this could be if one of the servers is less powerful than other ones.
In that case, we use a variation of round-robin called **weighted round-robin**. For example 50% of req goes to first server and 25% to second and
25% to third server.

Another strategy is load balance based on the number of least connections. We get a req in load balancer and we route it to the server that has the
least number of connections.

Another problem with round-robin is just because we're evenly distributing the reqs, maybe some reqs take longer to process(so the connection is
still alive). Round-robin wouldn't take this into account. In this case, we use load balancing based on least connections approach.

If servers are located on different locations we can load balance based on user location. Asian users to asian server.

Hashing load balancing: Take user req and use some field possibly the IP address or the actual content of what the user is requesting or maybe
userId and we create a hash of that so we can route them to the same server that possibly has cache for that user or ... .

There are a couple of types of load balancers: l4 and l7.

### Layer 4 vs layer 7 load balancing

l4(transport layer) is at a lower network layer, you can think of this as being TCP layer. L7 is application layer(like http).

l4 LB is typically faster because all we do is look at the IP address to determine how to balance. So we can use location-based balancing
or round-robin. But we can't be smart because in layer4 we don't have access to the application data, we can't decrypt that. So while this is faster,
it's less flexible.

With l7 LBs we can look at the application data. Maybe one of the servers is handling certain type of resources, for example one server is responsible
for tweets and another one for user profile and third one for auth. With l7 load balancer we can intelligently route user reqs to correct server
based on the **resource** user requested for. Layer7 LB is more expensive because we would have multiple connections(one between
client-LB and one with LB-server) and we're decrypting every req and then creating a new req whereas in layer4 we're taking a req
and then forwarding it to another server. we're just taking the IP address of destination of req which was LB IP address and we're gonna replace
that IP address with the server we route the req to and we just forward it there.

L4 is faster but less flexible and L7 is slower but more flexible.

If we have 1 LB, what happens it goes down?

Well this is a single point of failure. So we need multiple replicas of LB. There are 2 approaches here:

- all of them working concurrently
- we just have backup LBs and if the one working currently goes down, the req would be routed to the backup

Usually LBs can handle very large amount of traffic and handle high throughput, so we won't encounter the scenario where a LB gets
overloaded. Because they're not doing anything, they're just forwarding the req to other servers.

Maglev about load balancers

Nginx is LB and it can be used for other things. It's easier to use a LB provided by a cloud provider.

## 13-13 - Consistent Hashing

We can use hashing(instead of round-robin) to balance the reqs to servers. Let's say we want to hash based on the user's IP address.

One benefit of hashing approach compared to round-robin is the same user(same IP address) will always be routed to a specific server even though
that server is not the one with the lowest amount of traffic.

What is the benefit of this user always going to a specific server?

If our servers are REST APIs and therefore stateless, it doesn't really matter. But if each of the servers has redis cache attached to it and it's
not a shared cache and let's say these servers are not stateless, they are caching sth for their users. So this works with hashing load balancing.
But if we were doing round-robin the req goes to server 0 then we cache sth there, but next time it goes to another server, so we would have a
cache miss.

So by using consistent hashing, same user reqs are mapped consistently to the same server and we get caching.

So hashing in load balancing is more suited if we want user req to be mapped consistently(to the same server). This is where consistent hashing comes in.

The problem arise when we remove on the servers(maybe it crashed or ...). Ideally the users of server 1 should still go to server 1 and ... and
the user reqs of this down server should be balanced between other servers. Currently, the number of servers is changed.
So our hashing function gonna change. So the req of some users will be routed to other server and therefore we would have cache misses for the first
time and their data that was previously on cache of another server, is now useless.

This is not an intelligent way. We don't want to change where user's req is going even after a server goes down.

**Q:** How we can do consistent hashing?

A better approach(still using consistent hashing) is:

The idea is our servers are gonna be placed on some type of **circular** space. We take user reqs and instead of directly hashing and mapping
those to a server, we're gonna be mapping each req somewhere onto that circle using hashing function. If we map to the circle and there was no
server there, we're gonna keep moving **clockwise** on the circle and we assign that req to the first server or node that we arrive.

So far, this is exactly as our basic hashing approach was before, we're distributing reqs roughly evenly across servers. Now the different is going
to be when we remove one of the servers. You might be thinking: Now we our less servers, isn't our hashing function gonna change? and therefore
the servers handling the user's reqs gonna change and we would have cache misses and ... ?

No! the reqs will be routed to where ever they were routed before. Now the req of the crashed server will go clockwise on circle to the first
server they arrive(in theory). So load balancer will route the req of crashed server to server 0 and other reqs are still routed to the same
servers before. This is the best we can do.
![](../img/13-13-1.png)

Note: Ideally we want the reqs of the crashed server to be balancer between the remaining servers. But in our solution, all of those reqs will be
go to the next server on circle clockwise. So that first clockwise server will be handling about 2 times more reqs. There are solutions for this ... .

The same thing would happen as we **add** servers.
![](../img/13-13-2.png)

Consistent hashing helps us in:

- load balancing:
- CDN: Routing reqs to different CDN servers. We would in some cases keep that consistent(route to the same server).
- database: Imagine instead of servers, we had DB nodes. We split or shard our DB into for example 3 nodes and we keep a third
  of our user data on one node, a third on another and third on the other, to be able to handle more traffic and with consistent hashing,
  we route the req to the one that has his data.

Another hashing algo that is comparable to consistent hashing is rendezvous hashing. It tries to accomplishes the same thing as consistent hashing.

### Main components of consistent hashing

- hash key: In our case was the IP address. It needs to be sth that can identify a user and then we consistently map it to a node
- hash function: would be sth like a variation of sha(secure hashing algorithm)
- nodes: Like servers or DB partitions(shards). In servers, they can also be CDNs. The number of them can be increased or decreased and if we
  need to consistently map a user req to the same node, we need consistent hashing.

## 14-14 - SQL

Relational database management systems(RDBMS). SQL is a major component in most relational db management systems.

SQL is a query language - it's a way we can access the data that's stored in a RDBMS.

Data of relational DB management systems are stored on disk. Because we want the data to be persisted. Since we're gonna be storing a very very
large amount of data on disk, we want to organize it in a way where we can read and write from it efficiently.

The data structure that's typically used for RDBMSes is **B+ tree** which is kinda like a binary tree where each node doesn't have to have
at most 2 children, instead, it can have m children(arbitrary number) and also every node, every node doesn't have a single value, instead
we split each node into `m - 1` values. The reason we use m-way trees rather than binary trees is because this will help us reduce the size
of our tree. We want to minimize the number of reads and writes that we have to do.

Note: The entire of m-way trees(B+ trees) is not stored on disk because these trees can get very very large and to get the data we want, we
may traverse the entire height which can get very large. So increasing the number of children for every node is a way to reduce the height and
to to take this one step further, data is actually **only** stored in the leaf nodes of the tree and the non-leaf node's values just
help us get to the data(leaf nodes), all the data is stored at the leaf level and it's stored in a sort of linked list fashion(the values in
leaf nodes are ordered). This is how relational DBs are stored.

Tip: The way we key the data itself: Let's say we're organizing some data, let's say we're organizing it based on phonebook and that phonebook
maps a name to a phone number. Now how would we want to search for people in that phonebook? We would want to use their phone number or their
name?

Their name. So we key the tree based on name(it has alphabetical order).

When we key the tree based on some value, that value is called the **index**. We need to choose our index intelligently.

Indexes are about having some sorted property.

The main motivation is about being able to find and read data as quickly as possible.

What we can accomplish with RDBMSes?

With the table, we specify what the data needs to look like.

Primary key uniquely identifies every record(row) in a table.

Constraints are not usually possible with NOSQL DBs.

Q: What are the tradeoffs of relational databases when it comes to system design?

### ACID properties

The vast majority of RDBMses are ACID compliant:

- A - atomicity: **every DB transaction is all or nothing.** You can't split a DB transaction. If a part of transaction gets done but then
  our computer crashes and we don't do the other part of transaction and commit it, then the **entire** transaction is gonna fail(including the
  first part). In other words, if the entire transaction didn't complete, then none of it completed and if the entire transaction did complete,
  only then it's persisted(we don't commit half of the transaction). Example: Sending money from Alice to Bob. Note: Sometimes atomicity is not
  necessary.
- C - consistency: it means data consistency(there are different consistency that's used in different acronyms). This is following the constraints or
  rules that we as developers specify on DB(not null columns, foreign key constrains and ...). If we didn't have atomicity and isolation, it would be
  hard to follow the constraints that we specify, so we would end up in consistent state. Consistency is expensive to maintain. For example if
  we want to have a foreign key constraint, everytime we update our DB, we need to look out for these columns. So we create constraints or rules
  and our DBMS makes sure we follow those rules that we never reach an inconsistent state.
- I - Isolation: when we have multiple transactions happening concurrently on a DB, you want the transactions to appear as if they happened in order.
  We don't want multiple transactions to have side effects on each other. If we wouldn't have isolation, we would have dirty reads and
  phantom reads(non-repeatable reads) and other problems. **Dirty read:** Let's say we have 2 transactions running concurrently working on the
  same data and one of them commits which but another not and our DB crashes, so we would be left with wrong data. So while the first one did not
  commit but the second did commit(we have atomicity), we still left with wrong data. What went wrong? The second transaction read a value
  that was not committed yet and this is called a **dirty read** and it can lead us to inconsistent state. Isolation means these transactions are
  serialized, they will be executed in a particular order. So one of them will execute first and then the other one. So even though it kinda
  appears that the transactions are happening concurrently and they might be, because concurrently doesn't mean in parallel, they will appear
  as if they were executed serially in a particular order(one executed and completed before another one). The transactions behave as if they were
  performed serially.
- D - durability: This is why RDBMSes use disk. Because of this, redis is not ACID compliant(yeah it can be used as DB too and not only
  a cache - redis stores data in memory). A DB transaction is more than just a query, it could be a collection of queries. Every transaction
  that's been committed, we expect it to be durable(persisted) even if the DB crashed after that.

## 15-15 - NoSQL

NOSQL stands for **not only sql**. We don't have standard query language for NOSQL DBs.

A better name for these type of DBs is **non-relational**. So we can't use SQL to query them.

The biggest problem NOSQL are trying to solve is **scale**.

These DBs have a lot of variations.

### A few variations of NOSQL DBs

- key-value store: most simple variation. You can argue, most of these aren't necessary a DB. Because most of these are in-memory stores and they
  don't use disk redis, memcached, etcd. We would have a key and map it to object which is gonna be flat in nature. These are good in
  simple scenarios and if you need to read and write a lot of data very quickly, since they use RAM, they're fast. Usually key-value stores
  are used **together** with a primary DB and key-value stores will typically be used for cache.
- document store(document DB): a document typically is a JSON. Document would have some kind of primary key that is identifying it. The document
  is not flat in nature, it's nested. Like Mongodb. The benefits:
  - flexibility: it's a lot more flexible than a regular SQL DB where we have to define a **schema** for table.
    In NOSQL, we don't have schema for docs. We can do anything.
  - scale
- wide-column DBs: can handle massive massive scale, but they're typically oriented for a lot of writes like time-series data. These do have
  flexibility that document-based DBs have where we don't necessarily have to have a schema, but sometimes we **can** have a schema in wide-column DBs.
  It's better to use these DBs when you don't need to read and update a lot. After writing, we don't expect to update it again which would be the
  case with time-series data and other scenarios. Like Cassandra and google big table.
- graph-based DBs: These are actually all about **relations**! Storing people following each other can get hard in SQL tables and everytime
  you wanted the followings of a person and among those followings, what are the secondary connections(Friends of your friends)? These types of
  relations are hard to represent with SQL and it can get expensive when you have JOINs on multiple tables and you have to do that for millions of
  records in a table. It's better for these followings(social media) to be solved with graph(directed graphs).

### Motivation for NOSQL DBs

The biggest motivation is scalability. NOSQL DBs can generally scale alot better than SQL DBs and this is because of restrictions we put
on SQL(relational) DBs especially the ACID properties. The problem with this, is if we have a single SQL DB and it's really big and it's handling
a lot of reqs(like 1000 req/s) and we want to scale this DB up. **The easiest way to scale a SQL DB is vertically**. It's hard to scale horizontally
with relational DBs(SQL DBs) because of ACID and ... . Some problems when sharding(SQL DBs):

- Q: How do we know how to route a req to the correct DB(when we splitted out a DB into multiple nodes(shards))?
- How do JOINs between the shards? and we tried to enforce the constraints(foreign key, unique and ...)

**Tip:** Horizontal scaling is unlimited and it's better if it's possible(because it's much more powerful than vertically).

NOSQL DBs generally do not follow the ACID:

- atomicity: Many times they will have atomic transactions,
- consistency: they don't have consistency(they don't have foreign key constraints and ...)
- isolation: they don't necessarily perform transactions in isolation.
- durability: Generally they're durable(in memory stores are not durable though)

NOSQL DBs sacrifice ACID so that they can scale. For example when we shard them, since we don't have any foreign keys to worry about and
we're probably not gonna do JOINs between shards. So this allows us to split data into multiple nodes(shards) and horizontally scale the data.

NOSQL DBs have their own acronym like **BaSE** which boils down to eventual consistency.

### eventual consistency

Consistency here is different than consistency in ACID. In ACID, consistency meanas the data would follow the constraints that we specify.
But here, with replicas, consistency means the data should be in sync.

When we have multiple copies of DB(replication), we need them to be consistent. To be consistent, we choose leader and followers and everytime
we write, we're only going to write to the leader DB, we would never write to the follower DBs, because the leader is gonna be responsible
for all of followers being written the same data, so **eventually** the leader will make sure the data gets written to followers.

We sacrifice the fact that some people for a small portion of time, will see out of date data. For example number of followers which isn't
a big deal in most cases.

There's no guarantee on how long that time for eventual consistent would take.

Technically we could have replicas with SQL DBs as well.

Partitioning or sharding with SQL DBs can be done as well, but it's gonna be complex(because of ACID), but NOSQL DBs typically provide
this by default.

Sharding in SQL DBs is less common.

Partitioning = sharding

## 16-16 - Replication and Sharding

These two are different but related.

With single DB node, all of our app data is on that DB. As we get more traffic and have manny connections to this DB at the same time or
maybe we're processing so much data from DB(every query has to read through so much data) and because of these, we can't handle as many
reqs as we're getting.

We need replication. We create a copy of our DB(replicate it) and now we're able to handle more req. But replication has a lot of nuances and
tradeoffs and decisions.

### Leader-follower(master-slave) replication

This is for scaling up our reads.

When we replicate a DB, we create a follower DB of the initial one, the leader is the one that's responsible for replicating it's own data to one or
more followers.

Now the followers could also replicate data as well, or maybe the leader will be the one responsible for replicating to all the followers but that
doesn't make sense, it's ok if the followers pick up some of the work if they're able to do that.

We can read and write data to leader node. But with a leader-follower replication, we're only allowed to read from the follower DB and we're not
allowed to write to it, because the way our replication is working. Our leader is gonna replicate it's data to followers. But **if** we allow
the client(our server) write data to the follower, the follower is not replicating it's data to other nodes like leader.

Now obviously with this, we're able to scale **reads** which is usually more important. Usually reading is more common. Also the reliability and
availability are increased, because if one of the DBs goes down(like leader goes down), at least we have mostly up to date replica that has most
of the data. So it can take the leader's place.

There are different strategies for replicating data.

### tradeoffs in replicating

The tradeoff is async vs sync data replication.

Async is when we don't have to do it immediately. After writing to leader of course, in async approach, at some point leader will take this data
and replicate it to the followers.

The replication could be on schedule or could be a few seconds later the leader will take the transaction(the client made) and make that same
transaction on the followers.

The downside with async is other clients that are reading from the followers, may see inconsistent data. We would have inconsistencies between
leader and followers for some period of time.

In sync replication, everytime a user has a write transaction on the leader, the leader will stop and **immediately** take that write transaction
and replicate it to the followers and only after these are done, the transaction is considered complete. We won't have inconsistent data.
Downside is we have more latency. If the replicas are in different parts of world, latency would increase significantly.

### leader-leader replication(multi master replication)

In this case we have multiple leaders. The benefit here is we can read and write from every single node(all of them are leaders). We scale up reads **and**
writes. It has tradeoffs. OFC replicating data between multiple leaders can get very complicated. OFC these replicas are gonna get out of sync.
Some data will be written in one leader, some data will be written in another one, these replicas need to sync themselves.

With multiple leaders we'll have lower consistencies. But we'll scale up the writes.

With async replication in this case, our data will be loosely consistent(but it's definitely not going to be consistent) and with sync,
we'll have much higher latency depending on how many leaders we have and how they are geographically distributed around the world.

**Tip:** The main reason you'd want to use multi-leader replication is if you had one leader serving every continent of the world.
Now if these db nodes(leader nodes in this case) become out of sync, it's OK because everybody in that continent gonna be mainly interacting with
one the nodes and everybody from other side of world(let's say we have 2 leader nodes) is mainly gonna be interacting with the other one.

Now sometimes people from one side of world might fly to the other side, that's OK because we're gonna keep these nodes in sync but maybe every hour.
But we don't have to do it immediately. This is the main use case of multi-leader replication which is being able to serve different parts of
world roughly independently.

### Sharding

Implementing it is very hard.

Similar to replication, if we have a DB that's getting so much traffic that a single DB can't handle it. We try to horizontally scale this DB.
One approach would be replication. But sometimes that's still not enough. What if we had a massive amount of data, not TB but PT(petabytes) and
searching that amount of data in a single machine would take multiple seconds. This is where sharding comes in. We not only put this data on
multiple machines on multiple nodes and computers but we take the data itself and split it into smaller DBs. Literally we take a table and
for example put half of it's data into one DB which will be **on it's own machine**(if we put both DBs after sharding into one machine, that's not
solving any problems, we put them on different machines to have more resources) and the second half of table goes to other DB on separate machine.

These new DBs are called shards.

Now reading and writing will go to individual shards and not only we're able to handle more traffic, because now maybe half of traffic goes
to one shard(node) and rest to other shard. But also the queries themselves will run(on shards OFC) faster because of fewer data.

Q: How do we decide how to shard the DB?

#### There are a couple of approaches:

- range-based: we would take some range of values and some other range of values and decide what range do we actually use?
  That's called the **shard key** which is a special value that's used to decide how to split the data and one type of shard key is
  range-based shard key. For an individual table, we would typically choose the primary key to decide how to split the data.
  Using that primary key, we say that some range of values go to one shard and other to other shards. FOr example based on last name, we could say
  A-L is gonna go to first shard and second half go to other shard. But now, running JOINs on two halfs of a table is gonna be complicated(slow or
  might not work). When we shard, maybe the data in one shard is related to other shard. So keeping things consistent is hard, we need custom logic.
- hash-based sharding: This is a use case for consistent hashing. Consistent hashing is an algo to shard data(to decide how to distribute data
  into multiple shards)

Q: How do we know how to route a req to the correct DB(when we splitted out a DB into multiple nodes(shards))?

Popular SQL DBs like mysql and postgres, do not have sharding by default. If you want to shard them, you have to implement the logic for that
at application level. For example how to know where to find data(which shard has the data you're looking for) and it makes sense because SQL DBs
naturally are not meant to distributed in this way. When we do sharding SQL DBs, we're losing the consistency that we get from ACID where we know
the foreign key constraints and other constraints we specify in DB is gonna be enforced, we lost these with shards.

Most NOSQL DBs will have sharding by default(it will be implemented as part of DB management system). Nosql DBs are naturally meant to be sharded.
That was the whole point of nosql DBs that we can horizontally scale them better, because we're not going to be JOINing data that much,
we're not gonna have foreign key constraints, our data is non-relational. The data doesn't even have to be consistent in nodes, it will be
eventually consistent.

## 17-17 - CAP Theorem

Applies to distributed DBs(not a single DB, so only with replicated data).

- c: consistency
- a: availability
- partition tolerance

P is always part of our choice. So the real theorem is P is guaranteed and we need to choose between C **or** A.

### partition tolerance

In a distributed system like two replicas of DB(one leader, one follower) we would have a network of 2 nodes.
If we get a partition(network partition which means our system gets cut, so nodes can't talk to each other), users can still interact with
nodes individually. Partition tolerance means our system will continue to function if nodes become disconnected.
If there's a partition, we **could** allow the system to not function at all but in most cases we don't do this. That's why P is guaranteed.

### consistency:

In this case consistency means data consistency between the replicated DBs. Every single read will get the most uptodate written data.

Note: While our network is partitioned, none of the data from the leader will be written to follower. So if somebody writes to leader and then
reads from follower, he won't get the most uptodate data. This is an example where we don't have consistency. So every read doesn't get the
most up-to-date write.

Consistency in CAP is different than consistency from ACID. Because in ACID, consistency means a **single** node has consistent(following
constraints) data.

### availability

In partition, since the follower can't get the most up-to-date data from the leader, how about we disallow users from using just the
follower(giving up availability), but they can continue to read and write to leader. Because we know reading and writing to leader is
gonna have consistent(up-to-date) data. But we only achieve this consistency by giving up availability.

In order to have availability, every single node in our system(including the ones that don't have most up-to-date data) will respond to
valid reqs. So every node is available to respond to reqs.

So we can only choose consistency or availability. Actually it's not guaranteed we can get either of C or A. In most cases, it's OK if we have
inconsistent data. So we choose availability.

### PACLEC

PACLEC theorem is an extension of CAP. It's better than CAP.

PAC: is CAP theorem still

It means given P(when there is a partition), choose A or C. Else(if there's no network partition), favor latency or consistency.

So if there's no P, we can choose both A and C. But we will favor latency or consistency. Which means it would take time to backup or replicate
the new data to followers. Now we have to favor either latency or consistency. In other words, we let the user read data immediately but they may get
stale data(favor latency, we would have lower latency) **or** we give them consistent data, we give them the most up-to-date data even if they
have to wait for some time so we can replicate the new data.

## 18-18 - Object Storage

Newer than filesystem. It's much more comparable to a filesystem than it is to an actual DB. Because in DB how we structure and filter
and search for a data is much more important with DBs. But with object storage, data is stored in a flat way. There's no hierarchy, but with filesystems
we have folders and more folders and ... . In object storage there's not anything like folder.

AWS S3, google cloud storage, ... .

These services kinda give the illusion of a filesystem, with s3 you can have folders and files inside of them. But in that case, the folder is
actually just part of the name of the file, it's a way to give you that illusion, there aren't hierarchies with object storage, all of the objects
in S3 are stored in a flat way.

The predecessor to object storage was blob storage and when we talk about objects we mean blobs. Blob stands for(binary large object).
Media things like images, videos, a database backup(it's not media but it's a large object), we store files but typically not a regular text file that
we expect to update, because when we put files and objects in object storage, we can write those files to object storage(add files to object storage)
and then we can read from storage, but we can't update that file. That's one tradeoff with object storage.

Object storage is optimized to store a large amount of objects in a flat way(so no hierarchy) so that we can immediately find objects that we have
stored, sorta like a hash map where every object has a unique key so that gives us a quick way of accessing that file.

Also, we can't have duplicates in hash maps and that's another similarity with blob storage and object storage.

In object storage, we need a globally unique names, it's not enough to have a unique name in your s3 instances, but if you have a name conflict with
anybody else who's using that object storage(like s3) that doesn't work either, it should be global in whole s3.

### use cases

If we're storing large files like img, video, media or anything for long term storage in general, we could of course use DB and store them in DB.
But it doesn't make sense because are we ever gonna be querying based on image? or a video? Or we wanna use some metadata related to an img like
the name of the img or tag related to it or ... and we can store that metadata instead of the img itself in DB.

Another reason of not storing media in db is: videos and imgs are large files when we store them in DB, they're gonna slow down our queries, increase
our storage and we're gonna be reading and writing from DB a lot and having these stored in DB, makes it if a user wants to see an img or vid,
we have to read it from DB and from app, send that back. It's better to use object storage.

The interface for reading and writing files from object storage is with HTTP. So we don't have to query anything or write any SQL, we don't need
to read the entire object storage, we can make a network req to object storage, ask for whatever filename we want(we need to configure it so
we're the only ones who can access this file). So via HTTP, we get back the media and send it back to user.

## 19-19 - Message Queues

If we have a large amount of application events and those app events are going fast so much so that maybe our app server can't handle them
all at once and we want to process these events asynchronously.

Now of course we could scale up our servers but if we really don't need to do these processing all at once and at the same time, it's better
to queue these events and handle them at some later time and that's what message queues can provide for us.

The events will go to message queue and then from that queue, those events will then to our application server and finally our server will process
those. For example the payment events. The queue is durable(persisted). If the queue crashes, the data will still remain there. It's not
stored in RAM, it's usually being stored on disk.

We decoupled the events. It could be it's own server creating events. In other words, we've decoupled these services - the one that
will produce the events and then one that will receive and process those events. Tbe queue architecture will allow us to process the events
asynchronously and allow us to handle a larger amount of scale because those events don't need to be processed instantly, we can queue them.
With this type of example, we probably process them FIFO.

Pros of message queues:

- durable: So in some ways they're similar to DBs
-

The way data is transported between the queue and the destination(app server), can vary. It can either be **pulling** where the destination is
pulling from the queue. It's periodically checking does the queue have any new messages? If it does, I'm gonna take some of them.

We can also **push** messages directly from the queue to the destination whenever we get them on demand.
Now in some cases, if we were pushing data, the app server(destination) may not be able to process the data immediately or maybe it may have
missed the message or sth may have gone wrong. For this, there's a way for the destination to tell the queue it can acknowledge that it received
a particular message(it can ack that message) and after that's done, the queue will know that this particular message that it was trying to send,
has been received and processed by the destination, so it doesn't need to send that message anymore. If the queue sent a message
and it did not receive an acknowledgment from the server, then the queue would try again. It would keep trying to send the message a certain number
of times or until the server acknowledge that it received the message and processed it. This is a way where we don't loose messages. We don't send
messages to the destination that it can't process and then those messages get dropped(we never see them again), we don't want that to happen. For this,
there's a lot of features like this that are built into queues to make them durable and reliable.

There could be multiple apps feeding into the queue and could be multiple apps receiving from the queue and this brings us to pub/sub were we
get this sort of many-to-many relationships.

### Pub/sub (publisher/subscriber)

Variation of message queueing. In this case, the app event producers will be publishers and the event receivers will be the subscribers.
These events will go to some middle layer(part of message queue), we will get topics and these topics will be the ones that receive and store
those messages from publishers and persist them and topics will feed into subscriptions. There could be multiple subscriptions that a single topic
is feeding into and the purpose of this is that multiple apps can receive from the same topic.

For example a single topic may receive all payment data and another topic receives analytics info and this analytics topic will feed into
a single subscription and we have a single app server to receive all the analytics events and then we write them to some logs DB.
But for payments info, we need 2 servers to receive them One server is going to process that payment and then another server is going to do sth
with it, maybe back it up to a DB or a data warehouse.

Topics receive data and segregate data. Maybe we want half of data to go to this topic and the other half of data is completely unrelated, so we want
them to go to another topic.

**Subscriptions** are a way for us to fan-out these topics. Because maybe it's not just a single app that needs this topic of message, but maybe
we need multiple apps, so we fan it out to multiple subscriptions and then that's where our application logic comes in, it will receive messages
from subscription(a single app could receive from multiple subscriptions if we want it to).

The whole idea is we have all this logic in the message queue so that we decouple our app. So we can get rid of a server(a destination) and replace it
and when doing this, we don't have to change our entire architecture. That's the benefit.

</details>
