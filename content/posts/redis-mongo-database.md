---
title: "How to Drop Databases in Redis, MongoDB, and PostgreSQL"
subtitle: ""
date: 2024-06-04T16:57:31+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "How to Drop Databases in Redis, MongoDB, and PostgreSQL with list out all user and database"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- redis
- postgress
- mongo
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

<!--more-->

# 📚 How to Check and Delete Redis and MongoDB Data Using Docker Compose

This tutorial will guide you through the process of checking if a key exists in a Redis database, deleting it if necessary, and also how to delete data from MongoDB, all using Docker Compose commands.

## 🛠️ Prerequisites

- Docker and Docker Compose installed
- Redis and MongoDB services defined in your `docker-compose.yml` file

## 🚀 Using Docker Compose

### 🔴 Interacting with Redis

#### 🔍 Check if a Key Exists

1. **Ensure your Redis container is running** by using Docker Compose:
    ```sh
    docker-compose up -d
    ```

2. **Connect to the Redis container** using the following command:
    ```sh
    docker-compose exec redis redis-cli
    ```
    Replace `redis` with the name of your Redis service defined in your `docker-compose.yml` file.

3. **Check if the key exists** using the `EXISTS` command:
    ```sh
    EXISTS your-key-name
    ```
    Replace `your-key-name` with the actual key you want to check. The command will return `1` if the key exists and `0` if it does not.

#### 🗝️ Check All Keys

1. **List all keys** using the `KEYS` command:
    ```sh
    KEYS *
    ```
    This command will return a list of all keys stored in the currently selected database.

#### 📝 Get Key Information

1. **Get detailed information about a specific key** using the `TYPE` and `TTL` commands:
    ```sh
    TYPE your-key-name
    TTL your-key-name
    ```
    - `TYPE` returns the data type of the key (e.g., string, list, set).
    - `TTL` returns the remaining time to live of a key that has a timeout, or `-1` if the key does not have a timeout.

#### 🗑️ Delete a Specific Key

1. **Delete the key** using the `DEL` command:
    ```sh
    DEL your-key-name
    ```

#### 🧹 Delete All Keys

1. **Delete all keys** in the currently selected database using the `FLUSHDB` command:
    ```sh
    FLUSHDB
    ```
    This will remove all keys from the currently selected database.

2. **Delete all keys in all databases** using the `FLUSHALL` command:
    ```sh
    FLUSHALL
    ```
    This will remove all keys from all databases.

### 🟢 Interacting with MongoDB

#### 🔎 Determine the Actual Name of Your Database

1. **Ensure your MongoDB container is running** by using Docker Compose:
    ```sh
    docker-compose up -d
    ```

2. **Connect to the MongoDB container** using the following command:
    ```sh
    docker-compose exec mongo mongo
    ```
    Replace `mongo` with the name of your MongoDB service defined in your `docker-compose.yml` file.

3. **List all databases**:
    ```sh
    show dbs
    ```
    This command will display a list of all databases on your MongoDB server along with their sizes.

    **Example Output**:
    ```sh
    > show dbs
    admin       0.000GB
    config      0.000GB
    local       0.000GB
    exampleDB   0.001GB
    ```

    In this example, `exampleDB` is the name of one of the databases.

#### 📂 Select and Work with a Database

Once you know the name of your database, you can select it using the `use` command:

1. **Select the database**:
    ```sh
    use exampleDB
    ```
    Replace `exampleDB` with the actual name of your database.

#### 📋 List Collections in a Database

After selecting the database, you can list all collections within it:

1. **List all collections**:
    ```sh
    show collections
    ```
    This will display all collections within the selected database.

    **Example Output**:
    ```sh
    users
    orders
    products
    ```

#### 🗑️ Delete All Collections or Specific Collections

##### 🗑️ Delete All Collections

1. **Drop the entire database** (this will delete all collections and documents within it):
    ```sh
    db.dropDatabase()
    ```
    This command will remove the entire database, including all collections and their documents.

##### 🗑️ Delete Specific Collections

1. **Drop a specific collection**:
    ```sh
    db.your-collection-name.drop()
    ```
    Replace `your-colle...


# Guide to Manage PostgreSQL Databases in Docker 🐘

This guide will help you list PostgreSQL users and databases, find your database name, and drop a PostgreSQL database within a Docker container.

## Listing PostgreSQL Users and Databases

### Step 1: Access the PostgreSQL Container

To access your running PostgreSQL container, use the following command. Replace `your_container_name` with the actual name or ID of your container.

```bash
docker exec -it your_container_name bash
```

### Step 2: Connect to PostgreSQL

Once inside the container, connect to the PostgreSQL database using the `psql` command. Replace `username` with your PostgreSQL username (often `postgres`).

```bash
psql -U username
```

### Step 3: List Users

To list all users, execute:

```sql
\du
```

### Step 4: List All Databases

To list all databases, use:

```sql
\l
```

### Step 5: Find Your Database Name

If you're not sure about your database name, you can check the list of databases using the command above (`\l`). Look for the name in the output.

### Step 6: Exit

To exit the `psql` prompt, type:

```sql
\q
```

Then, exit the container:

```bash
exit
```

---

## Dropping a PostgreSQL Database 💔

### Step 1: Access the PostgreSQL Container

If you are not already inside the container, access it using the same command as before:

```bash
docker exec -it your_container_name bash
```

### Step 2: Connect to PostgreSQL

Connect to PostgreSQL using:

```bash
psql -U username
```

### Step 3: Drop the Database

After connecting, you can drop the database by executing the following command. Replace `zilean` with the name of the database you want to drop.

```sql
DROP DATABASE zilean;
```

### Step 4: Exit

To exit the `psql` prompt, type:

```sql
\q
```

Then, exit the container:

```bash
exit
```

---

## Summary 🌟

This guide provided instructions on how to list PostgreSQL users and databases, find your database name, and drop a PostgreSQL database within a Docker container. Ensure you have the necessary permissions and always back up your data before performing destructive operations.