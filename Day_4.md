# Day 4 — Notification System, News Feed, Chat System (My Notes)

Today felt so real bro — like actually making a full social media app from scratch. Notifications, feeds, chat — these are things we use every day but never really think about how they work. All of this I learned from Alex Xu’s System Design book (Vol 1). I’ll just write down what I understood in my own way:

---

### Notification System

Notifications seem easy like “someone liked ur post” but behind that simple thing, there’s a lot happening.

* When user do some action (like, comment, follow)
* That thing is converted into an **event**
* A background service see that event and creates the notification
* Then it’s **stored** in DB or cache and **delivered** (push, email, in-app)

Main parts:

* **Event Producer**: Where action starts (like user followed you)
* **Message Queue** (like Kafka): helps to separate diff services
* **Notification Service**: makes messages like “Anush liked ur post”
* **Delivery**: via push notif, mail, or in-app badge

Also:

* **Unread count** (for that red dot badge)
* **Deduplication** (so same notif don’t repeat again)
* **Batching** (like “5 people liked ur post” instead of 5 notifs)
* **Retries** if delivery fails

Biggest thing I learned — **notifications should be async**. Don’t make user wait just bcz notif is being created. Queue is must.

---

### News Feed System

Feed is like your main timeline — posts from people u follow. There’s 2 ways to do it:

#### 1. Pull-based (Fan-out on read)

* Don’t store any feed
* When user opens app, fetch posts from all they follow
* Saves space but slow if user follow many ppl

#### 2. Push-based (Fan-out on write)

* When someone posts, add it into feed of all followers
* Super fast for reading, but heavy for writing (imagine celeb posts)

So real systems mix both:

* **Push** for normal users
* **Pull** for celebs (bcz they have too many followers)

Also feeds use:

* **Ranking** (ML, likes, freshness, etc)
* **Feed service** to create + store feed
* **Redis** for fast feed storage, DB for older posts
* **Filtering** + **deduplication** (no duplicates)

Main thing: feed isn’t just timeline. It’s optimized, ranked, filtered.

---

### Chat System

Chat looks easy. Just send a msg right? But making it real-time and for millions of users — that’s hard af.

Main question: **how to send + get msgs instantly?**

There are 3 ways:

#### 1. Short Polling

* Client ask server every few seconds: “new msg?”
* Very wasteful and laggy

#### 2. Long Polling

* Client sends request, server waits until msg comes, then replies
* Better than short polling, feels real-time
* But server needs to handle so many hanging connections

#### 3. WebSockets

* Makes permanent connection b/w client and server
* Super fast, real-time — like WhatsApp
* But hard to scale (too many connections open)

Also chat needs:

* **Message order** (msgs should be in correct sequence)
* **Delivery guarantee** (don’t lose msgs)
* **History storage** (DB + maybe cache)
* **Presence** (who’s online, who’s typing)

So yeah, chat isn’t just sending msgs. It’s giving users a smooth, fast, reliable experience.

---

That’s it for Day 4. Tbh, this is the day it felt like I’m actually building real-world stuff. All these features — notifs, feed, chat — I use every day. Now I kinda get how they’re made. Next up: **Search and CDNs.** Let’s go!
