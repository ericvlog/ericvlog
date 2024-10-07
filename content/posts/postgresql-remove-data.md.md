---
title: "Postgresql Remove Data Tutorial"
subtitle: ""
date: 2024-10-06T16:47:33+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "postgresql bigining guide"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- postgresql
- database
categories:
- computer

hiddenFromHomePage: false
hiddenFromSearch: false

summary: ""
resources:
- name: featured-image
  src: featured-image.jpg
- name: featured-image-preview
  src: featured-image-preview.jpg

toc:
  enable: true
math:
  enable: false
lightgallery: false
seo:
  images: []

# See details front matter: /theme-documentation-content/#front-matter
---
# 🗂️ Identifying Your TV Show Data in PostgreSQL

To locate the table where your TV show data is stored in PostgreSQL, follow these steps:

## Step 1: List All Tables in the Database 🗃️

1. **Access the PostgreSQL Prompt**:

   ```bash
   docker exec -it riven-db psql -U postgres
   ```

2. **Connect to Your Database**:

   ```sql
   \c riven
   ```

3. **List All Tables**:

   Run the following command to view all tables in the current database:

   ```sql
   \dt
   ```

   This will display a list of tables along with their schema, name, type, and owner.

## Step 2: Examine Table Structure 🔍

Once you have a list of tables, examine the structure of each to find the relevant columns for your TV show data.

1. **Describe a Table**:

   For example, to see the structure of the `tvshows` table:

   ```sql
   \d tvshows
   ```

   This command shows you the columns, their types, and any constraints (like primary keys).

## Step 3: Look for Relevant Columns 🔑

When describing a table, look for columns that might contain your TV show data. Common names include:

- `title`
- `name`
- `show_name`
- `description`
- `genre`

## Example Workflow 🛠️

Here’s how the entire process might look:

```bash
# Access the PostgreSQL prompt
docker exec -it riven-db psql -U postgres

# Connect to your database
\c riven

# List all tables
\dt

# Describe the 'tvshows' table to see its structure
\d tvshows
```

## Conclusion 🏁

By following these steps, you can identify which table holds your TV show data and what columns are available for querying or deleting. If you find a table that seems relevant but you're unsure, feel free to share its structure, and I can help you interpret it!

---

## Troubleshooting Command Errors ⚠️

If you encounter issues with the command you entered, here’s how to resolve them:

### Step 1: Ensure You Are in the Correct Environment ✅

Make sure you are in the PostgreSQL command prompt (`psql`). A prompt like `riven=#` indicates you’re in the right place.

### Step 2: Execute the Query Again 🔄

Try running the query again:

```sql
SELECT _id FROM "MediaItem" WHERE title ILIKE '%Isekai%';
```

### Step 3: Verify the Output 📊

If the command executes successfully, it should return the `_id` of any media items that match the title. If there are no results, it will simply return an empty set.

---

## Checking for References to `_id = 3944` 🔗

You’ve successfully retrieved a list of tables referencing the `MediaItem` table. Here’s a summary:

| Table Name                | Column Name          |
|---------------------------|----------------------|
| Movie                     | _id                  |
| Show                      | _id                  |
| StreamBlacklistRelation    | media_item_id       |
| StreamRelation            | parent_id            |
| Subtitle                  | parent_id            |
| Season                    | _id                  |
| Episode                   | _id                  |

### Step 1: Query Each Table for `_id = 3944` 🔍

Run `SELECT` queries on each of these tables:

1. **Movie**:

   ```sql
   SELECT * FROM "Movie" WHERE _id = 3944;
   ```

2. **Show**:

   ```sql
   SELECT * FROM "Show" WHERE _id = 3944;
   ```

3. **StreamBlacklistRelation**:

   ```sql
   SELECT * FROM "StreamBlacklistRelation" WHERE media_item_id = 3944;
   ```

4. **StreamRelation**:

   ```sql
   SELECT * FROM "StreamRelation" WHERE parent_id = 3944;
   ```

5. **Subtitle**:

   ```sql
   SELECT * FROM "Subtitle" WHERE parent_id = 3944;
   ```

6. **Season**:

   ```sql
   SELECT * FROM "Season" WHERE _id = 3944;
   ```

7. **Episode**:

   ```sql
   SELECT * FROM "Episode" WHERE _id = 3944;
   ```

### Step 2: Execute Queries 🏃‍♂️

Run these queries one by one in your PostgreSQL prompt to check for any records associated with `_id = 3944`.

### Step 3: Review Results 📋

- Check each output to see if there are related records.
- If a table returns results, it indicates associations with the `MediaItem` having `_id = 3944`.

---

## Deleting Shows and Related Data 🗑️

If you need to delete entries, follow these steps:

### Step 1: Check Related Seasons 🔎

First, identify which seasons reference this show:

```sql
SELECT * FROM "Season" WHERE parent_id = 3944;
```

### Step 2: Delete Related Seasons ❌

Once identified, delete those records:

```sql
DELETE FROM "Season" WHERE parent_id = 3944;
```

### Step 3: Delete the Show 🎬

After deleting related seasons, you can delete the show itself:

```sql
DELETE FROM "Show" WHERE _id = 3944;
```

### Complete Workflow 🔄

1. **Check for Related Seasons**:

   ```sql
   SELECT * FROM "Season" WHERE parent_id = 3944;
   ```

2. **Delete Related Seasons** (if found):

   ```sql
   DELETE FROM "Season" WHERE parent_id = 3944;
   ```

3. **Delete the Show**:

   ```sql
   DELETE FROM "Show" WHERE _id = 3944;
   ```

### Important Considerations ⚠️

- **Backup**: Always ensure you have a backup before performing delete operations.
- **Cascade Deletion**: Consider setting the foreign key constraint in the `Season` table to `ON DELETE CASCADE` for future deletions.

If you have any further questions or need assistance with the queries, feel free to ask! 😊

--- 

This guide provides a comprehensive overview of how to manage your TV show data in PostgreSQL, complete with visual cues and easy-to-follow instructions. Let me know if you need any more adjustments!
<!--more-->
