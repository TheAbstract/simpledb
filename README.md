# SimpleDB

Often I need work with data in something more structured than csv's but without the complexity of a full-featured RDMS; that's what this is for.  

SimpleDB is a lightweight (zero external dependencies), thread-safe database for Python that simplifies interacting with SQLite databases.
It's built to be easy to use while providing robust features to handle concurrent access, data serialisation, and automatic transaction management.

## Features

- **Thread Safety**: Ensures safe concurrent access to the database.
- **Automatic Transaction Management**: Automatically commits changes every 60 seconds or after 16,384 operations, whichever comes first. This prevents long-term database locks.

## Installation

To get started with SimpleDB, you need to have Python (requires Python 3.6 or higher) installed on your system. You can then install SimpleDB using pip:

```
pip install git+https://github.com/TheAbstract/simpledb.git
```

Since SimpleDB has no external dependencies and is a single file you can also just copy it into you project.

## Usage

### Simple Example

Here's a basic example of how to use SimpleDB to store and retrieve data:

```python
from simpledb import Database

# Initialise the database
db = Database('my_database.db')

# Store data
db['my_key'] = {'name': 'Amir', 'age': 33}

# Retrieve data
data = db['my_key']
print(data)

# Close the database
db.close()
```

### Something more realistic

Say we're working on a recommendation engine, SimpleDB can be used to store and retrieve embeddings:

```python
from simpledb import Database
import random

# Initialise the database
db = Database('embeddings.db')

# Generate and store embeddings
for i in range(1000):
    embedding = [random.random() for _ in range(128)]
    db[f'embedding_{i}'] = embedding

# Perform a similarity search
def similarity_search(query_embedding, threshold=0.5):
    similar_items = []
    for key, value in db:
        distance = sum((a - b) ** 2 for a, b in zip(query_embedding, value))
        if distance < threshold:
            similar_items.append((key, distance))
    return similar_items

# Example query
query_embedding = [random.random() for _ in range(128)]
similar_items = similarity_search(query_embedding)
print('Similar items:', similar_items)

# Close the database
db.close()
```
