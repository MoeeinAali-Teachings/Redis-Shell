---
marp: true
theme: beam
paginate: true
size: 16:9
header: Moeein Aali
footer: Introduction to Redis
title: Introduction to Redis
---
<!-- _class: title -->

# Introduction to Redis

[Moeein Aali](https://github.com/MoeeinAali/)

Sharif University of Technology
CE384 -  Database Design - Spring 2025
Dr. Ramezani


---
<!-- _class: tinytext -->

# Agenda

1. **What is Redis?**
   - Definition and Core Concepts
   - Key Features and Benefits

2. **Redis Use Cases**
   - Caching, Session Management, Real-time Analytics

3. **Getting Started with Redis**
   - Installation with Docker
   - Basic Configuration

4. **Redis CLI (Command Line Interface)**
   - Connecting and Basic Operations
   - Data Types and Commands

5. **Redis Tools**
   - Redis Insight Overview
   - Monitoring and Debugging

---

# What is Redis?

**Redis** = **RE**mote **DI**ctionary **S**erver

- **In-Memory Data Store**: Stores data primarily in RAM for ultra-fast access
- **NoSQL Database**: Key-value store with support for complex data structures
- **Open Source**: Created by Salvatore Sanfilippo in 2009
- **Single-threaded**: Uses event-driven architecture for high performance

## Key Characteristics
- **Persistent**: Can save data to disk
- **Atomic Operations**: All operations are atomic
- **Pub/Sub**: Built-in messaging system
- **Lua Scripting**: Execute custom scripts server-side

---

# Why Redis? Performance Benefits

## Speed Comparison
- **Traditional Database**: ~1,000-10,000 operations/second
- **Redis**: ~100,000-1,000,000+ operations/second

## Memory vs Disk
```
Disk Access Time: ~10ms
RAM Access Time: ~0.1ms
Redis is 100x faster than disk-based databases!
```

**Trade-off**: Speed vs Persistence
- Redis prioritizes speed
- Persistence is optional but available

---

# Redis Data Types

Redis supports various data structures:

| Data Type | Description | Example Use Case |
|-----------|-------------|------------------|
| **String** | Binary-safe strings | Caching HTML pages |
| **Hash** | Field-value pairs | User profiles |
| **List** | Ordered collection | Message queues |
| **Set** | Unordered unique values | Tags, followers |
| **Sorted Set** | Ordered set with scores | Leaderboards |
| **Bitmap** | Bit arrays | Analytics |
| **HyperLogLog** | Probabilistic counting | Unique visitors |

---

# Redis Use Cases

## 1. **Caching Layer**
```
Application → Redis (Cache) → Database
Fast retrieval of frequently accessed data
```

## 2. **Session Management**
- Store user sessions in web applications
- Share sessions across multiple servers

## 3. **Real-time Analytics**
- Count page views, user actions
- Leaderboards and rankings

## 4. **Message Broker**
- Pub/Sub messaging
- Task queues with Redis Lists

---

# Getting Started: Redis with Docker

## Installation Command
```bash
# Pull Redis image
docker pull redis:latest

# Run Redis container
docker run --name my-redis -p 6379:6379 -d redis:latest

# Run with persistent storage
docker run --name my-redis -p 6379:6379 -v redis-data:/data -d redis:latest
```

## Verify Installation
```bash
# Check if container is running
docker ps

# View Redis logs
docker logs my-redis
```

---

# Connecting to Redis CLI

## Basic Connection
```bash
# Connect to Redis CLI
docker exec -it my-redis redis-cli

# Alternative: Connect from host (if redis-cli installed)
redis-cli -h localhost -p 6379
```

## Connection with Authentication
```bash
# If password is set
redis-cli -h localhost -p 6379 -a yourpassword

# Or authenticate after connection
AUTH yourpassword
```

**Welcome Message:**
```
127.0.0.1:6379>
```

---

# Basic Redis CLI Commands

## Test Connection
```redis
# Test if Redis is working
PING
# Response: PONG

# Get server info
INFO server

# Select database (0-15, default is 0)
SELECT 1
```

## Essential Commands
```redis
# Clear current database
FLUSHDB

# Clear all databases
FLUSHALL

# Exit CLI
EXIT
```

---

# Working with Strings

## Basic String Operations
```redis
# Set a key-value pair
SET name "Ahmad"

# Get value by key
GET name
# Output: "Ahmad"

# Set with expiration (10 seconds)
SETEX temp_key 10 "temporary value"

# Set multiple keys at once
MSET key1 "value1" key2 "value2" key3 "value3"

# Get multiple keys
MGET key1 key2 key3
```

## String Manipulation
```redis
# Append to existing string
APPEND name " Rezaei"
GET name
# Output: "Ahmad Rezaei"

# Get string length
STRLEN name
# Output: 12
```

---

# Working with Numbers

## Numeric Operations
```redis
# Set a number
SET counter 10

# Increment by 1
INCR counter
# Output: 11

# Increment by specific amount
INCRBY counter 5
# Output: 16

# Decrement
DECR counter
# Output: 15

# Decrement by specific amount
DECRBY counter 3
# Output: 12
```

**Use Case**: Page view counters, user scores

---

# Working with Hashes

## Hash Operations
```redis
# Set hash field
HSET user:1001 name "Sara" age 25 city "Tehran"

# Get specific field
HGET user:1001 name
# Output: "Sara"

# Get all fields and values
HGETALL user:1001
# Output: 1) "name" 2) "Sara" 3) "age" 4) "25" 5) "city" 6) "Tehran"

# Get multiple fields
HMGET user:1001 name city
# Output: 1) "Sara" 2) "Tehran"

# Check if field exists
HEXISTS user:1001 email
# Output: 0 (false)
```

## Hash Utilities
```redis
# Get all field names
HKEYS user:1001

# Get all values
HVALS user:1001

# Get number of fields
HLEN user:1001
```

---

# Working with Lists

## List Operations
```redis
# Add to beginning of list
LPUSH tasks "task1" "task2" "task3"

# Add to end of list
RPUSH tasks "task4"

# Get list length
LLEN tasks
# Output: 4

# Get elements by range
LRANGE tasks 0 -1
# Output: 1) "task3" 2) "task2" 3) "task1" 4) "task4"

# Get specific element by index
LINDEX tasks 0
# Output: "task3"
```

## List as Queue/Stack
```redis
# Remove from beginning (queue behavior)
LPOP tasks

# Remove from end (stack behavior)
RPOP tasks
```

---

# Working with Sets

## Set Operations
```redis
# Add members to set
SADD tags "redis" "database" "cache" "nosql"

# Check if member exists
SISMEMBER tags "redis"
# Output: 1 (true)

# Get all members
SMEMBERS tags
# Output: 1) "redis" 2) "database" 3) "cache" 4) "nosql"

# Get set size
SCARD tags
# Output: 4

# Remove member
SREM tags "cache"
```

## Set Operations (Mathematical)
```redis
SADD set1 "a" "b" "c"
SADD set2 "b" "c" "d"

# Intersection
SINTER set1 set2
# Output: 1) "b" 2) "c"

# Union
SUNION set1 set2
# Output: 1) "a" 2) "b" 3) "c" 4) "d"

# Difference
SDIFF set1 set2
# Output: 1) "a"
```

---

# Working with Sorted Sets

## Sorted Set Operations
```redis
# Add members with scores
ZADD leaderboard 1500 "Player1" 1200 "Player2" 1800 "Player3"

# Get members by rank (lowest to highest)
ZRANGE leaderboard 0 -1
# Output: 1) "Player2" 2) "Player1" 3) "Player3"

# Get members with scores
ZRANGE leaderboard 0 -1 WITHSCORES
# Output: 1) "Player2" 2) "1200" 3) "Player1" 4) "1500" 5) "Player3" 6) "1800"

# Get members by score range
ZRANGEBYSCORE leaderboard 1300 1600
# Output: 1) "Player1"

# Get player rank
ZRANK leaderboard "Player1"
# Output: 1 (zero-based)
```

---

# Key Management Commands

## Key Information
```redis
# Check if key exists
EXISTS name
# Output: 1 (exists) or 0 (doesn't exist)

# Get key type
TYPE name
# Output: string

# Get all keys (use carefully in production!)
KEYS *

# Get keys with pattern
KEYS user:*

# Get time to live (TTL)
TTL temp_key
# Output: seconds remaining or -1 if no expiration
```

## Key Expiration
```redis
# Set expiration (in seconds)
EXPIRE name 3600

# Set expiration at specific time
EXPIREAT name 1640995200

# Remove expiration
PERSIST name

# Delete key
DEL name
```

---

# Practical Exercise 1: User Management System

## Task: Create a user management system using Redis

```redis
# Create user profile
HSET user:1001 name "Ali Ahmadi" email "ali@example.com" age 28 city "Isfahan"

# Create user preferences
HSET user:1001:prefs theme "dark" language "fa" notifications "enabled"

# Add user to groups
SADD group:admins "user:1001"
SADD group:developers "user:1001"

# Track user login count
SET user:1001:login_count 0
INCR user:1001:login_count

# Set user session with expiration
SETEX user:1001:session 3600 "session_token_xyz"
```

## Your Turn!
Create a user profile for user:1002 with your own data and add them to relevant groups.

---

# Practical Exercise 2: E-commerce Cart System

## Task: Implement a shopping cart using Redis Lists

```redis
# Add items to cart
LPUSH cart:user123 "product:101" "product:205" "product:150"

# View cart contents
LRANGE cart:user123 0 -1

# Remove last added item
LPOP cart:user123

# Get cart size
LLEN cart:user123

# Track product popularity
ZINCRBY popular_products 1 "product:101"
ZINCRBY popular_products 1 "product:205"
ZINCRBY popular_products 3 "product:150"

# Get top 5 popular products
ZREVRANGE popular_products 0 4 WITHSCORES
```

---

# Redis Configuration and Persistence

## Redis Configuration File
```bash
# View current configuration
CONFIG GET *

# Set configuration
CONFIG SET maxmemory 100mb
CONFIG SET maxmemory-policy allkeys-lru

# Save configuration to file
CONFIG REWRITE
```

## Persistence Options
```redis
# Manual save to disk
SAVE

# Background save
BGSAVE

# Get last save time
LASTSAVE

# Auto-save configuration
CONFIG SET save "900 1 300 10 60 10000"
# Save if: 1 change in 900s, 10 changes in 300s, 10000 changes in 60s
```

---

# Redis Insight Tool

## What is Redis Insight?
- **Visual Redis Management Tool**
- **Web-based Interface**
- **Free tool by Redis**

## Installation with Docker
```bash
docker run -d --name redis-insight \
  -p 8001:8001 \
  redislabs/redis-insight:latest
```

## Features
- **Browser-based**: Access via http://localhost:8001
- **Key Browser**: Visual key exploration
- **Query Editor**: Execute Redis commands with syntax highlighting
- **Performance Monitoring**: Real-time metrics
- **Memory Analysis**: Analyze memory usage patterns

---

# Redis Insight Features Demo

## Key Browser
- Visual representation of all keys
- Filter and search capabilities
- Key type indicators
- TTL information

## Command Editor
- Syntax highlighting
- Command history
- Auto-completion
- Result formatting

## Performance Dashboard
- Operations per second
- Memory usage
- Connected clients
- Hit/miss ratios

**Note**: While Redis Insight is useful for development and debugging, mastering the CLI is essential for production environments.

---

# Quiz Time! 🧠

## Question 1
What command would you use to create a hash for a product with id 501, name "Laptop", price 1500, and category "Electronics"?

<details>
<summary>Answer</summary>

```redis
HSET product:501 name "Laptop" price 1500 category "Electronics"
```
</details>

## Question 2
How would you add three items to a shopping cart and then remove the most recently added item?

<details>
<summary>Answer</summary>

```redis
LPUSH cart:user456 "item1" "item2" "item3"
LPOP cart:user456
```
</details>

---

# Advanced Redis CLI Features

## Monitoring Commands
```redis
# Monitor all commands in real-time
MONITOR

# Get Redis statistics
INFO stats

# Get memory usage info
MEMORY USAGE keyname

# Get client list
CLIENT LIST

# Get slow queries
SLOWLOG GET 10
```

## Debugging Commands
```redis
# Debug specific key
DEBUG OBJECT keyname

# Get Redis version
INFO server

# Check Redis configuration
CONFIG GET maxmemory
```

---

# Production Best Practices

## Security
```redis
# Set password
CONFIG SET requirepass "your_strong_password"

# Disable dangerous commands
CONFIG SET rename-command FLUSHDB ""
CONFIG SET rename-command FLUSHALL ""
```

## Performance Optimization
```redis
# Set memory policy
CONFIG SET maxmemory-policy allkeys-lru

# Enable AOF persistence
CONFIG SET appendonly yes

# Optimize for your use case
CONFIG SET maxmemory 2gb
```

## Monitoring
- Use `INFO` command regularly
- Monitor memory usage
- Track slow queries with `SLOWLOG`
- Set up alerts for memory thresholds

---

# Common Redis Patterns

## 1. Caching Pattern
```redis
# Check cache first
GET cache:user:123
# If not found, fetch from database and cache
SETEX cache:user:123 3600 "user_data_from_db"
```

## 2. Counter Pattern
```redis
# Page views counter
INCR page_views:homepage
# Daily unique visitors
PFADD unique_visitors:2024-01-15 "user123" "user456"
```

## 3. Session Management
```redis
# Store session data
HSET session:abc123 user_id 1001 login_time 1640995200
EXPIRE session:abc123 3600
```

## 4. Pub/Sub Pattern
```redis
# Publisher
PUBLISH news:tech "New Redis version released!"
# Subscriber
SUBSCRIBE news:tech
```

---

# Troubleshooting Common Issues

## Connection Issues
```bash
# Check if Redis is running
docker ps | grep redis

# Check port availability
netstat -an | grep 6379

# Test connection
redis-cli ping
```

## Memory Issues
```redis
# Check memory usage
INFO memory

# Check biggest keys
redis-cli --bigkeys

# Set memory limit
CONFIG SET maxmemory 1gb
```

## Performance Issues
```redis
# Check slow queries
SLOWLOG GET 10

# Monitor commands
MONITOR

# Check client connections
CLIENT LIST
```

---

# Hands-on Lab: Build a Real-time Voting System

## Requirements
- Store votes for multiple candidates
- Track total votes
- Get real-time rankings
- Prevent duplicate voting

## Implementation
```redis
# Initialize candidates
ZADD votes 0 "Candidate_A" 0 "Candidate_B" 0 "Candidate_C"

# Cast vote (increment score)
ZINCRBY votes 1 "Candidate_A"

# Check if user already voted
SISMEMBER voted_users "user123"
SADD voted_users "user123"

# Get current rankings
ZREVRANGE votes 0 -1 WITHSCORES

# Get total votes
EVAL "return redis.call('ZRANGE', 'votes', 0, -1, 'WITHSCORES')" 0
```

---

# Exercise Solution & Discussion

## Complete Voting System
```redis
# Vote function (with duplicate prevention)
MULTI
SISMEMBER voted_users "user123"
EXEC
# If returns 0 (user hasn't voted):
MULTI
SADD voted_users "user123"
ZINCRBY votes 1 "Candidate_A"
EXEC

# Get results
ZREVRANGE votes 0 -1 WITHSCORES

# Get vote statistics
ZCARD votes          # Number of candidates
SCARD voted_users    # Number of users who voted
```

## Discussion Points
- How to handle concurrent votes?
- What about vote expiration?
- How to scale for millions of users?

---

# Comparison: Redis vs Traditional Databases

| Feature | Redis | Traditional DB |
|---------|-------|----------------|
| **Speed** | ~100K ops/sec | ~1K ops/sec |
| **Data Model** | Key-Value + Structures | Relational Tables |
| **Persistence** | Optional | Always |
| **Memory Usage** | High (in-memory) | Low (disk-based) |
| **Scalability** | Horizontal (Cluster) | Vertical mostly |
| **ACID** | Limited | Full |
| **Queries** | Simple | Complex (SQL) |
| **Use Case** | Cache, Session, Real-time | Complex business logic |

---

# When to Use Redis

## ✅ Perfect for:
- **Caching**: Frequently accessed data
- **Session Management**: Web application sessions
- **Real-time Analytics**: Counters, metrics
- **Pub/Sub**: Messaging systems
- **Leaderboards**: Gaming, rankings
- **Rate Limiting**: API throttling

## ❌ Not ideal for:
- **Complex Queries**: Multi-table joins
- **Large Datasets**: > Available RAM
- **ACID Transactions**: Complex business logic
- **Reporting**: Historical data analysis
- **Primary Database**: Critical data without backup

---

# Redis in Production Architecture

```
[Web Application]
        ↓
[Load Balancer]
        ↓
[Application Servers] ←→ [Redis Cluster]
        ↓                      ↓
[Primary Database] ←→ [Redis Backup]
```

## Typical Setup:
- **Redis as Cache**: Between app and database
- **Redis Cluster**: For high availability
- **Backup Strategy**: AOF + RDB snapshots
- **Monitoring**: Redis Insight + Custom alerts

---

# Next Steps: Advanced Redis Topics

## Topics to Explore Further:
1. **Redis Cluster**: Horizontal scaling
2. **Redis Sentinel**: High availability
3. **Redis Modules**: Extend functionality
4. **Redis Streams**: Event streaming
5. **Lua Scripting**: Custom server-side logic
6. **Redis with Programming Languages**: Python, Java, Node.js clients

## Resources:
- **Official Documentation**: https://redis.io/documentation
- **Redis University**: Free online courses
- **Try Redis**: https://try.redis.io/
- **Redis Commands Reference**: https://redis.io/commands

---

# Summary

## What We Learned:
- Redis is an in-memory data store optimized for speed
- Supports multiple data types: strings, hashes, lists, sets, sorted sets
- Redis CLI is the primary interface for interacting with Redis
- Docker makes Redis installation and management easy
- Redis Insight provides a visual interface for development

## Key Takeaways:
- **Master the CLI** - Essential for production work
- **Understand data types** - Choose the right structure for your use case
- **Practice commands** - Hands-on experience is crucial
- **Know when to use Redis** - It's not a silver bullet

## Practice Assignment:
Design and implement a Redis-based system for a library management system including book inventory, user borrowing history, and popular books tracking.

---
<!-- _class: title -->

# The End!