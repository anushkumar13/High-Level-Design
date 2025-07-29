# Day 5 — Search Autocomplete System, YouTube System Design (My Notes)

Today was pretty wild bro. Learnt stuff that powers some of the biggest platforms we all use — search boxes and YouTube itself. These things feel simple from outside but damn, the backend logic is crazy.

---

### Search Autocomplete System

You know when you type on Google or Instagram search bar and it already starts showing results before you even finish? That thing is autocomplete system. So many things going on there.

#### Problem

* When user type a few letters, we have to give suggestions **super fast**.

#### Main Parts

* **Data Gathering Service**: Collect search queries typed by users (for trending / history-based suggestions)
* **Query Service**: When user start typing, this service is called to return matching suggestions
* **Storage**: Has all possible search strings, should be fast AF to search

#### Trie Data Structure

* Most used DS here is **Trie**
* Each letter is a node, and full words are paths
* Searching "cha" in Trie will instantly give all words starting with "cha"
* Very fast for prefix matching

#### Scale The Storage

* For billion queries, can’t store in 1 machine
* So Trie can be **sharded** by alphabet
* Like all queries starting with A-D on one server, E-H on another

Also:

* **Ranking logic**: which suggestions show first (popularity, recent, etc)
* **Personalization**: different suggestions for different users (based on their history)
* **Caching** popular prefixes (like "cric", "insta")

Realized even a search bar is like an entire system itself.

---

### YouTube System Design

Bro this one hit different. YouTube is massive. And designing it means thinking about video storage, streaming, user history, DAG, everything.

#### Video Upload Flow

1. User uploads a video
2. Video goes to a **Preprocessor** — checks format, size, etc
3. Then sent to **Video Transcoding Service** — makes multiple formats (240p, 360p, 720p, etc)
4. Store all formats in **Object Storage** (like S3)

#### Video Streaming Flow

* When someone watch a video:

  * Based on their internet, the correct video version is picked
  * That video chunks are streamed using **CDNs** (to reduce buffering)

#### DAG (Directed Acyclic Graph)

* Used to show related videos, watch history
* Each video is a node, edges show "what user watched after this"
* Helps in recommendation engine

#### Other Concepts

* **Metadata DB**: stores title, desc, views, etc
* **Like/Comment Service**: count likes/comments separately, don’t mix with main video logic
* **Recommendation Engine**: uses DAG + ML models
* **Analytics**: count watch time, engagement, etc

Biggest thing — video streaming needs insane optimization. Cuz slow buffering = bad UX = user bounce.

---

That’s all for Day 5. I thought search and video would be easy topics but they were super deep. Especially Trie and DAG — these DS are actually used at scale. Makes me feel closer to what real engineers work on.
