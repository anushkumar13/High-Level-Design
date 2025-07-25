# Day 1 — High-Level Design Fundamentals

These are my personal notes from Day 1 of learning High-Level Design (HLD), based on the book *System Design Interview: An Insider's Guide* by Alex Xu. I'm just trying to get the hang of these basics — not trying to sound smart, just need to actually understand how things work.

---

### Single Server Architecture

I started with the basic setup where literally everything runs on one server — web, backend, and database. Super simple. This works fine for small apps or just testing things out. But obviously, it's not scalable. Once traffic increases, this one server becomes a bottleneck. And if that one machine dies, the whole app goes down. So ya, good for early-stage stuff, not for anything serious.

---

### Choosing the Right Database

Then I tried to wrap my head around databases. If I need things like transactions, proper structure, and strong consistency, relational DBs like MySQL or Postgres make sense. But if the app needs to scale like crazy and schema is not fixed, then NoSQL like MongoDB or DynamoDB is better. Honestly, it really depends on what the app needs. Most big apps probably use a mix of both.

---

### Vertical vs Horizontal Scaling

Vertical scaling is like upgrading your PC — add more RAM, CPU, etc. Super easy but doesn’t scale forever. Horizontal scaling is when you add more machines instead of upgrading just one. It's more scalable, but obviously, also more complicated. You need to balance traffic, manage data between servers, all that.

---

### Load Balancer

Load balancers basically act like traffic police. They sit in front of multiple servers and distribute incoming requests. This makes sure no server gets overwhelmed and improves reliability too. Tools like Nginx or AWS ELB do this. I didn't go deep into how they work, but I get the overall idea.

---

### Database Replication

Okay this part was kinda tricky. The main idea is to have one main DB server (called leader) which handles all the writes, and then you have follower servers that copy that data and help with reads. This helps reduce load and gives backup. But it also creates issues like data lag between leader and followers. Also if the leader crashes, failover setup needs to be solid.

---

### Caching and Cache Tier

Caching is something that helps a lot with speed. Like instead of fetching data from DB every time, store it in a fast memory layer like Redis or Memcached. Cache tier is like a middle layer between the app and DB. But ya, caching is not as easy as it sounds. Cache miss, stale data, and what to cache or when to remove it — all these need to be figured out carefully.

---

### CDN (Content Delivery Network)

CDNs are for speeding up delivery of static content like images or CSS files. These files are stored on servers around the world, so users get data from the nearest one. This reduces latency and reduces load on the main app server. Seems super useful for apps with lots of users or media content.

---

### Stateless Web Tier

In stateless architecture, servers don’t remember anything between requests. Every request is treated like a fresh one. All session data is stored outside — maybe in Redis or sent with the request. This makes it very easy to scale horizontally since any server can take any request.

---

### Stateful Architecture

In stateful setup, server keeps user data/session info locally. So now if I log in to one server, I need to keep going back to the same one for stuff to work properly. This makes scaling and reliability more painful, honestly. Not ideal unless there's a really good reason.

---

### Final Thoughts

This first day was mostly about understanding how systems grow and where the bottlenecks start showing up. Every concept like cache, replication, or load balancing — it has its own pros and cons. I’m still trying to figure out when to use what, but at least now I know these building blocks.

Next: gonna dive into **queues, async stuff, and message brokers** — sounds interesting tbh.
