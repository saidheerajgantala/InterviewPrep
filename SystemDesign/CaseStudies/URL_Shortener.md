# 📝 URL Shortener System Design

## **Problem Statement**

* Design a URL shortening service like TinyURL or bit.ly.
* The service should take a long URL and return a significantly shorter URL.
* When users access the short URL, they should be redirected to the original long URL.
* Scale considerations: 100 million URLs generated per day, 10:1 read to write ratio.
* Example use case: A user shares a very long URL on Twitter where character count is limited.

---

## **Step 1: Requirements Clarification**

* **Functional Requirements:**
  * Given a URL, generate a shorter and unique alias.
  * When users access the short URL, redirect them to the original link.
  * Users should have the option to specify a custom short URL.
  * Links should expire after a default timespan (configurable).
  * Users should be able to specify expiration time.

* **Non-functional Requirements:**
  * High availability - the service should be up and running most of the time.
  * Minimal latency - URL redirection should happen in real-time with minimal delay.
  * The system should be scalable to handle growing users/URLs.
  * Shortened links should not be predictable.

* **Out of Scope:**
  * User authentication and authorization.
  * Analytics features for tracking link usage.
  * API rate limiting.

* **Assumptions:**
  * Traffic is not evenly distributed. Some URLs might be accessed much more frequently.
  * Once created, URLs cannot be modified but can be deleted.
  * The system doesn't need to handle all possible URLs on the internet.

---

## **Step 2: Back-of-the-envelope Estimation**

* **Traffic Estimates:**
  * New URLs: 100 million per day = ~1,160 URLs per second
  * URL redirections: 10 × 100 million = 1 billion per day = ~11,600 redirections per second

* **Storage Estimates:**
  * Assuming each URL entry takes 500 bytes:
    * Long URL: average 300 bytes
    * Short URL key: 7 bytes
    * Creation date, expiration date, user ID: ~200 bytes
  * 100 million URLs per day × 500 bytes = 50 GB per day
  * For 5 years: 50 GB × 365 × 5 = ~91 TB

* **Bandwidth Estimates:**
  * Incoming: 1,160 URLs/s × 500 bytes = ~580 KB/s
  * Outgoing: 11,600 redirections/s × 500 bytes = ~5.8 MB/s

* **Memory Requirements:**
  * If we cache 20% of the daily URLs:
    * 0.2 × 100 million × 500 bytes = ~10 GB

---

## **Step 3: System Interface Definition**

* **API Endpoints:**

  * Create a new shortened URL:
    ```
    POST /api/v1/shorten
    {
      "longUrl": "https://www.example.com/very/long/url/path",
      "customAlias": "custom" (optional),
      "expirationDate": "2023-12-31" (optional)
    }
    
    Response:
    {
      "shortUrl": "https://short.url/abcdef",
      "expirationDate": "2023-12-31"
    }
    ```

  * Redirect to original URL:
    ```
    GET /{shortUrlKey}
    
    Response: HTTP 302 redirect to original URL
    ```

  * Delete a shortened URL:
    ```
    DELETE /api/v1/urls/{shortUrlKey}
    
    Response: HTTP 204 No Content
    ```

* **Error Handling:**
  * 400 Bad Request: Invalid URL format
  * 404 Not Found: Short URL not found
  * 409 Conflict: Custom alias already exists
  * 410 Gone: URL has expired
  * 500 Internal Server Error: Server-side issues

---

## **Step 4: High-Level Design**

* **Core Components:**
  1. **Application Service**: Handles API requests, URL shortening logic, and redirections
  2. **Database**: Stores mappings between short and long URLs
  3. **Cache**: Stores frequently accessed URL mappings
  4. **Load Balancer**: Distributes incoming requests

* **Data Flow:**
  1. **URL Shortening:**
     * Client sends a request to shorten a URL
     * Application service generates a unique short key
     * Mapping is stored in the database
     * Short URL is returned to the client

  2. **URL Redirection:**
     * Client requests a short URL
     * Application service looks up the mapping in the cache
     * If not found, it queries the database
     * Client is redirected to the original URL

* **Initial Architecture Diagram:**
```
                    ┌─────────────┐
                    │Load Balancer│
                    └──────┬──────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │Application Service│
                 └─────┬───────┬─────┘
                       │       │
                       ▼       ▼
                ┌──────────┐ ┌─────────┐
                │  Cache   │ │Database │
                └──────────┘ └─────────┘
```

---

## **Step 5: Database Design**

* **Data Model:**
  * **URL Table:**
    * `short_key`: varchar(7), Primary Key
    * `original_url`: varchar(2048)
    * `creation_date`: timestamp
    * `expiration_date`: timestamp
    * `user_id`: varchar(128) (if we track users)

* **Schema Design:**
  ```sql
  CREATE TABLE urls (
    short_key VARCHAR(7) PRIMARY KEY,
    original_url VARCHAR(2048) NOT NULL,
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    expiration_date TIMESTAMP,
    user_id VARCHAR(128)
  );
  
  CREATE INDEX idx_expiration ON urls (expiration_date);
  CREATE INDEX idx_user_id ON urls (user_id);
  ```

* **Database Choice:**
  * Initially, a relational database like PostgreSQL would work well
  * As we scale, we might consider NoSQL options like Cassandra for better horizontal scaling

* **Indexing Strategy:**
  * Primary index on `short_key` for fast lookups
  * Secondary index on `expiration_date` for cleanup jobs
  * Secondary index on `user_id` if we need to retrieve all URLs for a user

* **Data Partitioning:**
  * Horizontal partitioning (sharding) based on `short_key`
  * Hash-based partitioning to distribute data evenly across database servers

---

## **Step 6: Detailed Component Design**

### **Component 1: URL Shortening Service**

* **Purpose and Responsibilities:**
  * Generate unique short keys for URLs
  * Store URL mappings in the database
  * Handle custom alias requests

* **Internal Design:**
  * **Key Generation Approaches:**
    1. **Base62 Encoding**: Convert an auto-incrementing ID to a base62 representation (a-z, A-Z, 0-9)
    2. **MD5 Hash**: Generate MD5 hash of the URL and take first 7 characters
    3. **Counter-based**: Use a distributed counter and encode the value

  * For this design, we'll use base62 encoding with a counter:
    ```
    function generateShortKey(counter):
        characters = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
        base = characters.length (62)
        shortKey = ""
        
        while counter > 0:
            shortKey = characters[counter % base] + shortKey
            counter = counter / base
            
        return shortKey.padStart(7, '0')
    ```

* **Scaling Considerations:**
  * Use a distributed counter service (e.g., Zookeeper) to avoid collisions
  * Pre-generate and store keys in a key database to improve performance

### **Component 2: URL Redirection Service**

* **Purpose and Responsibilities:**
  * Look up original URLs based on short keys
  * Redirect users to original URLs
  * Handle expired URLs

* **Internal Design:**
  * Use a fast lookup mechanism with caching:
    ```
    function redirect(shortKey):
        # Try cache first
        originalUrl = cache.get(shortKey)
        
        if not originalUrl:
            # Cache miss, query database
            originalUrl = database.query("SELECT original_url, expiration_date FROM urls WHERE short_key = ?", shortKey)
            
            if not originalUrl:
                return 404 NOT FOUND
                
            if originalUrl.expiration_date < current_time:
                return 410 GONE
                
            # Update cache
            cache.set(shortKey, originalUrl, TTL=86400)  # 1 day TTL
            
        return 302 REDIRECT to originalUrl
    ```

* **Scaling Considerations:**
  * Cache frequently accessed URLs to reduce database load
  * Distribute redirection servers geographically (CDN) to reduce latency

---

## **Step 7: Identifying and Resolving Bottlenecks**

* **Potential Bottlenecks:**
  * **Database read operations**: High volume of redirection requests could overload the database
  * **Key generation**: Could become a bottleneck if centralized
  * **Hot URLs**: Some URLs might be extremely popular, causing cache misses

* **Single Points of Failure:**
  * Database server
  * Key generation service
  * Application servers

* **Consistency vs. Availability Tradeoffs:**
  * For URL shortening, we can prioritize availability over strong consistency
  * It's acceptable if a newly created short URL takes a few seconds to propagate

* **Mitigation Strategies:**
  * **Database**: Use read replicas to handle high read traffic
  * **Key Generation**: Distribute pre-generated keys to application servers in batches
  * **Hot URLs**: Implement multiple layers of caching with longer TTL for popular URLs
  * **Redundancy**: Deploy services across multiple data centers

---

## **Step 8: Scaling the Design**

### **Horizontal Scaling**

* **Stateless Application Servers:**
  * Deploy multiple application servers behind a load balancer
  * Each server can handle URL shortening and redirection independently

* **Database Sharding:**
  * Shard the database based on the first 2 characters of the short key
  * This gives us 62² = 3,844 potential shards
  * Start with fewer shards and split as needed

* **Load Balancing Strategy:**
  * Use consistent hashing to distribute requests across application servers
  * This minimizes cache misses when servers are added or removed

### **Caching Strategy**

* **What to Cache:**
  * Frequently accessed URL mappings
  * Results of recent URL shortening operations

* **Cache Invalidation:**
  * Time-based expiration (TTL)
  * Lazy invalidation: update cache only when a cache miss occurs and DB has different data

* **Cache Placement:**
  * **Client-side**: Browser caching for repeated visits
  * **CDN**: For globally distributed access
  * **Application server**: Local cache for very hot URLs
  * **Distributed cache**: Redis or Memcached cluster for shared caching

### **Load Balancing**

* **Algorithm Choice:**
  * Round-robin for URL creation requests
  * Consistent hashing for URL redirection requests

* **Session Handling:**
  * No session state needed for basic functionality
  * If user accounts are added, use token-based authentication

* **Health Checks:**
  * Regular ping to application servers
  * Remove unhealthy instances from the load balancer

---

## **Step 9: Monitoring and Alerting**

* **Key Metrics to Track:**
  * Request rate (shortening and redirection)
  * Latency (p50, p95, p99)
  * Error rate
  * Cache hit/miss ratio
  * Database query performance
  * Resource utilization (CPU, memory, disk, network)

* **Alerting Thresholds:**
  * Error rate > 1%
  * p99 latency > 500ms
  * Cache hit ratio < 80%
  * Database CPU > 70%

* **Logging Strategy:**
  * Access logs for all API requests
  * Error logs with sufficient context
  * Distributed tracing for request flows
  * Aggregate logs in a centralized system (ELK stack)

* **Debugging Approaches:**
  * Correlation IDs for tracking requests across services
  * Detailed error messages (internal only)
  * Ability to increase log verbosity for specific components

---

## **Step 10: Security Considerations**

* **Authentication and Authorization:**
  * Rate limiting by IP address
  * API keys for authenticated users
  * CAPTCHA for public URL shortening to prevent abuse

* **Data Encryption:**
  * HTTPS for all endpoints
  * Encryption at rest for the database

* **Rate Limiting:**
  * Tiered rate limits based on user type
  * Sliding window rate limiting algorithm

* **DDoS Protection:**
  * Use a CDN with DDoS protection
  * Implement request throttling
  * Maintain an IP blacklist for repeated abusers

---

## **Tradeoff Summary**

| Design Decision | Pros | Cons | Alternatives Considered |
|---|---|---|---|
| Base62 encoding with counter | Simple, guarantees uniqueness, short URLs | Central counter can be a bottleneck | MD5 hashing (risk of collisions), UUID (longer URLs) |
| Relational DB initially | ACID properties, simpler to set up | Limited horizontal scalability | NoSQL from the start (more complex, eventual consistency) |
| Multi-layer caching | Reduces database load, improves latency | Increased complexity, potential consistency issues | Single Redis cache (simpler but less resilient) |
| Stateless application servers | Easy to scale horizontally | Requires external state management | Sticky sessions (less scalable) |

---

## **Final Architecture Diagram**

```
                                 ┌─────────┐
                                 │   CDN   │
                                 └────┬────┘
                                      │
                                      ▼
                               ┌────────────┐
                               │Load Balancer│
                               └──────┬─────┘
                                      │
                 ┌───────────────────┬┴┬───────────────────┐
                 │                   │ │                   │
                 ▼                   ▼ ▼                   ▼
         ┌───────────────┐   ┌───────────────┐   ┌───────────────┐
         │App Server 1   │   │App Server 2   │   │App Server 3   │
         └───────┬───────┘   └───────┬───────┘   └───────┬───────┘
                 │                   │                   │
                 └───────────┬───────┴───────────┬───────┘
                             │                   │
                     ┌───────┴───────┐   ┌───────┴───────┐
                     │ Cache Cluster │   │Counter Service│
                     └───────┬───────┘   └───────────────┘
                             │
                     ┌───────┴───────┐
                     │               │
             ┌───────┴───────┐ ┌─────┴───────┐
             │  Database     │ │  Database   │
             │  Primary      │ │  Replicas   │
             └───────────────┘ └─────────────┘
```

---

## **Real-world Examples**

* **Bit.ly:**
  * Uses a hash-based approach for generating short URLs
  * Provides analytics and custom domains as premium features
  * Scaled to handle billions of links

* **TinyURL:**
  * One of the first URL shorteners
  * Uses a simpler approach with random string generation
  * Provides custom aliases as a feature

* **Twitter (t.co):**
  * Built for internal use to track links shared on the platform
  * Every link shared on Twitter is automatically shortened
  * Focuses on security scanning and analytics

* **Lessons Learned:**
  * Cache aggressively - URL redirection is read-heavy
  * Plan for data growth - old URLs may need to be archived
  * Security is important - URL shorteners can be used for phishing

---

## **Summary**

* **Key Design Decisions:**
  * Base62 encoding with a distributed counter for generating unique short keys
  * Multi-layered caching strategy to handle high read traffic
  * Horizontally scalable architecture with stateless application servers
  * Sharded database design to handle large data volumes

* **How the Design Meets Requirements:**
  * **Functionality**: Provides URL shortening and redirection with custom aliases and expiration
  * **Performance**: Multi-layer caching ensures fast redirections
  * **Scalability**: Horizontal scaling of all components supports growth
  * **Availability**: Redundancy at all levels minimizes downtime

* **Future Improvements:**
  * Analytics platform for URL usage statistics
  * Machine learning for detecting malicious URLs
  * Geographic routing to reduce latency for global users
  * Archiving solution for expired URLs 