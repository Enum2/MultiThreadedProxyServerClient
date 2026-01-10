# 🚀 Multithreaded HTTP Proxy Server with LRU Cache

A fully functional **Multithreaded HTTP Proxy Server** implemented in **C**, designed to handle multiple client requests concurrently while improving performance using an **LRU (Least Recently Used) caching mechanism**.  
The project uses **POSIX threads**, **semaphores**, and **mutex locks** to ensure thread safety and efficient concurrency control.

---

## 📌 Project Overview

This proxy server acts as an intermediary between clients and remote web servers.  
It intercepts HTTP GET requests, checks if the response is already cached, and either serves it directly (cache hit) or forwards the request to the remote server (cache miss).

The project demonstrates core concepts of:
- Computer Networks
- Operating Systems
- Multithreading
- Synchronization
- Memory management

---

## ✨ Features

- ✅ Supports HTTP **GET** requests
- 🧵 Multithreaded client handling using `pthread`
- 🚦 Semaphore-based control for maximum concurrent clients
- 🗂 In-memory **LRU Cache** using linked list
- 🔒 Thread-safe cache access using mutex locks
- ⚡ Faster response time for cached requests
- ❌ Proper HTTP error handling (400, 403, 404, 500, 501, 505)

---

## 🧠 System Architecture

Clients (c1, c2, c3...)
|
v
Proxy Listening Socket (proxysocketid)
|
v
Semaphore (MAX_CLIENTS limit)
|
v
Worker Threads (pthread_create)
|
v
LRU Cache <----> Remote Web Server
|
v
Client Response


---

## 🔄 Request Flow

1. Client sends an HTTP GET request to the proxy.
2. Proxy accepts the connection and spawns a new worker thread.
3. Semaphore ensures the number of active clients does not exceed `MAX_CLIENTS`.
4. Proxy checks the LRU cache for the requested URL.
   - **Cache HIT** → Response is sent directly to the client.
   - **Cache MISS** → Request is forwarded to the remote server.
5. Response from the remote server is:
   - Streamed to the client.
   - Stored in cache with LRU metadata.
6. If cache is full, the least recently used entry is evicted.
7. Thread closes connection, releases semaphore, and exits.

---

## 🗂 LRU Cache Design

- Cache implemented using a **singly linked list**
- Each cache entry stores:
  - URL (request key)
  - Response data
  - Length of data
  - `lru_time_track` timestamp
- **Most Recently Used (MRU)** element is kept at the head
- **Least Recently Used (LRU)** element is removed when cache exceeds size limit

---

## 🔐 Synchronization Mechanisms

| Resource | Mechanism Used |
|--------|----------------|
| Client concurrency | Semaphore |
| Cache operations | Mutex lock |
| Thread handling | POSIX threads |

---

## 📁 Project Structure

.
├── proxy_server_with_cache.c // Main proxy server implementation
├── proxy_parse.c // HTTP request parsing logic
├── proxy_parse.h // Parser definitions
├── Makefile // Build instructions
└── README.md // Project documentation


---


---

## ⚙️ Build Instructions

Compile the project using the following commands:

```bash
g++ -g -Wall -o proxy_parse.o -c proxy_parse.c -lpthread
g++ -g -Wall -o proxy.o -c proxy_server_with_cache.c -lpthread
g++ -g -Wall -o proxy proxy_parse.o proxy.o -lpthread

▶️ Running the Proxy Server

./proxy <PORT_NUMBER>


curl -x localhost:8080 http://example.com

🚫 Limitations

Supports only HTTP GET requests

HTTPS (CONNECT method) is not supported

Cache is stored in memory (non-persistent)

🚀 Future Enhancements

HTTPS support using CONNECT tunneling

Persistent disk-based cache

Thread pool optimization

Improved cache eviction strategies

👨‍💻 Author

Sujal Suryawanshi
Multithreaded HTTP Proxy Server


---

If you want next, I can:
- Add **diagram images**
- Create **project report (PDF/Word)**
- Write **viva questions & answers**
- Prepare **PPT slides**

Just tell me 👍
