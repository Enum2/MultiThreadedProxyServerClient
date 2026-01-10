🧩 Multithreaded HTTP Proxy Server with LRU Cache
📌 Overview

This project implements a multithreaded HTTP proxy server in C that handles multiple client requests concurrently, forwards valid HTTP GET requests to remote web servers, and caches responses using an LRU (Least Recently Used) caching policy to improve performance.

The proxy server uses POSIX threads, semaphores, and mutex locks to ensure efficient concurrency control and thread-safe cache operations.

✨ Features

Supports HTTP GET requests

Multithreaded client handling using pthread

Semaphore-based control for maximum concurrent clients

LRU Cache implementation using a linked list

Cache HIT / MISS handling

Thread-safe cache access using mutex

Handles common HTTP errors (400, 403, 404, 500, 501, 505)

🏗 System Architecture

Clients (c1, c2, ...)
        |
        v
Proxy Listening Socket (proxysocketid)
        |
        v
Semaphore (MAX_CLIENTS limit)
        |
        v
Worker Threads (pthread)
        |
        v
LRU Cache  <---->  Remote Web Server
        |
        v
Client Response

🔄 Request Flow

Client sends an HTTP GET request to the proxy.

Proxy accepts the connection and creates a new thread.

Semaphore ensures client limit is not exceeded.

Proxy checks if the request exists in the cache:

Cache HIT → Response sent directly to client.

Cache MISS → Request forwarded to remote server.

Remote server response is:

Streamed to the client.

Stored in cache with LRU metadata.

If cache is full, least recently used entry is evicted.

Thread releases resources and exits.

🗂 Cache Design (LRU)

Implemented as a singly linked list

Each node contains:

URL (request key)

Response data

Length of data

lru_time_track timestamp

Most recently used item is moved to the head.

Least recently used item is removed when cache is full.

🔐 Synchronization
Resource	Mechanism
Client limit	Semaphore
Cache access	Mutex lock
Threads	POSIX pthreads

Project Structure
.
├── proxy_server_with_cache.c   // Main proxy server implementation
├── proxy_parse.c               // HTTP request parsing logic
├── proxy_parse.h               // Header for parser
├── Makefile                    // Build instructions
├── README.md                   // Project documentation

⚙️ Build Instructions

Compile the project using:

g++ -g -Wall -o proxy_parse.o -c proxy_parse.c -lpthread
g++ -g -Wall -o proxy.o -c proxy_server_with_cache.c -lpthread
g++ -g -Wall -o proxy proxy_parse.o proxy.o -lpthread

▶️ How to Run
./proxy <PORT_NUMBER>


Example:

./proxy 8080


Configure your browser or curl to use:

Proxy: localhost
Port: 8080

🧪 Example Test
curl -x localhost:8080 http://example.com

🚫 Limitations

Supports only HTTP GET requests

Does not support HTTPS (CONNECT)

Cache stored in memory (non-persistent)

📈 Future Improvements

HTTPS support using CONNECT tunneling

Cache persistence

Improved eviction strategy

Thread pool optimization

👨‍💻 Author

Sujal Suryawanshi
Multithreaded Proxy Server Project