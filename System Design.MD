# Steps for creating a system design

## Step 1: Requirements gathering
## Step 2: Capacity planning
   1. Number of transactions
   1. Storage
   1. Network bandwidth
## Step 3: System interface defintion (APIs)
## Step 4: Data model description
## Step 5: Block diagram of high level design
## Step 6: Detailed design of major components
    1. How should be store data? Data partitioning?
    2. Where should we introduce cache?
    3. Where is load balancing needed?
## Step 7: Identifying and resolving bottlenecks
    1. Any single failure points?
    2. Data replication for ensuring reliability
    3. Service replication for ensuring reliability
    4. Monitoring the performance
    5. Setting up alerts


# Example: URL Shortener

## Step 1: Requirements gathering

Functional Requirements: 
1. Given a URL, generate a unique short link.
2. A short link should redirect to original link. 
3. Users can pick custom short links.
4. Short links should expire after some time. 

Non-functional requirements:
1. High availability
2. Instant redirection
3. Non-predictable short links

Extended requirements:
1. Analytics on redirection
2. Accessible through REST APIs


## Step 2: Capacity planning
1. Traffic: This is a read-heavy service. Let's assyme 100: 1 read/write ratio.
   
Number of redirects = 500M (new url) * 100 (100:1 read/write ratio) = 50B redirects

Queries / sec = 100 * 500 M / (30 * 24 * 3600) = 20K/s

2. Storage: Assuming we store a url for 5 years.
   1. Total number of objects = 500M * 5 * 12 = 30B
   2. Data = 30B * 500 bytes (assuming 1 object = 500bytes) = 15TB
   3. Bandwidth 
      1. = 500 bytes * 20 K/s / 100 (read/write ratio) = 100KB/s (incoming) 
      2. = 500 bytes * 20 K/s (read/write ratio) = 10 MB/s (outgoing)
   4. Memory in a day (assuming 80: 20 rule) = 20K/s * 24 hours * 3600 sec * 500 bytes * 20% = 170GB
   5. High level estimates 
      1. New URLs : 200/s
      2. URL redirections: 20K/s
      3. Incoming data: 100KB/s
      4. Outgoing data: 10MB/s
      5. Storage for 5 years: 15TB
      6. Memory for cache: 170GB
   
## Step 3: System interface defintion
1. createURL(api_cred, url, alias, username, expiry) => returns string (url)
2. deleteURL(api_cred, url) => return boolean

## Step 4: Database design
1. We need to store billions of records
2. Each object is small
3. No relationship between records
4. Service is read heavy

Database schema: 2 tables: one for url, one for user. 
Database type: a NoSQL (since no rel).

## Step 5: High level design and algorithm
Encoding the URL: 
   1. We can create unique hash (MD5, SHA256) of url. We can store this in base36 or base 64. 
   2. Base 64 encoding, 6 letters long key  = 64 ^ 6 = 68.7T possible strings
   3. MD5 algorithm produces 128-bit hash value. pick the first 6 characters.
Generating Keys
   1. We could use a key generation service that generates random six-letter strings and stores them in a database. 

## Step 6: Detailed design of components
1. Data Partinioning: 
   1. Letter based partioning
   2. Hash based partioning
2. Cache
   1. Memcached for server side cache
   2. Cache memory - start with 20%
   3. Cache eviction policy - LRU cache
3. Load Balancer (LB)
   1. LB between clients and application server
   2. LB between application server and db server
   3. LB between application server and cache server



#### Notes from: Educative, grokking the system design interview. 