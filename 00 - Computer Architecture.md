# Computer Architecture

## Components of a Computer

### Disk/Storage/Hard Disk Drive (HDD)/Solid-State Drive (SSD)

- Stores data persistently
- Data is retained even after computer restarts or crashes
- Capacity is measured in gigabytes (GB) or terabytes (TB)
  - 1 TB = 10^12 bytes (approx. 1 trillion bytes)
  - 1 GB = 10^9 bytes

### Random Access Memory (RAM)

- Used for storing information temporarily
- Smaller capacity compared to disk (typically 2-32 GB)
- More expensive than disk storage
- Much faster for reading and writing data (measured in microseconds, 10^-6 seconds)

### Central Processing Unit (CPU)

- The "brain" of the computer
- Performs read and write operations on RAM and disk
- Executes code (compiled from high-level languages to CPU instructions)
- Performs computations and arithmetic operations

### Cache

- Part of the CPU
- Small capacity (measured in megabytes, MB)
- Extremely fast read and write speed (measured in nanoseconds, 10^-9 seconds)
- Used to store frequently accessed portions of RAM for improved performance

## Data Flow

1. Code and data are stored on the disk
2. CPU reads code from disk and loads it into RAM
3. CPU reads data from disk and loads it into RAM
4. CPU executes code from RAM
5. CPU reads and writes data in RAM during execution
6. Frequently accessed data from RAM is cached in the CPU cache for faster access
7. Data is written back to disk for persistent storage

## Limitations

- Disk capacity is limited
- RAM capacity is limited
- CPU speed is limited (Moore's Law is slowing down)
- Individual computers have hardware limitations

## Distributed Systems

- Combining multiple computers can overcome limitations of individual machines
- Allows solving larger problems by leveraging combined resources
- Distributed systems have other benefits beyond scalability
