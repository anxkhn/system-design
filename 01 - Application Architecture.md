# Application Architecture for Production Apps

## Developer's Perspective

### Code Deployment

- Developers write code that needs to be deployed and run on a server
- Server: A computer that can handle requests and serve users
- Code is built and deployed before reaching the server
- Can happen on developer's local machine
- More commonly done on a CI/CD (Continuous Integration/Continuous Deployment) server

### Data Storage

- Server needs to store data
- External storage mechanism (e.g., database or other persistent storage)
- Server may have its own disk storage, but it has limitations

### User Interaction

- Users communicate with the server through requests (e.g., from a browser)
- Server responds with code (HTML, JavaScript) for front-end applications
- Server can also be a backend API responding with data (e.g., JSON)

## Scaling

### Vertical Scaling

- Improving a single server's resources (CPU, RAM, disk) to handle more requests
- Simple conceptually, but has limitations (finite resources)

### Horizontal Scaling

- Adding more servers to handle requests
- Load Balancer distributes requests across multiple servers
- Servers can communicate with external APIs and services

## Monitoring and Logging

### Logging

- Similar to local print statements, servers log information
- Logs stored in an external logging service for developers to access

### Metrics

- Collecting metrics on server performance and resource utilization
- Log-based metrics can be derived from logs
- Metrics displayed as time-series charts for analysis

### Alerting

- Alerting service monitors metrics and sends notifications when thresholds are breached
- Allows developers to be notified immediately when issues occur

## Networking

- Components communicate over a network, even if not on the same machine
- Understanding networking basics is important for designing large-scale applications
