# README.md for alx-backend-storage/0x01-NoSQL

## Introduction

This project focuses on NoSQL databases, specifically MongoDB, and Python scripting for interacting with the database. The tasks involve creating scripts to perform various operations, such as listing databases, creating a database, inserting documents, querying information, updating documents, and more.

## Project Details

### 0x01. NoSQL

- **Back-end:** NoSQL
- **Database:** MongoDB
- **Authors:** Emmanuel Turlay (Staff Software Engineer at Cruise) and Guillaume (CTO at Holberton school)
- **Weight:** 1
- **Project Duration:** Dec 18, 2023, 6:00 AM, to Dec 20, 2023, 6:00 AM
- **Checker Release:** Dec 18, 2023, 6:00 PM

### Resources

- [NoSQL Databases Explained](#)
- [What is NoSQL?](#)
- [MongoDB with Python Crash Course - Tutorial for Beginners](#)
- [MongoDB Tutorial 2: Insert, Update, Remove, Query](#)
- [Aggregation](#)
- [Introduction to MongoDB and Python](#)
- [mongo Shell Methods](#)
- [The mongo Shell](#)

### Learning Objectives

Upon completing this project, you are expected to:

1. Explain what NoSQL means.
2. Understand the differences between SQL and NoSQL.
3. Comprehend ACID (Atomicity, Consistency, Isolation, Durability).
4. Grasp the concept of document storage.
5. Identify different types of NoSQL databases.
6. Recognize the benefits of using a NoSQL database.
7. Query information from a NoSQL database.
8. Insert, update, and delete information from a NoSQL database.
9. Use MongoDB for various operations.

## Requirements

### MongoDB Command File

- All files interpreted/compiled on Ubuntu 18.04 LTS using MongoDB 4.2.
- All files should end with a new line.
- The first line of all files should be a comment: `// my comment`.
- A README.md file at the root of the project folder is mandatory.
- The length of files will be tested using `wc`.

### Python Scripts

- All files interpreted/compiled on Ubuntu 18.04 LTS using Python3 (version 3.7) and PyMongo (version 3.10).
- All files should end with a new line.
- The first line of all files should be exactly `#!/usr/bin/env python3`.
- A README.md file at the root of the project folder is mandatory.
- Code should use the `pycodestyle` style (version 2.5.\*).
- The length of files will be tested using `wc`.
- All modules and functions should have documentation.
- Code should not be executed when imported (using `if __name__ == "__main__":`).

### More Information

- Install MongoDB 4.2 in Ubuntu 18.04 using the [official installation guide](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/).
- Example installation steps:
  ```bash
  $ wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | apt-key add -
  $ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" > /etc/apt/sources.list.d/mongodb-org-4.2.list
  $ sudo apt-get update
  $ sudo apt-get install -y mongodb-org
  ...
  $ sudo service mongod status
  mongod start/running, process 3627
  $ mongo --version
  MongoDB shell version v4.2.8
  ...
  $ pip3 install pymongo
  $ python3
  >>> import pymongo
  >>> pymongo.__version__
  '3.10.1'
  ```
- Potential issue resolution:
  - If document creation doesn’t work or encounters the error "Data directory /data/db not found, terminating," run:
    ```bash
    $ sudo mkdir -p /data/db
    ```
  - If /etc/init.d/mongod is missing, find an example of the file [here](#).

### Tasks

1. **List all databases**

   - Write a script that lists all databases in MongoDB.

   ```bash
   $ cat 0-list_databases | mongo
   ```

2. **Create a database**

   - Write a script that creates or uses the database `my_db`.

   ```bash
   $ cat 1-use_or_create_database | mongo
   ```

3. **Insert document**

   - Write a script that inserts a document in the collection `school` with the name "Holberton school."

   ```bash
   $ cat 2-insert | mongo my_db
   ```

4. **All documents**

   - Write a script that lists all documents in the collection `school`.

   ```bash
   $ cat 3-all |
   ```

mongo my_db

````

5. **First document**
- Write a script that lists all documents with `name: "Holberton school"` in the collection `school`.

```bash
$ cat 4-match | mongo my_db
````

6. **Update document**

   - Write a script that updates the first document with the name "Holberton school" to "New School" in the collection `school`.

   ```bash
   $ cat 5-update | mongo my_db
   ```

7. **Delete by match**

   - Write a script that deletes all documents with `name: "New School"` in the collection `school`.

   ```bash
   $ cat 6-delete | mongo my_db
   ```

8. **List all documents in Python**

   - Write a Python script that lists all documents in the collection `school`.

   ```bash
   $ python3 8-all.py
   ```

9. **Insert a document in Python**

   - Write a Python script that inserts a new document in the collection `school`.

   ```bash
   $ python3 9-insert_school.py
   ```

10. **Change school topics**

    - Write a Python script that changes all topics of a school document based on the name.

    ```bash
    $ python3 10-update_topics.py
    ```

11. **Where can I learn Python?**

    - Write a Python script that lists all documents in the collection `school` where topics match "Python."

    ```bash
    $ python3 11-schools_by_topic.py
    ```

## Example

```bash
$ cat 0-list_databases | mongo
$ cat 1-use_or_create_database | mongo
$ cat 2-insert | mongo my_db
$ cat 3-all | mongo my_db
$ cat 4-match | mongo my_db
$ cat 5-update | mongo my_db
$ cat 6-delete | mongo my_db
$ python3 8-all.py
$ python3 9-insert_school.py
$ python3 10-update_topics.py
$ python3 11-schools_by_topic.py
```

## How to use MongoDB Aggregations

- Aggregations are a powerful way to manipulate data in MongoDB. They allow you to process data records and return computed results. Below is a basic example:

### Aggregation Pipeline

- Aggregation in MongoDB is a series of data transformation stages. Each stage takes an input and produces an output. The output of one stage serves as the input for the next.

### Example

- In this example, we have a collection `sales` with documents representing sales transactions. Each document has the fields `item` (product name), `quantity` (number of items sold), and `price` (price per item).

#### Data

```json
[
  { "item": "apple", "quantity": 10, "price": 0.5 },
  { "item": "orange", "quantity": 5, "price": 0.75 },
  { "item": "banana", "quantity": 8, "price": 0.4 }
]
```

#### Aggregation

- Suppose we want to calculate the total revenue for each product. We can use the aggregation pipeline to achieve this.

```bash
$ mongo my_db --eval 'db.sales.aggregate([
  {
    $project: {
      item: 1,
      total: { $multiply: ["$quantity", "$price"] }
    }
  },
  {
    $group: {
      _id: "$item",
      totalRevenue: { $sum: "$total" }
    }
  }
])'
```

#### Result

```json
{ "_id" : "banana", "totalRevenue" : 3.2 }
{ "_id" : "orange", "totalRevenue" : 3.75 }
{ "_id" : "apple", "totalRevenue" : 5 }
```

### Breakdown

- The aggregation pipeline consists of two stages:
  1. **$project:** This stage reshapes the documents by including the `item` field and creating a new field `total` calculated as the product of `quantity` and `price`.
  2. **$group:** This stage groups the documents by `item` and calculates the total revenue for each group using the $sum accumulator.

### Applying Aggregations to This Project

- For more advanced operations, you might want to explore MongoDB's aggregation framework. It's a powerful tool for manipulating and analyzing data. To incorporate aggregations into this project, consider adding an optional task or section exploring aggregations in the context of the provided data.

## Advanced Usage

- This project covers basic interactions with MongoDB using Python and the mongo shell. For more advanced use cases, consider exploring features such as indexing, sharding, and replication. These features can enhance the performance, scalability, and reliability of your MongoDB deployment.

## Additional Resources

- [MongoDB Documentation](https://docs.mongodb.com/)
- [PyMongo Documentation](https://pymongo.readthedocs.io/en/stable/)
- [MongoDB University](https://university.mongodb.com/)

```

This README provides an overview of the alx-backend-storage/0x01-NoSQL project. It includes information on the project's objectives, resources, learning objectives, requirements, and tasks. The README also offers examples and guidance on MongoDB aggregations and advanced usage of MongoDB features. It serves as a comprehensive guide for developers working on this project. If you have any questions or need further clarification, feel free to reach out. Happy coding!
```
