# Day 6 — Google Drive System Design (My Notes)

Today I studied how Google Drive works, and honestly it was way more complex than I thought. We just upload file and forget, but behind the scenes there's whole system working together.

Here's what I understood from it:

---

### Basic Idea

Google Drive is basically a cloud file storage system. User can upload, download, share and manage files from any device.

But question is — how they store so many files? How they handle millions of users uploading files every second? How sharing and sync works?

---

### Main Components

* **Client**: User app (mobile/web) that interact with the system
* **Metadata Server**: stores info about files (like name, type, location, ownership)
* **Block Storage Servers**: actual place where file data is stored in chunks
* **Notification Service**: tells client if someone made changes to shared file

---

### Upload Flow

1. User clicks upload
2. File is splitted into small small chunks (maybe 4MB each)
3. Each chunk is uploaded parallelly to block servers
4. Metadata server saves info about chunk locations, file name, owner, etc
5. If upload breaks in middle, resumable upload happen from last successful chunk

---

### Download Flow

1. Client ask metadata server about the file
2. Server gives chunk locations
3. Client download all chunks and merge into full file

---

### Syncing

Like if u change one file from mobile, same change should show on laptop.
This happen using **versioning** and **notification system**.

* Every time file changes, a new version created
* Notification is pushed to all devices connected to same account
* Device download updated chunks only

---

### Sharing

* Metadata server keeps track of file permissions
* If u share with someone, their ID is added to access list
* So when they try to open file, system check if they have permission

---

### Storage Optimization

* Files are stored in chunks, so same chunk reused if two files have same content
* Deduplication reduce storage cost
* Compression also used

---

### Failure Handling

* Chunks are replicated on multiple servers (like 3 copies)
* If one server crash, other copy used

---

### Extra Stuff

* Google Docs uses their own editor format, not just regular files
* So editing happens on server side, not like regular file

---

So yeah bro, I thought it's just like "upload and done" but it’s crazy how they handle uploading, syncing, versioning, downloading, and sharing — all at scale.

That’s it for Day 6. Next up: maybe S3, GDrive vs Dropbox or something big.
