# Hands-on Lab: Introduction to MongoDB Atlas
### An alternative to the AWS DynamoDB lab

---

## Overview

MongoDB Atlas is a fully managed NoSQL database service in the cloud. Like Amazon DynamoDB, it handles storage scaling, replication, and infrastructure automatically — you define your data structure and Atlas takes care of the rest.

In this lab you will work with **collections** and **documents** — the MongoDB equivalents of DynamoDB tables and items. By the end you will have created collections, added and edited data, built indexes, and run queries — all through the Atlas browser interface.

---

## Concept Reference

Before starting, use this table to map DynamoDB concepts to MongoDB equivalents:

| DynamoDB | MongoDB | Notes |
|---|---|---|
| Table | Collection | A group of related documents |
| Item | Document | A single JSON record |
| Partition Key | Indexed field / `_id` | The primary identifier for a document |
| Local Secondary Index | Single-field index | Index on one additional field |
| Global Secondary Index | Compound index | Index spanning multiple fields |
| Query | Find with filter | Retrieving documents by field value |

---

## Learning Objectives

By the end of this lab you will be able to:

- Create and configure MongoDB collections
- Add and edit documents in a collection
- Create single-field and compound indexes
- Query a collection using filters
- Delete a collection

---

## Prerequisites

- Basic understanding of databases (tables, rows, fields)
- A browser — no software installation required

---

## Estimated Time

45–60 minutes

---

&nbsp;

## Step 1 — Create a MongoDB Atlas Account and Cluster

1. Go to [https://www.mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register)
2. Sign up for a free account using your email address
3. When prompted to create a cluster, select:
   - **Free tier (M0)**
   - Any cloud provider (AWS, Google Cloud, or Azure — it does not matter for this lab)
   - A region close to you
4. Click **Create Cluster** and wait 1–3 minutes for it to provision
5. When prompted to set up security:
   - Create a **database user** with a username and password — make a note of these
   - Under **Network Access**, click **Add IP Address** → select **Allow access from anywhere** (for lab purposes)

> ✅ **Checkpoint:** You should see your cluster listed as **Active** in the Atlas dashboard before continuing.

---

## Step 2 — Create a Collection with a Primary Field

This is the equivalent of creating a DynamoDB table with a partition key.

1. In the Atlas dashboard, click **Browse Collections**
2. Click **Add My Own Data**
3. Enter the following:
   - **Database name:** `MusicLibrary`
   - **Collection name:** `Songs`
4. Click **Create**

You now have an empty collection. In this lab, `Artist` will act as our primary identifying field — the equivalent of a DynamoDB partition key.

> ✅ **Checkpoint:** You should see `MusicLibrary > Songs` listed in the left-hand panel.

---

## Step 3 — Create a Second Collection with Indexes

This step covers the equivalent of creating a DynamoDB table with local and global secondary indexes.

### 3a — Create a second collection

1. In the left panel, click the **+** icon next to `MusicLibrary`
2. Enter the collection name: `Albums`
3. Click **Create Collection**

### 3b — Create a single-field index (Local Secondary Index equivalent)

1. Select the `Albums` collection
2. Click the **Indexes** tab
3. Click **Create Index**
4. Enter the following definition:
   ```json
   { "Artist": 1 }
   ```
5. Leave all other options as default and click **Create Index**

This index allows fast lookups by `Artist` — equivalent to a local secondary index in DynamoDB.

### 3c — Create a compound index (Global Secondary Index equivalent)

1. Click **Create Index** again
2. Enter the following definition:
   ```json
   { "Artist": 1, "Year": 1 }
   ```
3. Click **Create Index**

This compound index supports queries that span multiple fields — the equivalent of a DynamoDB global secondary index.

> ✅ **Checkpoint:** The Indexes tab for `Albums` should show two custom indexes in addition to the default `_id` index.

---

## Step 4 — Insert Documents into a Collection

This is the equivalent of inserting items into a DynamoDB table.

1. Select the `Songs` collection
2. Click **Insert Document**
3. Switch to the **{}** (JSON view) and replace the default content with:

```json
{
  "Artist": "Radiohead",
  "SongTitle": "Karma Police",
  "Album": "OK Computer",
  "Year": 1997,
  "Genre": "Alternative"
}
```

4. Click **Insert**
5. Repeat with these two additional documents:

```json
{
  "Artist": "Radiohead",
  "SongTitle": "Creep",
  "Album": "Pablo Honey",
  "Year": 1992,
  "Genre": "Alternative"
}
```

```json
{
  "Artist": "Portishead",
  "SongTitle": "Glory Box",
  "Album": "Dummy",
  "Year": 1994,
  "Genre": "Trip-hop"
}
```

> ✅ **Checkpoint:** The `Songs` collection should show 3 documents.

---

## Step 5 — Edit a Document

This is the equivalent of editing an item in a DynamoDB table.

1. In the `Songs` collection, find the document for **Creep**
2. Click the **pencil icon** to edit it
3. Change the `Genre` field value from `"Alternative"` to `"Grunge"`
4. Click **Update**

> ✅ **Checkpoint:** The Creep document should now show `"Genre": "Grunge"`.

---

## Step 6 — Query a Collection

This is the equivalent of querying a DynamoDB table.

### 6a — Simple query (by partition key equivalent)

1. In the `Songs` collection, locate the **Filter** bar at the top
2. Enter the following filter and click **Apply**:

```json
{ "Artist": "Radiohead" }
```

You should see only the two Radiohead songs returned.

### 6b — Query with a condition

Clear the filter and try this:

```json
{ "Artist": "Radiohead", "Year": { "$lt": 1995 } }
```

This returns Radiohead songs released before 1995. The `$lt` operator means "less than".

### 6c — Query by a different field

```json
{ "Genre": "Trip-hop" }
```

This returns all songs in the Trip-hop genre, regardless of artist.

> ✅ **Checkpoint:** Each query should return the expected subset of documents.

---

## Common MongoDB Query Operators

| Operator | Meaning | Example |
|---|---|---|
| `$eq` | Equals | `{ "Year": { "$eq": 1997 } }` |
| `$lt` | Less than | `{ "Year": { "$lt": 2000 } }` |
| `$gt` | Greater than | `{ "Year": { "$gt": 1990 } }` |
| `$in` | Matches any value in a list | `{ "Genre": { "$in": ["Alternative", "Trip-hop"] } }` |

---

## Step 7 — Delete a Collection

This is the equivalent of deleting a DynamoDB table.

1. In the left panel, hover over the `Songs` collection
2. Click the **bin icon** that appears
3. Type `Songs` to confirm deletion
4. Click **Drop Collection**

> ✅ **Checkpoint:** The `Songs` collection should no longer appear in the left panel.

---

## Lab Complete

You have successfully:

- Created a MongoDB Atlas cluster and database
- Created collections with indexes (single-field and compound)
- Inserted, edited, and queried documents
- Deleted a collection

---

## How This Maps Back to AWS

When you return to working with DynamoDB, the concepts transfer directly:

- **Collections → Tables**
- **Documents → Items**
- **Compound indexes → Global Secondary Indexes**
- **Filter queries → DynamoDB Query and Scan operations**

The main difference is that DynamoDB requires you to define your partition key and indexes upfront, while MongoDB is more flexible about schema. Both are managed NoSQL services designed for scale.

---

*Lab adapted from the QA DynamoDB lab as an alternative exercise where AWS access is unavailable.*
