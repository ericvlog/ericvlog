---
title: "A Comprehensive Guide to SQLite: Installation, Checking, and Deleting Records"
subtitle: ""
date: 2024-10-05T22:22:29+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "Hello welcome to my blogs"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- sqlite
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
```markdown
# 📚 SQLite Guide: Installation, Checking, and Deleting Records

## 1. Install SQLite 🛠️

To get started with SQLite, you'll need to install it on your system. Follow these steps:

### For Debian/Ubuntu:

1. **Open Terminal** 🖥️
2. **Update Package List**:
   ```bash
   sudo apt update
   ```
3. **Install SQLite**:
   ```bash
   sudo apt install sqlite3
   ```
4. **Verify Installation** ✅:
   ```bash
   sqlite3 --version
   ```

## 2. Open Your Database 📂

Once SQLite is installed, you can open your database file:

1. **Navigate to the Directory**:
   ```bash
   cd ~/appdata/arrr/jellyseerr/db
   ```
2. **Open SQLite with Your Database**:
   ```bash
   sqlite3 db.sqlite3
   ```

## 3. Check for Existing Records 🔍

Before deleting any records, it's good practice to check if they exist.

### Run the Following Query:
```sql
SELECT media_request.id 
FROM media 
LEFT JOIN media_request ON media.id = media_request.mediaId 
WHERE media.status = 5;
```

### Interpreting the Results:
- The output will show IDs of `media_request` records that are linked to media entries with a status of `5`.
- Example Output:
  ```
  1
  3
  5
  7
  8
  9
  10
  11
  14
  15
  28
  ```

## 4. Delete Records 🗑️

If you confirmed that records exist, you can proceed to delete them.

### Run the Following Query:
```sql
DELETE FROM media_request 
WHERE id IN (
    SELECT media_request.id 
    FROM media 
    LEFT JOIN media_request ON media.id = media_request.mediaId 
    WHERE media.status = 5
);
```

## 5. Confirm Deletion ✔️

After deleting, it’s important to verify that the records have been removed.

### Run the Check Again:
```sql
SELECT media_request.id 
FROM media 
LEFT JOIN media_request ON media.id = media_request.mediaId 
WHERE media.status = 5;
```
- If the result set is empty, the deletion was successful! 🎉

## Conclusion 🎈

You have successfully installed SQLite, checked for existing records, and deleted them as needed. If you have more questions or need further assistance, feel free to ask!
```
<!--more-->
