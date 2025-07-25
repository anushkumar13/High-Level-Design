# Day 2 — Scalability Concepts & System Design Patterns (My Notes)

These are my raw notes from Day 2 of learning High-Level Design (HLD). Today I read about rate limiters, consistent hashing and how distributed key-value stores work. Some stuff was easy to understand, some went a little over my head but I’m slowly getting it.

---

### Designing a Rate Limiter

So basically rate limiter is something which stops people from sending too many requests in a short time. Like suppose someone is hitting your API again and again, it can slow down or crash the system. Rate limiter helps to block extra requests and protect the server.

There are few different ways to make a rate limiter. Each one have its own pros and cons.

---

### Algorithms for Rate Limiting

**1. Token Bucket**
Each user gets a bucket filled with tokens. Tokens are added at a fixed speed. To send a request, user need to use 1 token. If no token is left, request is blocked. It allows some sudden burst but still controls the limit.

**2. Leaking Bucket**
Like imagine a bucket with small hole leaking water slowly. Incoming requests are like water. If water (requests) come too fast and bucket is full, extra requests are dropped. It's useful to keep a steady flow.

**3. Fixed Window Counter**
Set a fixed time like 1 min. Count how many requests came in that minute. If user hits the limit, block rest of the requests till window resets. Problem is, if someone sends at end of 1st window and start of 2nd, they can bypass limit.

**4. Sliding Window Log**
Store time of each request in a log. Every time a new request comes, check how many requests happened in last X seconds. It's very accurate but uses more memory.

**5. Sliding Window Counter**
This is like mix of fixed window and sliding log. It keeps counters for small windows and gives a better estimation. It's more balanced in terms of memory and accuracy.

---

### Designing Consistent Hashing

This topic was a bit confusing first but made sense later. Usually to store keys in servers we do something like `hash(key) % n`. But when a new server is added or removed, all keys gets remapped — which is very bad.

**Consistent Hashing** fixes this. It puts both keys and servers on a circle (hash ring). Each key goes to the next server on the ring. So when server is added/removed, only few keys move — not all. This is good for load balancing and system stability.

Also there is something called **virtual nodes** — like same server appears multiple times on the ring to spread the load better.

---

### Designing a Key-Value Store

Key-value store is just like a big hashmap but on the network. Key maps to value. That’s all. But if you want to scale it:

* Hash the key to know where to store it (use consistent hashing).
* Divide data into parts (shards).
* Use multiple servers and copy data to backup (replication).
* Also handle read/write requests properly.

---

### CAP Theorem

This thing is important in distributed systems. It says you can’t get all 3 together: **Consistency**, **Availability**, and **Partition Tolerance**.

* If you want Consistency and Partition Tolerance (CP), system may become unavailable.
* If you want Availability and Partition Tolerance (AP), system may return old/stale data.
* All 3 is not possible, you have to pick only 2.

Helps to make better decisions depending on system needs.

---

### Inconsistency Resolution: Versioning

In distributed systems, same data can be written on different servers at different time. This causes conflicts.

To fix this, systems use **versioning**. Each version of data gets a number or timestamp.

There are few ways to solve conflicts:

* Use latest version (based on time)
* Use vector clocks to track changes from multiple nodes
* Manual fix (not used often)

I saw this in Amazon Dynamo paper. Not fully understood everything but basic idea is: systems should be ready to handle conflicts and out-of-sync data.

---

That’s it for Day 2. A little heavy and technical than Day 1, but I’m getting used to it. Next I will read about queues, async processing and pub-sub stuff — looking forward to it.
