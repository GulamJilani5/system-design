# Scheduler Services
    These test your ability to manage delayed execution, retries, and time-based workflows reliably.
 ### 1. Design a Web Crawler
     → Handle crawl scheduling, politeness policies, and duplication filtering.
     → Talk about scalability via distributed workers and queueing.
 ### 2. Design a Distributed Task Scheduler
     → Support retries, job priorities, and delayed execution.
     → Think cron triggers, deduplication, and worker orchestration.
 ### 3. Design a Real-Time Notification System
     → Handle push delivery, user targeting, and token lifecycle.
     → Discuss delivery at scale, retries, and cross-device sync.

# Write-Heavy Systems
    These test your ability to ingest, process, and store large volumes of data efficiently.
 ### 4. Design a Rate Limiter
     → Choose between token bucket and leaky bucket models.
     → Discuss stateless vs stateful limits and distributed implementation using Redis.
 ### 5. Design a Log Ingestion System
     → Use Kafka (or similar) for high-throughput collection.
     → Highlight buffering, log processing, and downstream storage solutions.
 ### 6. Design a Collaborative Document Editor (Google Docs)
     → Handle real-time updates using CRDTs or OT algorithms.
     → Discuss synchronization, conflict resolution, and concurrent edits.
 ### 7. Design a Voting or Polling System → Emphasize idempotency and fraud prevention. 
     → Handle real-time vs batch aggregation and analytics.

# Strong Consistency Systems
    Here, correctness trumps speed - you're expected to manage transactions, concurrency, and fault tolerance.
 ### 8. Design an Online Ticket Booking System
    → Talk about seat reservation and conflict resolution (locks, OCC).
    → Walk through atomic operations between booking and payment.
 ### 9. Design an E-Commerce Checkout Flow
    → Handle inventory consistency, cart state, and payment processing.
    → Include order tracking, retries, and failure recovery.
 ### 10. Design an Online Messaging App (WhatsApp/ Slack)
    → Cover message delivery guarantees and ordering.
    → Dive into message queues, acknowledgements, and offline storage.
 ### 11. Design a Task Management Tool (Trello/Asana)
    → Think CRUD APIs, user roles, and real-time collaboration.
    → Discuss background jobs, notifications, and state transitions.

# Read-Heavy Systems
    These are all about serving data fast and at scale - caching, repli…
  ### 13. Design a Content Delivery Platform (YouTube/Netflix)
     → Touch on video storage, CDN architecture, and latency reduction.
     → Consider user-specific recommendations and metadata indexing.
  ### 14. Design a Social Network Feed (Facebook/ Instagram)
     → Dive into fanout strategies and feed generation (Push vs Pull).
     → Talk about timeline ranking, pagination, and denormalized DB design.
  ### 15. Design a Product Catalog (eCommerce) 
     → Cover search indexing, filters, and faceted navigation.
     → Focus on read-optimized DB design and availability under load.
