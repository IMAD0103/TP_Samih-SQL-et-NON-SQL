# Sakila Database Migration Project

This project facilitates the migration of selected tables from the Sakila PostgreSQL database to NoSQL databases (Redis and MongoDB), implemented in Node.js.

## Project Overview

The objective is to demonstrate data migration from a relational database (Sakila on PostgreSQL) to different NoSQL paradigms:
* **Key-Value Store (Redis):** For `country` and `city` tables.
* **Document-Oriented Database (MongoDB):** For `film`, `actor`, `category`, and `language` tables.

The migration process considers existing foreign key relationships and adapts the data structure for optimal NoSQL representation.

## Features

* Connects to PostgreSQL, Redis, and MongoDB.
* Migrates `country` and `city` data to Redis as key-value pairs (using Redis Hashes).
* Migrates `actor` data to MongoDB as individual documents.
* Migrates `language` and `category` data internally for embedding.
* Migrates `film` data to MongoDB, embedding `language` and `category` details, and referencing `actor` IDs.
* Uses environment variables for secure configuration.
* Includes optional functionality to clear target NoSQL databases before migration.

## Prerequisites

Before running the migration, ensure you have the following installed and running:

* **Node.js:** v14 or higher (LTS recommended)
* **PostgreSQL:** Database server
* **Redis:** Redis server
* **MongoDB:** MongoDB server

## Setup Instructions

1.  **Clone the Repository (or create your project):**
    ```bash
    git clone [https://github.com/your-username/sakila-migration.git](https://github.com/your-username/sakila-migration.git)
    cd sakila-migration
    ```

2.  **Install Node.js Dependencies:**
    ```bash
    npm install
    ```

3.  **Prepare PostgreSQL Database:**
    * Ensure your PostgreSQL server is running.
    * Create a new database (e.g., `sakila_pg`).
    * Execute the provided Sakila schema and data SQL files to populate the database:
        ```bash
        psql -U your_pg_user -d sakila_pg -f path/to/sakila-schema.sql
        psql -U your_pg_user -d sakila_pg -f path/to/sakila-data.sql
        ```
        (Replace `your_pg_user` and `path/to/` with your actual values)

4.  **Start Redis and MongoDB Servers:**
    * Ensure both Redis and MongoDB servers are running on their default ports or configured ports.

5.  **Environment Configuration (`.env` file):**
    * Create a file named `.env` in the root directory of this project.
    * Copy the contents from `.env.example` into your new `.env` file.
    * Fill in your actual database credentials and host information.
    * **IMPORTANT:** Do NOT commit your `.env` file to version control. Add `.env` to your `.gitignore` file.

    Example `.env` (with placeholder values):
    ```
    # PostgreSQL Configuration
    PG_USER=postgres
    PG_PASSWORD=mysecretpassword
    PG_HOST=localhost
    PG_PORT=5432
    PG_DATABASE=sakila_pg

    # Redis Configuration
    REDIS_HOST=localhost
    REDIS_PORT=6379

    # MongoDB Configuration
    MONGO_URI=mongodb://localhost:27017
    MONGO_DB_NAME=sakila_nosql

    # Optional: Set to 'true' to clear data in target NoSQL databases before migration
    # Set to 'false' if you want to append data or manage clearing manually.
    CLEAR_NOSQL_DATA=false
    ```

## How to Run the Migration

After completing all setup steps, run the migration script:

```bash
node migration.js
```