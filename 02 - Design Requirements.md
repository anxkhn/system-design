# Design Requirements

## Introduction

- The goal of system design is to create effective systems to solve big problems
- System design is not about memorizing facts, but about the thought process and analyzing improvements and trade-offs
- In system design interviews, there's not always a straightforward answer, and we need to weigh the trade-offs and figure out which option is best

## Moving, Storing, and Transforming Data

- In larger systems, data can be moved between machines that aren't running as part of the same machine
- Moving data across a network is not straightforward
- We're also concerned with how we store data, which can get complicated
- Storing data in larger systems follows some of the same ideas as storing data in individual computers (e.g., arrays vs. binary search trees)
- We want to transform data, which means manipulating it to get something more useful

## Main Design Requirements

- **Availability**: the percentage of time that a system is functioning correctly
  - Measured as uptime / (uptime + downtime)
  - Can be represented as a percentage (e.g., 99%, 99.9%, 99.99%)
  - Five nines (99.999%) is a high level of availability

| Availability Percentage | Downtime per Year | Downtime per Month | Downtime per Week | Downtime per Day  |
| ----------------------- | ----------------- | ------------------ | ----------------- | ----------------- |
| 90%                     | 36.5 days         | 216 hours          | 50.4 hours        | 7.2 hours         |
| 99%                     | 3.65 days         | 7.20 hours         | 1.68 hours        | 14.4 minutes      |
| 99.9%                   | 8.76 hours        | 43.8 minutes       | 10.1 minutes      | 1.44 minutes      |
| 99.99%                  | 52.6 minutes      | 4.38 minutes       | 1.01 minutes      | 8.64 seconds      |
| 99.999%                 | 5.26 minutes      | 26.3 seconds       | 6.05 seconds      | 864 milliseconds  |
| 99.9999%                | 31.5 seconds      | 2.63 seconds       | 605 milliseconds  | 86.4 milliseconds |

- **Service Level Objective (SLO)**: a target for the availability of a system
- **Service Level Agreement (SLA)**: an agreement with a customer that includes the SLO and consequences for not meeting it
- **Reliability**: the probability that a system won't fail
- **Fault Tolerance**: the ability of a system to continue functioning even if one part of the system fails
- **Redundancy**: having multiple copies of a system or component to ensure fault tolerance
- **Throughput**: the amount of operations or data that a system can handle over a period of time
  - Measured in requests per second (RPS), queries per second (QPS), or bytes per second
- **Latency**: the amount of time it takes for an operation to complete
  - Measured in seconds or milliseconds

## Trade-Offs and Measurements

- Availability, reliability, and fault tolerance are related but distinct concepts
- Throughput and latency are related but distinct concepts
- We need to balance these measurements when designing a system
- We can use techniques like horizontal scaling, vertical scaling, load balancing, and content delivery networks (CDNs) to improve these measurements

## System Design Principles

- We want to design systems that can handle failures and high throughput efficiently with low latency
- We need to consider the trade-offs and measurements when designing a system
- We'll spend the rest of the course expanding on these principles and focusing on how to satisfy these requirements.
