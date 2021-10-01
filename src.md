Web Systems Development
Content:

Client-Server Model
Network Protocols
Storage
Latency & Throughput
Availability
Caching
Proxies
Load Balancers
Hashing
Relational Databases
Key-Value Stores
Specialized Storage Paradigms
Replication & Sharding
Leader Election
Peer-to-Peer Networks
Polling and Streaming
Configuration
Rate Limiting 
Logging and Monitoring 
Publish/Subscribe Patterns
MapReduce
Security and HTTPS
API Design

How to
-------------
How to deploy Nodejs on DigitalOcean 

Recommended Books
----------------- 








MapReduce 

MapReduce is a programming model for processing and generating big data sets with a parallel, distributed algorithm on a cluster”
MapReduce
A popular framework for processing very large dataset =s in a distributed setting efficiently, quickly, and in a fault-tolerant manner. A MapReduce job is comprised of 3 main steps:

The map step, which runs a map function on the various chunks of the dataset and transforms these chunks into intermediate key-value pairs. 


The shuffle step, which reorganizes the intermediate key-value pairs such that pairs of the same key are routed to the same machine in the final step. 


The Reduce step, which runs a reduce function on the newly shuffled key-value pairs and transforms them into meaningful data. 

The canonical example of a MapReduce use case is counting the number of occurrences of words in a large text file. 


When dealing with a MapReduce library, engineers only have to worry about the map and reduce functions, as well as their inputs and outputs. All other concerns, including the parallelization of tasks and the fault-tolerance of the MapReduce job, are abstracted away and taken care of by the MapReduce implementation. 
Distributed File System
Is an abstraction over a (usually large) cluster of machines that allows them to act like large file systems. 
The two most popular implementations of a DFS are:
Google File System 
Hadoop Distributed File System (HDFS)

Typically DFSs take care of the classic availability and replication guarantees that can be tricky to obtain in a distributed-system setting. The overarching idea is that files are split into chunks of a certain size (4MB or 64MB for instance), and those chunks are sharder across a large cluster of machines. A central control machine/node is in charge of deciding where each chunk resides, routing reads to the right nodes, and handling communication between machines. 

Different DFS implementations have slightly different  APIs and semantics, but they achieve the same common goal: extremely large-scale persistent storage. 
! Hadoop
A popular, open-source framework that supports MapReduce jobs and many other kinds of data-processing pipelines. Its central component is HDFS, on top of which other technologies have been developed.  

Security and HTTPS

Man-In-The-Middle Attack
An attack in which the attacker intercepts a line of communication that is thought to be private by its two communicating parties. 

If a malicious actor intercepts and mutates an IP packet on its way from a client to a server, that would be a man-in-the-middle attack. 

MITM attacks are the primary threat that encryption and HTTPS aim to defend against. 

Symmetric Encryption
A type of encryption that relies on only a single key to both encrypt and decrypt data. 
The key must be known to all parties involved in communication and must therefore ty[ically be shared between the parties.

Symmetric-key algorithms tend to be faster than their asymmetric counterparts. 

The most widely used symmetric-key algorithms are part of the Advanced Encryption Standard (AES).

Asymmetric Encryption
Also known as public-key encryption, asymmetric encryption relies on two keys to encrypt and decrypt data:
Public key 
Private key
The keys are generated using cryptographic algorithms and are mathematically connected such that data encrypted with the public key can only be decrypted with the private key. 

While the private key must be kept secure to maintain the fidelity of this encryption paradigm, the public key can be openly shared. 

Asymmetric-key algorithms tend to be slower than their symmetric counterparts.

Advanced Encryption Standard (AES)
Is a widely used encryption standard that has three symmetric-key algorithms (AES-128, AES-192, and AES-256).

Of note, AES is considered to be the “gold standard” in encryption and is even used by the US National Security Agency (NSA) to encrypt top security information.

HTTPS 
The HyperText Transfer Protocol Secure is an extension of HTTP that’s used for secure communication online, it requires servers to have trusted certificates (usually SSL certificates) and uses the transport Layer Security (TLS), a security protocol built on top of TCP, to encrypt data communicated between a client and a server. 
 
TLS
The Transport Layer Security is a security protocol over which HTTP runs in order to achieve secure communication online. “HTTP over TLS” is also known as HTTPS.

SSL Certificate
A digital certificate granted to a server by a certificate authority. Contains the server’s public key, to be used as part of the TLS handshake process in an HTTPS connection

An SSL certificate effectively confirms that a public key belongs to the server claiming it belongs to them. SSL certificates are a crucial defense against man-in-the-middle attacks. 

Certificate Authority 
A trusted entity that signs digital certificates - namely, SSL certificates that are relied on in HTTPS connections.

TLS Handshake
The process through which a client and a server communicating over HTTPS exchange encryption-related information and establish a secure communication. The typically steps in a TLS handshake are roughly as follows:


The client sends a client hello a string of random bytes - to the server.


The server responds with a server hello - another string of random bytes - as well as its SSL certificate, which contains its public key.


The client verifies that the certificate was issued by a certificate authority and sends to the server a premaster secret - yet another string of random bytes, this time encrypted with the server’s public key


The client and the server use the client hello, the server hello, and the premaster secret to then generate the same symmetric-encryption session keys, to be used to encrypt and decrypt all data communication during the remainder of the connection. 

In other words: at the handshake stage, the client and server use asymmetric encryption to share a mutual key which is then used as a key for symmetric encryption for the rest of the communication. In this way the security (i.e. encryption/decryption) uses less computing power in the communication process. 
API Design

Pagination
When a network request potentially warrants a really large response, the relevant API might be designed to return only a single page of that response (i.e., a limited portion of the response), accompanied by an identifier or token for the client to request the next page if desired. 

Pagination is often used when designing List endpoints. For instance, an endpoint to Google results could return a huge list of results. This wouldn’t perform very well on mobile devices due to the lower network speed and simply wouldn’t be optimal, since most users will only scroll through the first few results of that list. So, the API could be designed to respond with only the first few results of that list; in this case, we would say that the API response is paginated.

CRUD Operation
Stands for Create, Read, Update, Delete Operations. These four operations represent the operation the API is designed to perform.  

HTTP defines a set of request methods (referred to as HTTP verbs) to indicate the desired action to be performed for a given resource.
Each of them implements a different semantic, but some common features are shared by a group of them: e.g. a request method can be: 
Safe - doesn't alter the state of the server
Idempotent -identical request can be made once or several times in a row with the same effect while leaving the server in the same state 
Cacheable -  is an HTTP response that can be cached


Table summarizing recommended API design 
 
HTTP Verb
CRUD
Entire Collection (e.g. /customers)
Specific Item (e.g. /customers/{id})
POST
Create
201 (Created), ‘Location’ header with link to /customers/{id} containing newly created ID
404 (Nor Found) or
409 (Conflict) -if resource already exists
Get
Read
200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists.
200 (OK), single customer. 
404 (Not Found), if ID not found or invalid.
PUT
Update or replace
405 (Method Not Allowed), unless you want to update/replace every resource in the entire collection.
200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.
Patch
Update or
modify
405 (Method Not Allowed), unless you want to modify the collection itself.
200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.
DELETE 
Delete
405 (Method Not Allowed), unless you want to delete the whole collection—not often desirable.
200 (OK). 404 (Not Found), if ID not found or invalid.










Messaging Tool
Url: https://headspring.com/2019/07/09/kafka-or-rabbitmq-messaging/

The right messaging tool for you: Kafka or RabbitMQ?

How do they compare head to head?
➡ RabbitMQ cannot be used as a store; Kafka can.

➡ In RabbitMQ, ordering is not guaranteed once we have multiple consumers. Kafka guarantees order for a partition in a topic.

➡ Messages can’t be replayed by RabbitMQ—they have to be resent those from the sending side. We do this with the Message Outbox pattern. Kafka stores data in the order it comes in and supports message replay with the help of offsets. However, it introduces other tradeoffs around data compaction, how long to keep the data on the streams, what to do if data required predates the stream, etc.

➡ RabbitMQ doesn’t support transactions natively, it uses acknowledgments. Kafka supports transactions.

➡ RabbitMQ has great .NET support—it completely outshines Kafka in this regard. Kafka treats .NET support as a secondary priority.

➡ RabbitMQ has good tooling for management on Windows. Kafka does not.

➡ RabbitMQ implements the Advanced Message Queuing Protocol. These guardrails help you stumble into a pit of success. With Kafka, you will have to implement a lot of these patterns and disciplines yourself.

➡ RabbitMQ doesn’t need an outside process running. Kafka requires Zookeeper’s running instance for its broker management. Zookeeper is responsible for assigning a broker for the topic.

➡ Out of the box, RabbitMQ is behind in multithreading support compared to Kafka—but not by much. Since NServiceBus works with RabbitMQ and has good support for multithreading, it is lesser of a problem for RabbitMQ. In both worlds, ordering is not guaranteed if the consumers are scaled out or have fetching records using multiple threads.

➡ RabbitMQ has a lot of plugins to support your needs. Kafka is not as mature and therefore doesn’t have as many plugin options.

There is no one tool superior to another, it all depends on your use case. There is no perfect tool.

Know in-depth: https://lnkd.in/dYNg2Zf


articles - git 
------------------
* https://linuxize.com/post/how-to-configure-git-username-and-email/

* https://careerkarma.com/blog/git-permission-denied-publickey/

* Recommended and secure method: SSH
https://stackoverflow.com/questions/35942754/how-can-i-save-username-and-password-in-git
