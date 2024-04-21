# DNS

**Introduction to DNS**

The Domain Name System (DNS) is a decentralized system that plays a crucial role in the internet infrastructure. It is a complex system, but we will be looking at it in a simplified way to understand its basic concepts and components.

**The Problem DNS Solves**

Imagine you want to access Google.com. You need to send a request to the Google.com server, but you don't want to type in the IP address of the server (e.g., 192.0.2.1). Instead, you want to use a user-friendly name (Google.com) that points to the IP address. This is similar to having a phone book or contact list on your phone, where you map each person's name to their phone number.

**How DNS Works**

At a high level, DNS works as follows:

1. You type in a domain name (e.g., Google.com) in your browser.
2. The DNS system translates the domain name into an IP address.
3. Your browser sends a request to the IP address.
4. The server at the IP address responds to your request.


**Components of DNS**

There are several components involved in the DNS system:

1. **Internet Corporation for Assigned Names and Numbers (ICANN)**: ICANN is a nonprofit organization that owns all domains in the world. When you type in a domain name, ICANN is involved in resolving the domain to an IP address.
2. **Domain Registrars**: Domain registrars are accredited resellers of domains. They sell domains on behalf of ICANN. Examples of domain registrars include Google Domains, Namecheap, and GoDaddy.
3. **Internet Service Provider (ISP)**: Your ISP is a crucial component of the DNS system. When you send a request to a domain, your ISP interacts with the DNS components to resolve the domain to an IP address.
4. **DNS Servers**: DNS servers are responsible for storing DNS records. A DNS record is a database entry that maps a domain name to an IP address. There are many types of DNS records, including A records (Address records), which map a domain name to an IP address.

**How DNS Resolution Works**

Here's a high-level overview of how DNS resolution works:

1. Your request goes through your ISP.
2. Your ISP interacts with ICANN and domain registrars to resolve the domain to an IP address.
3. The request is forwarded to a DNS server, which stores the DNS record for the domain.
4. The DNS server returns the IP address associated with the domain.
5. Your browser sends a request to the IP address.
6. The server at the IP address responds to your request.

**Caching**

To improve performance, DNS caching is used to store frequently accessed DNS records. When you type in a domain name, the DNS system checks the cache to see if the IP address is already stored. If it is, the request is sent directly to the IP address without going through the DNS resolution process.

**Components of a URL**

A URL consists of several components:

1. **Protocol**: The protocol is the first part of the URL (e.g., http or https).
2. **Domain**: The domain is the main part of the URL (e.g., google.com).
3. **Top-Level Domain (TLD)**: The TLD is the last part of the domain (e.g.,.com,.io, or.jp).
4. **Subdomain**: The subdomain is an optional part of the domain (e.g., www or domains).
5. **Path**: The path is the part of the URL that follows the domain (e.g., /about or /contact).
6. **Query Parameters**: Query parameters are optional parameters that follow the path (e.g.,?name=John&age=30).
