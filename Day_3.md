# Day 3 — Unique ID Generator, URL Shortener, Web Crawler (My Notes)

Just writing down what I studied today in HLD. **Day 3 was honestly super interesting and kinda fun too**, because I finally got a glimpse of how real systems like Twitter, Bitly, and search engines actually work. I explored three core ideas today: **Unique ID Generator**, **URL Shortener**, and **Web Crawler**. All of them felt super practical, like something big companies use daily. Was a bit confused at some points, but overall I really enjoyed it.

---

### Unique ID Generator (Twitter Snowflake)

I started with trying to understand how a **Unique ID Generator** works in distributed systems. Like you can't just have one counter in a database 'cause that'll become a bottleneck and slow things down.

Then I saw how Twitter uses something called **Snowflake** and it’s actually pretty clever. It creates a **64-bit unique ID** that combines:

* **41 bits** → timestamp in milliseconds
* **10 bits** → machine ID (so different servers can make IDs without clashing)
* **12 bits** → sequence number (if multiple IDs created in same ms)
* **1 bit** → not used (just kept 0)

This way:

* All IDs are **unique**
* They are **time-ordered**, which is useful
* No need for central DB or coordination

I found it cool how every bit had a proper role. Like time makes sure things stay in order, machine ID avoids clashes, and the sequence handles burst requests. Super clean and efficient.

---

### URL Shortener

After that, I moved to **URL Shortener** design. Like Bitly. It looks simple when you use it, but actually there’s a lot to think about.

Goal is simple: take a **long URL**, convert it into a short key, store it, and later when someone visits that short key, redirect them to the original URL.

But the tricky part is:

* Making sure there’s **no key collisions**
* The system is **fast** and doesn’t slow down
* It should be able to **handle millions of users**

The best solution I found was using **Base62 encoding** of an auto-incrementing ID (means using characters from a-z, A-Z, 0-9). That way, you get short and nice looking keys.

So the flow is:

* User submits URL → generate short key → save key and URL in DB
* Later, user visits short URL → fetch original from DB → redirect

To make it faster, we can use **Redis** caching so we don’t keep hitting the DB again and again. Some people also use hashing or UUIDs, but they’re either long or can cause key conflicts.

---

### Web Crawler (HTML Downloader)

Finally I learnt how a basic **Web Crawler** works. It’s just a program that visits websites, downloads HTML content, finds more links inside, and keeps going.

How it works:

1. Start with some **seed URLs**
2. Download HTML pages
3. Extract links from those pages
4. Add new links to the queue
5. Repeat the process

Challenges I found were:

* Avoid visiting **same URL again**
* Respect **robots.txt** so you don’t crawl restricted pages
* Use **rate limiting** so you don’t spam the servers

Main parts of crawler:

* **URL Frontier**: list of URLs to crawl
* **Downloader**: gets the page
* **Parser**: finds new links in the page
* **Duplicate Checker**: makes sure you don’t visit same page again
* **Storage**: for saving HTML or useful info

If you want to scale this thing, then make the queue distributed and run multiple downloaders at the same time.

---

So yeah, **Day 3 was actually super productive**. Felt like I got a real taste of how distributed systems work in real life. Snowflake, Base62, and web crawling — all super useful ideas. Let’s see what tomorrow brings. Next up: gonna read about **queues, pub-sub, and CDN design**.
