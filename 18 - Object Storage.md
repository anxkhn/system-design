Here are the detailed and verbose notes on Object Storage:

## Introduction to Object Storage

Object storage has become a crucial storage mechanism in the last 10-15 years. It is a relatively new concept compared to traditional file systems, but it is more comparable to a file system than a database. Unlike databases, where data is structured and filtered, object storage stores data in a flat way, without any hierarchy.

## Key Characteristics of Object Storage

- No hierarchy: Unlike file systems, object storage does not have folders or subfolders. Instead, all objects are stored in a flat way.
- Flat storage: Objects are stored in a flat namespace, without any hierarchical structure.
- No editing: Once an object is stored, it cannot be edited or updated. This is a trade-off for the benefits of object storage.
- Unique keys: Each object has a unique key, which allows for fast and efficient retrieval.
- Optimized for large files: Object storage is designed to store large amounts of large files, such as media files (images, videos), database backups, and other types of binary data.

## Popular Examples of Object Storage

- AWS S3 (Simple Storage Service)
- Google Cloud Storage
- Azure Blob Storage

## How Object Storage Works

- Objects are stored in a flat namespace, without any hierarchical structure.
- Each object has a unique key, which is used to retrieve the object.
- Objects can be written, read, and deleted, but not edited or updated.
- Object storage is optimized for storing large amounts of large files, making it ideal for media storage, database backups, and other types of binary data.

## Use Cases for Object Storage

- Storing large files, such as images, videos, and database backups.
- Long-term storage of infrequently accessed data.
- Storing metadata, such as image or video tags, titles, and descriptions.

## Design Perspective

- When designing a system, object storage is a good choice when dealing with large files that need to be stored and retrieved efficiently.
- Object storage is optimized for storing large amounts of large files, making it a better choice than databases for storing media files and other types of binary data.
- From a system design perspective, object storage is relatively easy to reason about, as it is designed to store large files in a flat namespace.

## Comparison to File Systems and Databases

- Object storage is similar to file systems in that it stores files, but it does not have a hierarchical structure like file systems do.
- Object storage is similar to databases in that it stores data, but it is optimized for storing large files, whereas databases are optimized for storing structured data.
- Object storage is more comparable to a file system than a database, as it is designed to store files, rather than structured data.

## Security Considerations

- Object storage provides security capabilities, such as access control and encryption, to ensure that stored objects are protected from unauthorized access.
- When designing a system that uses object storage, it is important to consider security issues, such as access control, encryption, and data protection.

## HTTP Interface

- Object storage provides an HTTP interface for reading and writing files.
- This interface allows for efficient and scalable access to stored objects.
- When retrieving an object, the system can simply make an HTTP request to the object storage service, without having to query a database or search through a file system.
