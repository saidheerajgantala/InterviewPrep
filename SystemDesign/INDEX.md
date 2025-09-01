# System Design Learning Index

This index maps the topics in the roadmap to their corresponding files in the repository.

## 1. Fundamentals
- **Basics**
  - [Client-Server Architecture](Patterns/001_Fundamentals/Basics/Client_Server_Architecture.md)
  - [Network Protocols](Patterns/001_Fundamentals/Basics/Network_Protocols.md)
  - [REST API Design](Patterns/001_Fundamentals/Basics/REST_API_Design.md)
  - [Storage Types](Patterns/001_Fundamentals/Basics/Storage_Types.md)

- **Scalability Concepts**
  - [Vertical vs Horizontal Scaling](Patterns/001_Fundamentals/ScalabilityConcepts/Vertical_vs_Horizontal_Scaling.md)
  - [Load Balancing](Patterns/001_Fundamentals/ScalabilityConcepts/Load_Balancing.md)
  - [Caching Strategies](Patterns/001_Fundamentals/ScalabilityConcepts/Caching_Strategies.md)
  - [Database Sharding & Partitioning](Patterns/001_Fundamentals/ScalabilityConcepts/Database_Sharding_Partitioning.md)

## 2. Core Components
- **Databases**
  - [Relational Databases](Patterns/002_CoreComponents/Databases/Relational_Databases.md)
  - [NoSQL Databases](Patterns/002_CoreComponents/Databases/NoSQL_Databases.md)
  - [Data Replication & Consistency](Patterns/002_CoreComponents/Databases/Data_Replication_Consistency.md)
  - [ACID vs BASE](Patterns/002_CoreComponents/Databases/ACID_vs_BASE.md)

- **Caching**
  - [Application Caching](Patterns/002_CoreComponents/Caching/Application_Caching.md)
  - [Database Caching](Patterns/002_CoreComponents/Caching/Database_Caching.md)
  - [CDN](Patterns/002_CoreComponents/Caching/CDN.md)
  - [Cache Invalidation Strategies](Patterns/002_CoreComponents/Caching/Cache_Invalidation_Strategies.md)

- **Message Queues & Pub/Sub**
  - [Kafka](Patterns/002_CoreComponents/MessageQueues/Kafka.md)
  - [RabbitMQ](Patterns/002_CoreComponents/MessageQueues/RabbitMQ.md)
  - [Event-Driven Architecture](Patterns/002_CoreComponents/MessageQueues/Event_Driven_Architecture.md)
  - [Stream Processing](Patterns/002_CoreComponents/MessageQueues/Stream_Processing.md)

## 3. Distributed Systems
- **Consistency Models**
  - [Strong Consistency](Patterns/003_DistributedSystems/ConsistencyModels/Strong_Consistency.md)
  - [Eventual Consistency](Patterns/003_DistributedSystems/ConsistencyModels/Eventual_Consistency.md)
  - [CAP Theorem](Patterns/003_DistributedSystems/ConsistencyModels/CAP_Theorem.md)
  - [PACELC Theorem](Patterns/003_DistributedSystems/ConsistencyModels/PACELC_Theorem.md)

- **Distributed Coordination**
  - [Consensus Algorithms](Patterns/003_DistributedSystems/DistributedCoordination/Consensus_Algorithms.md)
  - [Leader Election](Patterns/003_DistributedSystems/DistributedCoordination/Leader_Election.md)
  - [Distributed Transactions](Patterns/003_DistributedSystems/DistributedCoordination/Distributed_Transactions.md)
  - [Time and Order](Patterns/003_DistributedSystems/DistributedCoordination/Time_and_Order.md)

- **Fault Tolerance**
  - [Redundancy](Patterns/003_DistributedSystems/FaultTolerance/Redundancy.md)
  - [Replication](Patterns/003_DistributedSystems/FaultTolerance/Replication.md)
  - [Failover Strategies](Patterns/003_DistributedSystems/FaultTolerance/Failover_Strategies.md)
  - [Disaster Recovery](Patterns/003_DistributedSystems/FaultTolerance/Disaster_Recovery.md)

## 4. System Design Patterns
- **Architectural Patterns**
  - [Microservices](Patterns/004_ArchitecturalPatterns/Microservices.md)
  - [Service-Oriented Architecture](Patterns/004_ArchitecturalPatterns/Service_Oriented_Architecture.md)
  - [Event Sourcing](Patterns/004_ArchitecturalPatterns/Event_Sourcing.md)
  - [CQRS](Patterns/004_ArchitecturalPatterns/CQRS.md)

- **Resilience Patterns**
  - [Circuit Breaker](Patterns/005_ResiliencePatterns/Circuit_Breaker.md)
  - [Bulkhead](Patterns/005_ResiliencePatterns/Bulkhead.md)
  - [Retry with Exponential Backoff](Patterns/005_ResiliencePatterns/Retry_with_Backoff.md)
  - [Rate Limiting](Patterns/005_ResiliencePatterns/Rate_Limiting.md)

- **Scalability Patterns**
  - [Sharding](Patterns/006_ScalabilityPatterns/Sharding.md)
  - [Read Replicas](Patterns/006_ScalabilityPatterns/Read_Replicas.md)
  - [Database-per-Service](Patterns/006_ScalabilityPatterns/Database_per_Service.md)

- **Database Patterns**
  - [Sharding](Patterns/007_DatabasePatterns/Sharding.md)  
    (canonical under Database Patterns; see also: Patterns/006_ScalabilityPatterns/Sharding.md)
  - [Read-Write Separation](Patterns/007_DatabasePatterns/Read_Write_Separation.md)
  - [Multi-Model Databases](Patterns/007_DatabasePatterns/Multi_Model.md)
  - [Polyglot Persistence](Patterns/007_DatabasePatterns/Polyglot_Persistence.md)

- **Load Balancing Patterns**
  - [Round Robin](Patterns/008_LoadBalancingPatterns/Round_Robin.md)
  - [Least Connections](Patterns/008_LoadBalancingPatterns/Least_Connections.md)
  - [IP Hash](Patterns/008_LoadBalancingPatterns/IP_Hash.md)
  - [Consistent Hashing](Patterns/008_LoadBalancingPatterns/Consistent_Hashing.md)

- **Messaging Patterns**
  - [Publish-Subscribe](Patterns/009_MessagingPatterns/Publish_Subscribe.md)
  - [Point-to-Point](Patterns/009_MessagingPatterns/Point_To_Point.md)
  - [Request-Response](Patterns/009_MessagingPatterns/Request_Response.md)
  - [Event Sourcing](Patterns/004_ArchitecturalPatterns/Event_Sourcing.md)  
    (canonical under Architectural Patterns; messaging entry links here)

- **Microservices Patterns**
  - [API Gateway](Patterns/010_MicroservicesPatterns/API_Gateway.md)
  - [Service Discovery](Patterns/010_MicroservicesPatterns/Service_Discovery.md)
  - [Saga Pattern](Patterns/010_MicroservicesPatterns/Saga_Pattern.md)
  - [Backend for Frontend](Patterns/010_MicroservicesPatterns/Backend_For_Frontend.md)

## 5. Performance & Optimization
- **Performance Metrics**
  - [Latency](Patterns/011_Performance/Metrics/Latency.md)
  - [Throughput](Patterns/011_Performance/Metrics/Throughput.md)
  - [Availability](Patterns/011_Performance/Metrics/Availability.md)
  - [Consistency](Patterns/011_Performance/Metrics/Consistency.md)

- **Optimization Techniques**
  - [Query Optimization](Patterns/011_Performance/Optimization/Query_Optimization.md)
  - [Connection Pooling](Patterns/011_Performance/Optimization/Connection_Pooling.md)
  - [Data Denormalization](Patterns/011_Performance/Optimization/Data_Denormalization.md)
  - [Asynchronous Processing](Patterns/011_Performance/Optimization/Asynchronous_Processing.md)

## 6. Security
- **Authentication & Authorization**
  - [OAuth 2.0 and OpenID Connect](Patterns/012_Security/AuthenticationAuthorization/OAuth_OpenID.md)
  - [JWT](Patterns/012_Security/AuthenticationAuthorization/JWT.md)
  - [Role-Based Access Control](Patterns/012_Security/AuthenticationAuthorization/RBAC.md)
  - [API Security](Patterns/012_Security/AuthenticationAuthorization/API_Security.md)

- **Data Security**
  - [Encryption](Patterns/012_Security/DataSecurity/Encryption.md)
  - [Data Masking](Patterns/012_Security/DataSecurity/Data_Masking.md)
  - [Secure Storage](Patterns/012_Security/DataSecurity/Secure_Storage.md)
  - [Compliance](Patterns/012_Security/DataSecurity/Compliance.md)

## 7. Advanced Topics
- **Machine Learning Systems**
  - [ML Infrastructure](Patterns/013_AdvancedTopics/MachineLearning/ML_Infrastructure.md)
  - [Feature Stores](Patterns/013_AdvancedTopics/MachineLearning/Feature_Stores.md)
  - [Model Serving](Patterns/013_AdvancedTopics/MachineLearning/Model_Serving.md)
  - [Online Learning](Patterns/013_AdvancedTopics/MachineLearning/Online_Learning.md)

- **Blockchain Systems**
  - [Consensus Mechanisms](Patterns/013_AdvancedTopics/Blockchain/Consensus_Mechanisms.md)
  - [Smart Contracts](Patterns/013_AdvancedTopics/Blockchain/Smart_Contracts.md)
  - [Decentralized Applications](Patterns/013_AdvancedTopics/Blockchain/Decentralized_Applications.md)

- **Edge Computing**
  - [IoT Architectures](Patterns/013_AdvancedTopics/EdgeComputing/IoT_Architectures.md)
  - [Edge-Cloud Coordination](Patterns/013_AdvancedTopics/EdgeComputing/Edge_Cloud_Coordination.md)
  - [5G Networks](Patterns/013_AdvancedTopics/EdgeComputing/5G_Networks.md)

## 8. Case Studies
- **Web Applications**
  - [URL Shortener](CaseStudies/URL_Shortener.md)
  - [Social Media Platform](CaseStudies/WebApplications/Social_Media_Platform.md)
  - [E-commerce Website](CaseStudies/WebApplications/ECommerce_Website.md)

- **Data-Intensive Applications**
  - [Real-time Analytics](CaseStudies/DataIntensiveApplications/Realtime_Analytics.md)
  - [Recommendation Systems](CaseStudies/DataIntensiveApplications/Recommendation_Systems.md)
  - [Search Engines](CaseStudies/DataIntensiveApplications/Search_Engines.md)

- **Infrastructure Services**
  - [Distributed File Storage](CaseStudies/InfrastructureServices/Distributed_File_Storage.md)
  - [Load Balancer Design](CaseStudies/InfrastructureServices/Load_Balancer_Design.md)
  - [Rate Limiter](CaseStudies/InfrastructureServices/Rate_Limiter.md)

- **Database Systems**
  - [Distributed Database](CaseStudies/Databases/Distributed_Database.md)
  - [Time Series Database](CaseStudies/Databases/TimeSeries_Database.md)
  - [Graph Database](CaseStudies/Databases/GraphDatabase.md)

- **Streaming Systems**
  - [Video Streaming](CaseStudies/Streaming/VideoStreaming.md)
  - [Event Streaming](CaseStudies/Streaming/EventStreaming.md)
  - [Real-Time Analytics](CaseStudies/Streaming/RealTimeAnalytics.md)

- **Social Networks**
  - [Instagram](CaseStudies/SocialNetworks/Instagram.md)
  - [Twitter](CaseStudies/SocialNetworks/Twitter.md)
  - [Facebook](CaseStudies/SocialNetworks/Facebook.md)

- **E-Commerce Systems**
  - [Amazon](CaseStudies/ECommerce/Amazon.md)
  - [Payment Processing](CaseStudies/ECommerce/PaymentProcessing.md)
  - [Inventory Management](CaseStudies/ECommerce/InventoryManagement.md)

- **Messaging Systems**
  - [Chat Application](CaseStudies/Messaging/ChatApplication.md)
  - [Notification System](CaseStudies/Messaging/NotificationSystem.md)
  - [Email Delivery System](CaseStudies/Messaging/EmailDeliverySystem.md) 