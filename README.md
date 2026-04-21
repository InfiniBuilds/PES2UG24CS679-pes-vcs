# PES-VCS: A Mini Version Control System

## 👤 Student Details

**Name:** Shiva Swaroop D S 
**Course:** B.Tech CSE
**Lab:** Operating Systems
**SRN:** PES2UG24CS679

---

# 📌 Overview

This project implements a **simplified Version Control System inspired by Git**.
The system called **PES-VCS** demonstrates how modern version control systems manage file changes efficiently using **content-addressable storage and commit history tracking**.

The system supports core version control operations such as:

* Repository initialization
* File staging
* Snapshot creation using trees
* Commit creation with history tracking
* Content-addressable object storage using SHA-256

The implementation gives insight into **how Git works internally**.

---

# ⚙️ System Architecture

The system consists of four major components.

## 1️⃣ Object Store (`object.c`)

The object store is responsible for storing all repository data including:

* Blob objects (file contents)
* Tree objects (directory snapshots)
* Commit objects

Each object is stored using **SHA-256 hashing**.

### Object Format

```
<type> <size>\0<data>
```

### Advantages

* Data deduplication
* Data integrity verification
* Efficient storage

---

## 2️⃣ Index / Staging Area (`index.c`)

The index acts as the **staging area** where files are prepared before committing.

It stores metadata including:

* File path
* Hash of the file
* File mode
* Timestamp
* File size

This component behaves similar to:

```
git add
```

---

## 3️⃣ Tree Structure (`tree.c`)

Tree objects represent **snapshots of the directory structure**.

The index is converted into a tree object during commit.

Each tree entry stores:

```
<mode> <filename> <hash>
```

This allows the system to recreate the directory structure at any commit.

---

## 4️⃣ Commit System (`commit.c`)

The commit system creates commits containing:

* Tree reference
* Parent commit reference
* Author information
* Timestamp
* Commit message

### Commit Structure

```
tree <tree_hash>
parent <parent_hash>
author <author_name>
message <commit_message>
```

Commits are linked together forming a **commit history chain**.

---

# 🔄 Workflow

The complete workflow of PES-VCS:

```
pes init → pes add → pes commit → pes log
```

### Repository Initialization

```
pes init
```

Creates the `.pes` repository structure.

---

### Staging Files

```
pes add <filename>
```

Adds files to the staging area.

---

### Creating Commits

```
pes commit "<message>"
```

Creates a commit snapshot.

---

### Viewing History

```
pes log
```

Displays commit history.

---

# 📸 Screenshots

## Phase 1 – Object Storage

Object creation verified using:

```
./test_objects
```

All object tests passed successfully.

---

## Phase 2 – Tree Creation

Tree structure verified using:

```
./test_tree
```

Raw tree structure inspected using:

```
xxd .pes/objects/<hash>
```

---

## Phase 3 – Staging System

Files successfully staged using:

```
pes add
```

Index file contains metadata of staged files.

---

## Phase 4 – Commit History

Commits successfully created and history verified using:

```
pes log
```

---

## Final Integration Test

Full workflow verified using:

```
./test_sequence.sh
```

Output:

```
All integration tests completed
```

---

# 🧠 Key Concepts Learned

This project helped understand important operating system and version control concepts including:

* Content-addressable storage
* SHA-256 hashing
* Version tracking
* Directory snapshots using trees
* Commit history linked structure
* Separation of working directory and staging area

---

# 📊 Analysis Questions

## Branching

### How are branches implemented?

Branches are implemented as **references pointing to commits**.

Example:

```
refs/heads/main → <commit_hash>
```

### How does branching work internally?

When a branch is created:

* A new reference file is created
* It points to an existing commit
* Both branches share history until divergence

### Conceptual Merge Process

Merging works by:

1. Finding the common ancestor
2. Combining changes
3. Creating a commit with multiple parents

---

## Garbage Collection

### What are unreachable objects?

Objects not referenced by any commit.

### Garbage Collection Process

1. Traverse objects reachable from `HEAD`
2. Identify unreferenced objects
3. Delete them to free storage

---

# ✅ Conclusion

This project successfully implements a **mini version control system** demonstrating:

* Efficient storage using hashing
* Snapshot-based version tracking
* Tree-structured filesystem representation
* Commit history traversal

It provides a practical understanding of **how Git manages data internally**.
