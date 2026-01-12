# 🧠 ONQL — Object Notation Query Language
### Complete Developer Guide & Documentation

> **ONQL (Object Notation Query Language)** is a next-generation, **context-aware database** designed for the modern era of APIs and AI integration. It understands relationships **without explicit joins**, making data fetching faster, smarter, and more natural.

**Developed by:** Autobit Software Services Pvt. Ltd.  
**Last Updated:** January 2026

---

## 📑 Table of Contents

1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Architecture](#architecture)
4. [Installation & Setup](#installation--setup)
5. [Schema Management](#schema-management)
6. [ONQL Query Language](#onql-query-language)
7. [Protocols & Relations](#protocols--relations)
8. [Data Operations](#data-operations)
9. [Shell Client Usage](#shell-client-usage)
10. [Docker Deployment](#docker-deployment)
11. [Extensions & Plugins](#extensions--plugins)
12. [Complete Examples](#complete-examples)
13. [API Reference](#api-reference)
14. [Contributing](#contributing)

---

## 🚀 Overview

ONQL is built to simplify how data is fetched, related, and exposed through APIs. Traditional databases require **joins, foreign keys, and rigid schema design** to fetch related data. ONQL introduces **context-awareness**, allowing the engine to automatically understand and retrieve related objects — ideal for **AI-driven systems**, **microservices**, and **API backends**.

### Why ONQL?

- **No Manual Joins:** Query related data naturally using dot notation
- **API-First Design:** Expose data directly to frontends without extensive backend code
- **Context-Aware Security:** Define access controls within your data relationships
- **Object-Based Queries:** Use intuitive, human-friendly syntax inspired by object notation
- **Real-Time Streaming:** Built-in support for data subscriptions via NATS
- **Extensible Architecture:** Add custom functionality through plugins

---

## ✨ Key Features

- 🧩 **Context-Aware Query Engine** — Fetch related data automatically without manual joins
- ⚡ **Object Notation Query Language (ONQL)** — Human-friendly query syntax
- 🌐 **API-First Design** — Direct frontend-to-database queries with security
- 🤖 **AI & Microservices Ready** — Simplify data consumption for complex systems
- 🛡️ **Context-Aware Security** — Row-level access controls and permissions
- 🔄 **Real-Time Streaming** — Subscribe to data changes with NATS messaging
- 🐳 **Docker Ready** — Easy deployment with Docker Compose
- 🔌 **Extensible** — Custom extensions via Python plugins
- 📊 **Multiple Data Types** — String, Number, Timestamp, JSON support
- 🎯 **Aggregates & Functions** — Built-in aggregation functions

---

## 🏗️ Architecture

ONQL is organized into several specialized components:

### Core Components

```
onql/
├── cmd/server/          # Application entry point
├── engine/              # BadgerDB & NATS integration
├── storemanager/        # Low-level database operations
├── database/            # High-level DB interface with validation
├── dsl/                 # ONQL parser, lexer, and evaluator
│   ├── parser/          # Query parsing
│   └── evaluator/       # Query execution
├── server/              # TCP server
├── router/              # Request routing
├── extensions/          # Core extensions (Python-based)
│   ├── schema/          # Schema management extension
│   └── protocol/        # Protocol management extension
├── config/              # Configuration management
└── utils/               # Utility functions
```

### Data Flow

1. **Client** → Sends query via TCP (port 5656 default)
2. **Server** → Receives and routes to appropriate handler
3. **Router** → Distributes to database, DSL, or extensions
4. **DSL Engine** → Parses and evaluates ONQL queries
5. **StoreManager** → Interacts with BadgerDB storage
6. **Response** → Returns JSON data to client

---

## 📦 Installation & Setup

### Prerequisites

- **Go 1.24.4+** (for server)
- **Python 3.7+** (for shell client)
- **BadgerDB** (embedded, installed automatically)
- **NATS** (for messaging)

### Server Installation

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/onql.git
cd onql
```

2. **Configure environment:**
Create `config/.env` file:
```env
SERVER_PORT=5656
NATS_URL=nats://localhost:4222
DATA_DIR=./data
LOG_LEVEL=info
```

3. **Install dependencies:**
```bash
go mod download
```

4. **Build the server:**
```bash
go build -o onql-server cmd/server/main.go
```

5. **Run the server:**
```bash
./onql-server
```

The server will start on port 5656 (default) or the port specified in your `.env` file.

### Shell Client Installation

1. **Navigate to client directory:**
```bash
cd onql-shell-client
```

2. **Create virtual environment (recommended):**
```bash
python -m venv myvenv
# Windows
myvenv\Scripts\activate
# Linux/Mac
source myvenv/bin/activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Run the client:**
```bash
python main.py
```

5. **Connect to server:**
```
Enter host :- localhost
Enter port :- 5656
```

---

## 🗂️ Schema Management

ONQL schemas define the structure of your databases and tables. They provide the foundational blueprint for your data.

### Data Types

| Type | Description | Example |
|------|-------------|---------|
| `string` | Text data | `"John Doe"` |
| `number` | Numeric values | `42`, `3.14` |
| `timestamp` | Date/time values | `"2026-01-12T10:30:00Z"` |
| `json` | JSON objects | `{"key": "value"}` |

### Column Attributes

Each column in ONQL has several attributes that define its behavior:

| Attribute | Values | Description |
|-----------|--------|-------------|
| **type** | `string`, `number`, `timestamp`, `json` | Data type of the column |
| **storage** | `disk`, `ram` | Storage location - `disk` for persistent storage, `ram` for in-memory (faster but volatile) |
| **blank** | `yes`, `no` | Whether field is optional (`yes`) or required (`no`) |
| **default** | Any value | Default value when field is not provided |

**Storage Types:**
- **disk**: Data persists on disk (survives restarts, slower access)
- **ram**: Data stored in memory (faster access, lost on restart)

**Blank Setting:**
- **no**: Field is required, cannot be empty or null
- **yes**: Field is optional, can be empty or omitted

### Schema Definition Structure

```json
{
  "database_name": {
    "table_name": {
      "column_name": {
        "type": "string|number|timestamp|json",
        "storage": "disk|ram",
        "blank": "yes|no",
        "default": "default_value"
      }
    }
  }
}
```

### Example Schema

```json
{
  "mydb": {
    "users": {
      "id": {
        "type": "string",
        "blank": "no",
        "default": ""
      },
      "username": {
        "type": "string",
        "blank": "no",
        "default": ""
      },
      "email": {
        "type": "string",
        "blank": "no",
        "default": ""
      },
      "balance": {
        "type": "number",
        "blank": "yes",
        "default": "0"
      },
      "created_at": {
        "type": "timestamp",
        "blank": "no",
        "default": ""
      },
      "metadata": {
        "type": "json",
        "blank": "yes",
        "default": "{}"
      }
    },
    "orders": {
      "id": {
        "type": "string",
        "blank": "no",
        "default": ""
      },
      "user_id": {
        "type": "string",
        "blank": "no",
        "default": ""
      },
      "amount": {
        "type": "number",
        "blank": "no",
        "default": "0"
      },
      "status": {
        "type": "string",
        "blank": "no",
        "default": "pending"
      }
    }
  }
}
```

### Shell Commands for Schema

```bash
# List all databases
schema databases

# List tables in a database
schema tables mydb

# Describe a table schema
schema desc mydb users

# Create a database
schema create db mydb

# Create a table
# Format: (column_name, type, storage, blank, default)
schema create table mydb users (id,string,disk,no,"")(username,string,disk,no,"")(balance,number,ram,yes,0)
#                               ↑   ↑      ↑    ↑   ↑
#                            name type storage blank default

# Examples with different configurations:
# Required string on disk
schema create table mydb users (name,string,disk,no,"")

# Optional number in RAM with default
schema create table mydb stats (count,number,ram,yes,0)

# Required timestamp on disk
schema create table mydb logs (created_at,timestamp,disk,no,"")

# Optional JSON in RAM
schema create table mydb metadata (data,json,ram,yes,"{}")

# Rename database
schema rename db oldname newname

# Rename table
schema rename table mydb oldtable newtable

# Drop database
schema drop db mydb

# Drop table
schema drop table mydb users

# Set schema from JSON file
schema set /path/to/schema.json

# Refresh indexes
schema refresh-indexes
```

---

## 🔍 ONQL Query Language

ONQL is a powerful Domain-Specific Language (DSL) that allows you to query your database using intuitive object notation.

### Data Types in Queries

| Type | Description | Example |
|------|-------------|---------|
| **Table** | Array of objects | `db.users` |
| **Row** | Single object from table | `db.users[0]` |
| **List** | Array of values from column | `db.users.username` |
| **Field** | Single value | `db.users[0].username` |
| **Literal** | Static value | `"harry"`, `1234` |

### Operators

#### Arithmetic Operators
- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Division
- `%` Modulo
- `**` Exponentiation

#### Comparison Operators
- `<` Less than
- `>` Greater than
- `<=` Less than or equal
- `>=` Greater than or equal
- `!=` Not equal
- `in` Membership test

#### Logical Operators
- `and` Logical AND
- `or` Logical OR
- `not` Logical NOT
- `( )` Grouping/Priority

### Basic Queries

```onql
# Get all users
db.users

# Get a single user by index
db.users[0]

# Get all usernames
db.users.username

# Get specific user's username
db.users[0].username
```

### Filtering

Use square brackets `[]` to filter data:

```onql
# Simple filter
db.users[balance > 1000]

# Multiple conditions
db.users[balance > 1000 and age > 30]

# Complex expressions
db.users[(balance + savings) > 10000]

# Using 'in' operator
db.users[status in ["active", "premium"]]

# Nested query filter
db.users[id in db.orders[amount > 100].user_id]

# Related table filter
db.users[orders.amount > 500]

# Aggregate in filter
db.users[orders.amount._sum > 5000]

# Chain filters
db.users[balance > 0][age > 25]
```

### Slicing

Python-style slicing to retrieve subsets:

```onql
# Get users 10-20
db.users[10:20]

# Get first 10 users
db.users[0:10]

# Get every 2nd user from first 20
db.users[0:20:2]

# Slice after filter
db.users[balance > 100][0:10]
```

### Projection

Use curly braces `{}` to select specific fields:

```onql
# Select specific fields
db.users{username, email, balance}

# Use aliases
db.users{"name": username, "mail": email}

# Mix aliases and regular fields
db.users{"name": username, email, balance}

# Include related data
db.users{username, orders}

# Nested query in projection
db.users{username, orders[status="pending"]}

# Complex projection
db.users{
  "user": username,
  "total_orders": orders._count,
  "total_spent": orders.amount._sum,
  recent_orders: orders._dsc(created_at)[0:5]
}
```

### Aggregate Functions

ONQL provides powerful aggregate functions:

| Function | Description | Example |
|----------|-------------|---------|
| `_sum` | Sum of values | `db.orders.amount._sum` |
| `_max` | Maximum value | `db.orders.amount._max` |
| `_min` | Minimum value | `db.orders.amount._min` |
| `_count` | Count items | `db.orders._count` |
| `_asc(field)` | Sort ascending | `db.users._asc(username)` |
| `_dsc(field)` | Sort descending | `db.orders._dsc(created_at)` |
| `_like(pattern)` | SQL LIKE pattern | `db.users.username._like("john%")` |
| `_date(format)` | Format timestamp | `db.users.created_at._date("2006-01-02")` |

**Examples:**

```onql
# Total order amount
db.orders.amount._sum

# Highest balance user
db.users._dsc(balance)[0]

# Users with names starting with 'J'
db.users[username._like("J%")]

# Count active users
db.users[status="active"]._count

# Average order value (calculated)
db.orders.amount._sum / db.orders._count

# Format date
db.orders{id, created_at._date("2006-01-02 15:04:05")}
```

### Nested Queries

Use nested queries for complex filtering:

```onql
# Find users who have placed orders
db.users[id in db.orders.user_id]

# Find users with high-value orders
db.users[id in db.orders[amount > 1000].user_id]

# Access parent in nested query
db.users[id in db.orders[parent.id == user_id].user_id]

# Complex nested projection
db.users{
  username,
  high_value_orders: orders[amount > 1000]{
    amount,
    status,
    created_at._date("2006-01-02")
  }
}
```

### Relation Access

Use dot notation to access related data (defined in protocols):

```onql
# Access related orders
db.users.orders

# Filter on related data
db.users[orders.status="pending"]

# Aggregate on related data
db.users[orders.amount._sum > 5000]

# Project related data
db.users{username, orders{id, amount, status}}

# Multiple relation levels
db.users.orders.items
```

---

## 🔗 Protocols & Relations

Protocols define relationships between tables and context-based access controls. They act as a security and relation mapping layer over your schema.

### Protocol Structure

```json
{
  "protocol_alias": {
    "database": "actual_database_name",
    "entities": {
      "entity_alias": {
        "table": "actual_table_name",
        "fields": {
          "field_alias": "actual_column_name"
        },
        "relations": {
          "relation_name": {
            "prototable": "target_entity_alias",
            "type": "oto|otm|mto|mtm",
            "through": "junction_table_name",
            "entity": "actual_target_table",
            "fk_field": "source:target or source:junction:junction:target"
          }
        },
        "context": {
          "context_name": "filter_query"
        }
      }
    }
  }
}
```

### Relation Types

| Type | Name | Description | fk_field Format |
|------|------|-------------|-----------------|
| `oto` | One-to-One | Single record to single record | `source_col:target_col` |
| `otm` | One-to-Many | Single record to multiple records | `source_col:target_col` |
| `mto` | Many-to-One | Multiple records to single record | `source_col:target_col` |
| `mtm` | Many-to-Many | Many to many via junction table | `source:junction_source:junction_target:target` |

### Example Protocol

```json
{
  "shop": {
    "database": "ecommerce",
    "entities": {
      "users": {
        "table": "users",
        "fields": {
          "id": "id",
          "name": "username",
          "email": "email",
          "balance": "balance"
        },
        "relations": {
          "orders": {
            "prototable": "orders",
            "type": "otm",
            "through": "",
            "entity": "orders",
            "fk_field": "id:user_id"
          },
          "profile": {
            "prototable": "profiles",
            "type": "oto",
            "through": "",
            "entity": "user_profiles",
            "fk_field": "id:user_id"
          }
        },
        "context": {
          "admin": "",
          "user": "shop.users[id=$1]"
        }
      },
      "orders": {
        "table": "orders",
        "fields": {
          "id": "id",
          "amount": "amount",
          "status": "status",
          "user_id": "user_id"
        },
        "relations": {
          "user": {
            "prototable": "users",
            "type": "mto",
            "through": "",
            "entity": "users",
            "fk_field": "user_id:id"
          },
          "items": {
            "prototable": "items",
            "type": "mtm",
            "through": "order_items",
            "entity": "products",
            "fk_field": "id:order_id:product_id:id"
          }
        },
        "context": {
          "admin": "",
          "user": "shop.orders[user_id=$1]"
        }
      }
    }
  }
}
```

### Context-Based Access

Context allows you to filter data based on user permissions:

```json
"context": {
  "admin": "",  // Empty = full access
  "user": "shop.users[id=$1]"  // $1 will be replaced with context value
}
```

**Usage in Shell Client:**
```bash
onql> context
enter context key :- user
enter context values ',' separated :- user123

# Now queries will be filtered by context
onql> db.users
# Returns only data where id=user123
```

### Shell Commands for Protocols

```bash
# Describe current protocol
protocol desc

# Set protocol from JSON file
protocol set /path/to/protocol.json

# Drop protocol for a table
protocol drop mydb mytable

# Set protocol password in shell
onql> password
enter protocol password :- myprotocolpass
```

---

## 💾 Data Operations

### Insert Data

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": {
    "id": "user123",
    "username": "johndoe",
    "email": "john@example.com",
    "balance": 1000,
    "created_at": "2026-01-12T10:00:00Z",
    "metadata": {"role": "admin"}
  }
}
```

**Shell Command:**
```bash
insert {"db":"mydb","table":"users","records":{"id":"user123","username":"johndoe","email":"john@example.com","balance":1000}}
```

**Bulk Insert:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": [
    {"id": "user1", "username": "alice", "balance": 100},
    {"id": "user2", "username": "bob", "balance": 200},
    {"id": "user3", "username": "charlie", "balance": 300}
  ]
}
```

### Update Data

ONQL provides multiple methods for updating records:

#### **1. Direct Update by ID (Full Record)**
Updates all fields of a record identified by ID.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": {
    "id": "user123",
    "username": "john_updated",
    "email": "new@email.com",
    "balance": 2000
  }
}
```

**Shell Command:**
```bash
update {"db":"mydb","table":"users","records":{"id":"user123","username":"john_updated","balance":2000}}
```

**Characteristics:**
- Requires `id` field in the record
- Sets default values for missing columns
- Validates against schema
- Updates entire record

#### **2. Partial Update by ID**
Updates only specific fields of a record.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": {
    "id": "user123",
    "balance": 3000
  }
}
```

**Shell Command:**
```bash
update {"db":"mydb","table":"users","records":{"id":"user123","balance":3000}}
```

**Characteristics:**
- Only updates fields provided in the record
- Loads existing record, merges changes, then updates
- More efficient for single-field updates

#### **3. Update by ONQL Query**
Update multiple records matching an ONQL query.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": {
    "status": "premium"
  },
  "query": "mydb.users[balance > 5000].id",
  "protopass": "default"
}
```

**Shell Command:**
```bash
update {"db":"mydb","table":"users","records":{"status":"premium"},"query":"mydb.users[balance > 5000].id","protopass":"default"}
```

**How it works:**
1. Executes ONQL query to get list of IDs
2. Updates each record with the provided fields
3. Uses partial update internally

**Example Queries:**
```bash
# Update all users with high balance
update {"db":"mydb","table":"users","records":{"tier":"gold"},"query":"mydb.users[balance > 10000].id","protopass":"default"}

# Update users with pending orders
update {"db":"shop","table":"users","records":{"notify":true},"query":"shop.users[orders.status='pending'].id","protopass":"default"}

# Update top 10 users by balance
update {"db":"shop","table":"users","records":{"vip":true},"query":"shop.users._dsc(balance)[0:10].id","protopass":"default"}

# Update based on related data
update {"db":"shop","table":"users","records":{"loyalty":"platinum"},"query":"shop.users[orders.amount._sum > 50000].id","protopass":"default"}
```

#### **4. Update by ID Array**
Update multiple records by providing an array of IDs.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "records": {
    "status": "verified"
  },
  "ids": ["user1", "user2", "user3"],
  "protopass": "default"
}
```

**Shell Command:**
```bash
update {"db":"mydb","table":"users","records":{"status":"verified"},"ids":["user1","user2","user3"],"protopass":"default"}
```

### Delete Data

ONQL provides multiple methods for deleting records:

#### **1. Direct Delete by ID Array**
Delete specific records by their IDs.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "ids": ["user1", "user2", "user3"]
}
```

**Shell Command:**
```bash
delete {"db":"mydb","table":"users","ids":["user1","user2","user3"]}
```

**Characteristics:**
- Requires at least one ID
- Deletes multiple records in one operation
- Direct deletion, no query needed

#### **2. Delete by ONQL Query**
Delete multiple records matching an ONQL query.

**JSON Format:**
```json
{
  "db": "mydb",
  "table": "users",
  "query": "mydb.users[balance < 0].id",
  "protopass": "default"
}
```

**Shell Command:**
```bash
delete {"db":"mydb","table":"users","query":"mydb.users[balance < 0].id","protopass":"default"}
```

**How it works:**
1. Executes ONQL query to get list of IDs
2. Deletes all records matching those IDs

**Example Queries:**
```bash
# Delete inactive users
delete {"db":"shop","table":"users","query":"shop.users[status='inactive'].id","protopass":"default"}

# Delete old records
delete {"db":"shop","table":"logs","query":"shop.logs[created_at < 1640995200].id","protopass":"default"}

# Delete users with no orders
delete {"db":"shop","table":"users","query":"shop.users[orders._count = 0].id","protopass":"default"}

# Delete with pattern matching
delete {"db":"shop","table":"users","query":"shop.users[email._like('%@temporary.com')].id","protopass":"default"}

# Delete sliced results (bottom 50 by stock)
delete {"db":"shop","table":"products","query":"shop.products._asc(stock)[0:50].id","protopass":"default"}

# Complex conditional delete
delete {"db":"shop","table":"orders","query":"shop.orders[status='cancelled' and created_at < 1640995200].id","protopass":"default"}

# Delete with nested query
delete {"db":"shop","table":"users","query":"shop.users[id in shop.orders[status='fraud'].user_id].id","protopass":"default"}
```

### Update & Delete Best Practices

#### **Update Notes:**
- **ID is Mandatory** - All update operations require the `id` field
- **Full vs Partial** - Use full update for complete records, partial for specific fields
- **Validation** - All updates are validated against schema before execution
- **Type Conversion** - Automatic conversion for timestamp and number types
- **Default Values** - Missing fields get default values in full update
- **Bulk Updates** - Query-based updates iterate through all matching IDs

#### **Delete Notes:**
- **ID Required** - Must provide at least one ID to delete
- **Irreversible** - Delete operations cannot be undone
- **No Cascading** - ONQL doesn't automatically delete related records
- **Bulk Delete** - Query-based deletes can affect multiple records
- **Query Returns IDs** - Delete query must return ID field only

#### **Query-Based Operations:**
- **Protocol Required** - Need valid protocol password for query execution
- **Context Aware** - Queries respect context-based access controls
- **ID Selection** - Query must return `.id` field for bulk operations
- **Performance** - Large result sets may take time to process

---

## 🖥️ Shell Client Usage

The ONQL Shell provides an interactive command-line interface.

### General Commands

```bash
# Set active keyword (avoid typing it each time)
use onql

# Clear active keyword
out

# Clear console
clear

# Exit shell
exit
```

### Query Commands

```bash
# Execute ONQL query
onql db.users[balance > 1000]{username, balance}

# Set protocol password
onql> password
enter protocol password :- mypass

# Set context
onql> context
enter context key :- user
enter context values ',' separated :- user123

# After setting 'use onql', you can skip 'onql' prefix
use onql
db.users{username, orders._count}
```

### Complete Workflow Example

```bash
# Start shell
python main.py
Enter host :- localhost
Enter port :- 5656

# Create database
schema create db shop

# Create users table
schema create table shop users (id,string,disk,no,"")(username,string,disk,no,"")(balance,number,ram,yes,0)

# Create orders table
schema create table shop orders (id,string,disk,no,"")(user_id,string,disk,no,"")(amount,number,disk,no,0)(status,string,disk,no,pending)

# Insert users
insert {"db":"shop","table":"users","records":{"id":"u1","username":"alice","balance":1000}}
insert {"db":"shop","table":"users","records":{"id":"u2","username":"bob","balance":2000}}

# Insert orders
insert {"db":"shop","table":"orders","records":{"id":"o1","user_id":"u1","amount":100,"status":"completed"}}
insert {"db":"shop","table":"orders","records":{"id":"o2","user_id":"u1","amount":200,"status":"pending"}}

# Set protocol password
onql> password
enter protocol password :- default

# Query users
use onql
db.users

# Query with filter
db.users[balance > 1500]

# Query with projection
db.users{username, balance}

# Count users
db.users._count

# Sort by balance
db.users._dsc(balance)
```

---

## 🐳 Docker Deployment

ONQL provides Docker support for easy deployment.

### Docker Compose Structure

```yaml
version: '3.8'

services:
  onql:
    image: autobitdevs/onql:1.3.0
    ports:
      - "5656:5656"
    environment:
      - SERVER_PORT=5656
      - DATA_DIR=/data
    volumes:
      - onql-data:/data
    networks:
      - onql-net

  nats:
    image: nats:latest
    ports:
      - "4222:4222"
    networks:
      - onql-net

  schema-extension:
    image: autobitdevs/onql-schema:1.3.0
    depends_on:
      - nats
    environment:
      - NATS_URL=nats://nats:4222
    networks:
      - onql-net

  protocol-extension:
    image: autobitdevs/onql-protocol:1.3.0
    depends_on:
      - nats
    environment:
      - NATS_URL=nats://nats:4222
    networks:
      - onql-net

volumes:
  onql-data:

networks:
  onql-net:
```

### Build Images

```bash
# Build all services
docker-compose -f docker/compose.build.yml build

# Build specific service
docker-compose -f docker/compose.build.yml build onql
```

### Deploy Stack

```bash
# Start all services
docker-compose -f docker/stack.yaml up -d

# Check logs
docker-compose -f docker/stack.yaml logs -f onql

# Stop services
docker-compose -f docker/stack.yaml down
```

---

## 🔌 Extensions & Plugins

ONQL supports Python-based extensions for custom functionality.

### Available Extensions

1. **Schema Extension** - Handles schema operations
2. **Protocol Extension** - Manages protocol definitions

### Extension Structure

```python
# extensions/custom/main.py
from request_handler import RequestHandler

class CustomExtension:
    def __init__(self):
        self.handler = RequestHandler()
    
    def process(self, data):
        # Custom logic here
        return {"result": "success"}

if __name__ == "__main__":
    ext = CustomExtension()
    ext.handler.listen("custom-topic", ext.process)
```

### Creating Custom Extensions

1. Create extension directory in `extensions/`
2. Implement `main.py` with your logic
3. Add to `extensions/registry.json`
4. Update Docker configuration
5. Deploy with Docker Compose

---

## 📚 Complete Examples

### Example 1: E-Commerce System

#### Schema Setup

```json
{
  "shop": {
    "users": {
      "id": {"type": "string", "blank": "no", "default": ""},
      "username": {"type": "string", "blank": "no", "default": ""},
      "email": {"type": "string", "blank": "no", "default": ""},
      "balance": {"type": "number", "blank": "yes", "default": "0"}
    },
    "products": {
      "id": {"type": "string", "blank": "no", "default": ""},
      "name": {"type": "string", "blank": "no", "default": ""},
      "price": {"type": "number", "blank": "no", "default": "0"},
      "stock": {"type": "number", "blank": "no", "default": "0"}
    },
    "orders": {
      "id": {"type": "string", "blank": "no", "default": ""},
      "user_id": {"type": "string", "blank": "no", "default": ""},
      "total": {"type": "number", "blank": "no", "default": "0"},
      "status": {"type": "string", "blank": "no", "default": "pending"}
    },
    "order_items": {
      "id": {"type": "string", "blank": "no", "default": ""},
      "order_id": {"type": "string", "blank": "no", "default": ""},
      "product_id": {"type": "string", "blank": "no", "default": ""},
      "quantity": {"type": "number", "blank": "no", "default": "1"},
      "price": {"type": "number", "blank": "no", "default": "0"}
    }
  }
}
```

#### Protocol Setup

```json
{
  "shop": {
    "database": "shop",
    "entities": {
      "users": {
        "table": "users",
        "fields": {
          "id": "id",
          "name": "username",
          "email": "email",
          "balance": "balance"
        },
        "relations": {
          "orders": {
            "prototable": "orders",
            "type": "otm",
            "entity": "orders",
            "fk_field": "id:user_id"
          }
        },
        "context": {"user": "shop.users[id=$1]"}
      },
      "orders": {
        "table": "orders",
        "fields": {
          "id": "id",
          "total": "total",
          "status": "status"
        },
        "relations": {
          "user": {
            "prototable": "users",
            "type": "mto",
            "entity": "users",
            "fk_field": "user_id:id"
          },
          "items": {
            "prototable": "products",
            "type": "mtm",
            "through": "order_items",
            "entity": "products",
            "fk_field": "id:order_id:product_id:id"
          }
        },
        "context": {"user": "shop.orders[user_id=$1]"}
      },
      "products": {
        "table": "products",
        "fields": {
          "id": "id",
          "name": "name",
          "price": "price",
          "stock": "stock"
        },
        "relations": {},
        "context": {}
      }
    }
  }
}
```

#### Query Examples

```onql
# Get all users with their order count
shop.users{name, email, order_count: orders._count}

# Get users who spent more than $1000
shop.users[orders.total._sum > 1000]{
  name,
  total_spent: orders.total._sum,
  order_count: orders._count
}

# Get pending orders with user details
shop.orders[status="pending"]{
  id,
  total,
  user{name, email}
}

# Get top 10 products by sales
shop.products._dsc(stock)[0:10]{name, price, stock}

# Get user's order history with items
shop.users[id="u1"].orders{
  id,
  total,
  status,
  items{name, price}
}

# Complex: Users with high-value pending orders
shop.users[
  id in shop.orders[status="pending" and total > 500].user_id
]{
  name,
  email,
  pending_orders: orders[status="pending"]{
    id,
    total,
    items{name, price}
  }
}
```

### Example 2: Financial Trading System

#### Queries

```onql
# Get all instruments with their categories
db.instruments{
  name,
  trading_name,
  currency,
  dinamic_categories{name, type},
  category{name, properties}
}

# Find instruments in specific category
db.instruments[
  id in db.instrument_dinamic_categories_relation[
    category_id="cat123"
  ].instrument_id
]{name, trading_name}

# Get chart history for instrument
db.instruments[id="inst1"].history._dsc(timestamp)[0:100]

# Complex: Trading instruments with volume analysis
db.instruments[timings != "00:00-00:00"]{
  name,
  trading_name,
  currency{code, name},
  history[timestamp._date("2006-01-02") = "2026-01-12"]{
    open,
    high,
    low,
    close,
    volume
  }._dsc(timestamp)
}
```

### Example 3: Account Management

```onql
# Get account with all trades
db.accounts[account_id="acc1"]{
  account_id,
  balance,
  currency,
  orders{id, status, amount},
  trades{id, instrument_id, quantity, price}
}

# Find profitable accounts
db.accounts[trades.profit._sum > 10000]{
  account_id,
  balance,
  total_profit: trades.profit._sum,
  trade_count: trades._count
}._dsc(total_profit)

# Daily trading summary
db.accounts{
  account_id,
  today_trades: trades[
    created_at._date("2006-01-02") = "2026-01-12"
  ]._count,
  today_profit: trades[
    created_at._date("2006-01-02") = "2026-01-12"
  ].profit._sum
}
```

---

## 📖 API Reference

### Server Configuration (Environment Variables)

```env
SERVER_PORT=5656           # TCP server port
NATS_URL=nats://localhost:4222  # NATS messaging URL
DATA_DIR=./data           # Data storage directory
LOG_LEVEL=info            # Logging level (debug|info|warn|error)
```

### Request/Response Format

#### Request Format
```json
{
  "target": "onql|schema|protocol|insert|update|delete|subscribe",
  "payload": "JSON string or query string"
}
```

#### Response Format
```json
{
  "error": "error message or null",
  "data": "response data"
}
```

### ONQL Query Request

```json
{
  "query": "db.users[balance > 1000]",
  "protopass": "default",
  "ctxkey": "user",
  "ctxvalues": ["user123"]
}
```

### Schema Operations

These operations are used internally when communicating with the ONQL server. They can be invoked in three ways:

#### **1. Via Shell Client (Recommended)**
The shell client automatically formats these operations for you:
```bash
schema databases
schema tables mydb
schema desc mydb users
schema create db mydb
schema drop table mydb users
```

#### **2. Via TCP Client/SDK**
When building your own client, send these JSON arrays as the payload with target "schema":

**Request Format:**
```json
{
  "target": "schema",
  "payload": "[\"databases\"]"  // JSON array as string
}
```

**Operation Payloads:**

```json
// Get all databases
["databases"]

// Get tables in a database
["tables", "database_name"]

// Describe table structure
["desc", "database_name", "table_name"]

// Create new database
["create", "db", "database_name"]

// Create table with schema
["create", "table", "db_name", "table_name", {
  "column_name": {
    "type": "string",
    "storage": "disk",
    "blank": "no",
    "default": ""
  }
}]

// Rename database
["rename", "db", "old_name", "new_name"]

// Rename table
["rename", "table", "db_name", "old_table", "new_table"]

// Alter table (add/remove/modify columns)
["alter", "db_name", "table_name", {
  "action": "add",
  "column": "new_column",
  "schema": {"type": "string", "blank": "yes", "default": ""}
}]

// Drop database
["drop", "db", "database_name"]

// Drop table
["drop", "table", "database_name", "table_name"]

// Set entire schema from JSON
["set", {schema_json_object}]

// Refresh indexes
["refresh-indexes"]
```

#### **Response Format:**

All schema operations return a response in this format:
```json
{
  "error": null,  // or error message string
  "data": {
    // Response data varies by operation
  }
}
```

**Examples:**

```json
// databases response
{
  "error": null,
  "data": ["mydb", "shop", "logs"]
}

// tables response
{
  "error": null,
  "data": ["users", "orders", "products"]
}

// desc response
{
  "error": null,
  "data": {
    "id": {"type": "string", "blank": "no", "default": ""},
    "username": {"type": "string", "blank": "no", "default": ""}
  }
}

// create response
{
  "error": null,
  "data": "success"
}
```

### Data Operations

These operations are used to insert, update, and delete data. They can be invoked in multiple ways:

#### **1. Via Shell Client (Recommended)**
```bash
# Insert
insert {"db":"mydb","table":"users","records":{"id":"1","name":"Alice"}}

# Update
update {"db":"mydb","table":"users","records":{"id":"1","balance":500}}

# Delete
delete {"db":"mydb","table":"users","ids":["1"]}
```

#### **2. Via TCP Client/SDK**

**Request Format:**
```json
{
  "target": "insert|update|delete",
  "payload": "{JSON string with operation data}"
}
```

**Insert Operation:**
```json
{
  "db": "database_name",
  "table": "table_name",
  "records": {
    "id": "user123",
    "name": "John Doe",
    "balance": 1000
  }
}

// Bulk insert (array of records)
{
  "db": "database_name",
  "table": "table_name",
  "records": [
    {"id": "u1", "name": "Alice"},
    {"id": "u2", "name": "Bob"}
  ]
}
```

**Update Operations:**
```json
// Update by ID (partial)
{
  "db": "database_name",
  "table": "table_name",
  "records": {
    "id": "user123",
    "balance": 2000
  }
}

// Update by query
{
  "db": "database_name",
  "table": "table_name",
  "records": {"status": "premium"},
  "query": "mydb.users[balance > 5000].id",
  "protopass": "default"
}

// Update by ID array
{
  "db": "database_name",
  "table": "table_name",
  "records": {"verified": true},
  "ids": ["user1", "user2", "user3"],
  "protopass": "default"
}
```

**Delete Operations:**
```json
// Delete by ID array
{
  "db": "database_name",
  "table": "table_name",
  "ids": ["user1", "user2"]
}

// Delete by query
{
  "db": "database_name",
  "table": "table_name",
  "query": "mydb.users[status='inactive'].id",
  "protopass": "default"
}
```

#### **Response Format:**

All data operations return:
```json
{
  "error": null,  // or error message
  "data": "success" // or primary key for insert
}
```

**Examples:**

```json
// Successful insert
{
  "error": null,
  "data": "user123"  // Returns the ID of inserted record
}

// Successful update
{
  "error": null,
  "data": "success"
}

// Error response
{
  "error": "table does not exist",
  "data": ""
}
```

---

## 🤝 Contributing

We welcome contributions! Here are areas where you can help:

### Core Database Engine
- Improve storage engine performance and caching
- Enhance query optimization and execution
- Fix concurrency issues and race conditions
- Add performance monitoring and profiling
- Implement query result caching

### ONQL Language Features
- Add new aggregate functions and operators
- Improve parser error messages and debugging
- Optimize query execution plans
- Enhance type system and validation

### Extensions & Plugins
- Develop new extensions for data processing
- Create integration plugins for external services
- Build data import/export tools (CSV, SQL, JSON)
- Add monitoring, analytics, and logging extensions

### Client Libraries & SDKs
- Create language-specific clients (Node.js, Java, Go, Ruby, PHP)
- Build ORMs and query builders for popular frameworks
- Develop GUI clients and admin dashboards
- Create testing frameworks and mock servers

### Documentation & Community
- Improve existing documentation and examples
- Write tutorials and how-to guides
- Create video tutorials and screencasts
- Translate documentation to other languages
- Answer questions and help users

### Development Setup

```bash
# Clone repository
git clone https://github.com/yourusername/onql.git
cd onql

# Install Go dependencies
go mod download

# Run tests
go test ./...

# Build server
go build -o onql-server cmd/server/main.go

# Run server
./onql-server
```

### Code Style

- Follow Go best practices
- Use meaningful variable names
- Add comments for complex logic
- Write unit tests for new features
- Keep functions focused and small

### Pull Request Process

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Ensure all tests pass
6. Submit pull request with clear description

---

## 📄 License

See [LICENSE](LICENSE) file for details.

---

## 🔗 Resources

- **GitHub Repository:** [https://github.com/ONQL/](https://github.com/ONQL/)

---

## 💡 Quick Start Cheat Sheet

```bash
# Start Server
./onql-server

# Start Client
python main.py

# Create Database & Table
schema create db mydb
schema create table mydb users (id,string,disk,no,"")(name,string,disk,no,"")

# Insert Data
insert {"db":"mydb","table":"users","records":{"id":"1","name":"Alice"}}

# Query Data
use onql
db.users
db.users[name="Alice"]
db.users{name}
db.users._count

# Set Protocol Password
onql> password
enter protocol password :- default

# Set Context
onql> context
enter context key :- user
enter context values :- user123
```

---

**Built with ❤️ by Autobit Software Services Pvt. Ltd.**  
*Empowering developers with context-aware data access*
