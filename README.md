# 🚀 Event-Driven & Messaging Architecture
Advanced projects demonstrating asynchronous communication patterns, message brokers, 
and event-driven microservices architecture using Apache Kafka and RabbitMQ.

---

## 📚 Projects Overview

### Foundation Projects (Learn the Basics)
These projects introduce the fundamental concepts of event-driven architecture and message brokers.

#### 1. **[kafka-tutorial](https://github.com/Cortadai/kafka-tutorial)**
   Introduction to Apache Kafka with Spring Boot
   - **Technology:** Spring Boot 3.1.8, Apache Kafka, Java
   - **Focus:** Producer-Consumer pattern, message serialization
   - **Key Concepts:**
     - Kafka topics, partitions, and consumer groups
     - Publishing simple text messages
     - Publishing/consuming JSON objects (User DTOs)
     - Spring Kafka integration (@KafkaListener)
   - **REST Endpoints:**
     - `GET /api/v1/kafka/publish?message=<text>` - Publish text messages
     - `POST /api/v1/kafka/publish/user` - Publish JSON messages
   - **Learning Outcome:** Understand Kafka basics and Spring Kafka integration
   - **Perfect For:** Beginners starting with event-driven architecture
   
   **Example Flow:**
   ```
   Client → Spring Boot App → Kafka Topic → Consumer Listener → Log Output
   ```

#### 2. **[rabbitmq-tutorial](https://github.com/Cortadai/rabbitmq-tutorial)**
   Introduction to RabbitMQ with Spring Boot
   - **Technology:** Spring Boot 3.x, RabbitMQ (AMQP), Java
   - **Focus:** Message broker patterns, Topic Exchange, routing
   - **Key Concepts:**
     - RabbitMQ exchanges (Topic Exchange)
     - Queue bindings and routing keys
     - Producer-Consumer pattern with @RabbitListener
     - Message serialization (text and JSON)
   - **REST Endpoints:**
     - Text messaging endpoints
     - JSON object messaging endpoints
   - **Learning Outcome:** Master RabbitMQ message broker concepts
   - **Perfect For:** Developers learning AMQP and message queuing
   
   **Architecture Pattern:**
   ```
   Producer → Exchange → Queue (with routing key) → Consumer Listener
   ```

#### 3. **[kafka-server-local](https://github.com/Cortadai/kafka-server-local)**
   Apache Kafka 3.6.1 complete installation for local development
   - **Technology:** Apache Kafka, ZooKeeper, Kafka Connect
   - **Contents:**
     - Complete Kafka broker setup
     - ZooKeeper configuration
     - Kafka Connect for integrations
     - Admin scripts and tools (/bin directory)
     - Configuration files (/config directory)
     - All necessary libraries (/libs directory)
   - **Purpose:** Ready-to-run Kafka environment
   - **Use Case:** Local development, testing, experimentation
   - **Includes:** Single-node cluster, producer/consumer scripts, monitoring tools
   - **Learning Outcome:** Understand Kafka infrastructure and deployment
   - **Perfect For:** DevOps engineers, infrastructure setup

---

### Intermediate Projects (Microservices with Events)
These projects show how to build microservices that communicate asynchronously through events.

#### 4. **[springboot-microservices-kafka](https://github.com/Cortadai/springboot-microservices-kafka)**
   Microservices architecture using Apache Kafka for async communication
   - **Technology:** Spring Boot 3.x, Apache Kafka, Java 17, Docker
   - **Architecture:** Event-Driven Microservices
   - **Services:**
     1. **Order Service** (Microservice 1)
        - Exposes REST API: `POST /api/v1/orders`
        - Receives order requests
        - Generates UUID for tracking
        - Creates `OrderEventDto` with status "PENDING"
        - Publishes event to Kafka topic "orders"
     
     2. **Email Service** (Consumer)
        - Listens to "orders" topic
        - Sends email notifications to customers
        - Decoupled from Order Service
     
     3. **Stock Service** (Consumer)
        - Listens to "orders" topic
        - Updates inventory/stock
        - Persists to database
        - Independent scaling and deployment
   
   - **Shared Module:** base-domain (DTOs: OrderDto, OrderEventDto)
   
   - **Key Concepts:**
     - Asynchronous event publishing
     - Multiple independent consumers
     - Event payload standardization
     - Decoupled microservices
     - Kafka topic partitioning
   
   - **Communication Flow:**
   ```
   REST Client → Order Service → Kafka Topic (orders) → Email Service & Stock Service
                                                          ↓                ↓
                                                       Send Email      Update Stock
   ```
   
   - **Benefits:**
     - ✅ Services are fully decoupled
     - ✅ Each service scales independently
     - ✅ Resilient to service failures
     - ✅ Supports adding new consumers without changing Order Service
   
   - **Learning Outcome:** Build event-driven microservices with Kafka
   - **Perfect For:** Understanding async communication between services

#### 5. **[springboot-microservices-rabbitmq](https://github.com/Cortadai/springboot-microservices-rabbitmq)**
   Microservices architecture using RabbitMQ for async communication
   - **Technology:** Spring Boot 3.x, RabbitMQ (AMQP), Java 17, Docker
   - **Architecture:** Event-Driven Microservices (Alternative to Kafka)
   - **Services:**
     1. **Order Service** (Port 8080)
        - REST API: `POST /api/v1/orders`
        - Publishes order events to RabbitMQ TopicExchange
        - Generates events with status
     
     2. **Stock Service** (Port 8081)
        - Consumes order events
        - Updates inventory
        - Persists changes
     
     3. **Email Service** (Port 8082)
        - Consumes order events
        - Sends confirmation emails
   
   - **RabbitMQ Components:**
     - TopicExchange for flexible routing
     - Multiple queues bound to exchange
     - Routing keys for message distribution
   
   - **Key Concepts:**
     - RabbitMQ vs Kafka differences
     - TopicExchange patterns
     - Queue binding strategies
     - Spring AMQP integration
   
   - **Communication Flow:**
   ```
   REST Client → Order Service → TopicExchange → Stock & Email Services
                                     ↓
                            Distributed via Routing Keys
   ```
   
   - **When to Use RabbitMQ vs Kafka:**
     - RabbitMQ: Smaller systems, flexible routing, traditional message queue needs
     - Kafka: High-volume streaming, event sourcing, log-based systems
   
   - **Learning Outcome:** Compare Kafka and RabbitMQ approaches
   - **Perfect For:** Understanding alternative event broker architectures

---

### Advanced Projects (Real-World Scenarios)
Production-grade examples demonstrating complex event-driven patterns.

#### 6. **[springboot-kafka-real-world-project](https://github.com/Cortadai/springboot-kafka-real-world-project)**
   Real-world event streaming pipeline: Wikimedia → Kafka → Database
   - **Technology:** Spring Boot, Apache Kafka, Server-Sent Events (SSE)
   - **Architecture:** Complete streaming pipeline
   - **Components:**
     1. **Kafka Producer Module** (kafka-producer-wikimedia)
        - Connects to Wikimedia EventStreams (live Wikipedia changes)
        - Uses Server-Sent Events (SSE) protocol
        - Publishes events continuously to Kafka topic
        - Events: page creations, edits, deletions, user actions
     
     2. **Kafka Consumer Module** (kafka-consumer-database)
        - Listens to Kafka topic
        - Receives real-time Wikipedia change events
        - Persists to database using Spring Data JPA
        - Stores complete event history
   
   - **Real-World Data Source:**
     - Wikimedia EventStreams API (public, no auth required)
     - Live events from Wikipedia in real-time
     - Thousands of events per minute
   
   - **Event Examples:**
     ```
     - Page "Machine Learning" edited by user "Alice"
     - New page "AI Safety" created by user "Bob"
     - Page "Python Programming" reverted by admin
     ```
   
   - **Data Pipeline:**
   ```
   Wikimedia → SSE Stream → Kafka Producer → Kafka Topic → Kafka Consumer → Database
                (External)      (Ingestion)    (Buffer)      (Ingestion)     (Storage)
   ```
   
   - **Key Concepts:**
     - Real-time event ingestion
     - Producer-consumer pipeline
     - Handling high-volume events
     - Event persistence
     - Stream processing basics
   
   - **Challenges Solved:**
     - ✅ Buffering with Kafka
     - ✅ Handling backpressure
     - ✅ Event persistence
     - ✅ Error handling
     - ✅ Scalable ingestion
   
   - **Learning Outcome:** Build production-grade event streaming systems
   - **Perfect For:** Understanding real-world event pipelines

---

## 🏗️ Architecture Patterns

### Pattern 1: Simple Producer-Consumer (Tutorials)
```
┌─────────────────────────────────────────┐
│           Producer                      │
│  (Publishes messages to topic)          │
├─────────────────────────────────────────┤
│  REST Endpoint receives request        │
│  → Creates message                     │
│  → Publishes to Kafka/RabbitMQ         │
│  → Returns immediately                 │
└──────────────┬──────────────────────────┘
               │
               ▼
┌──────────────────────────┐
│   Kafka/RabbitMQ Topic   │
│   (Distributed Buffer)   │
└──────────────┬───────────┘
               │
     ┌─────────┴──────────┐
     ▼                    ▼
┌──────────────┐    ┌──────────────┐
│  Consumer 1  │    │  Consumer 2  │
│ (Listener)   │    │ (Listener)   │
│              │    │              │
│ Log Message  │    │ Save to DB   │
└──────────────┘    └──────────────┘
```

### Pattern 2: Event-Driven Microservices
```
┌────────────────────────────────────────┐
│         Client Request                 │
│   POST /api/v1/orders                  │
└────────────────┬───────────────────────┘
                 │
                 ▼
        ┌──────────────────┐
        │  Order Service   │
        │  (Producer)      │
        └────────┬─────────┘
                 │
      Publishes: OrderCreatedEvent
                 │
                 ▼
        ┌──────────────────┐
        │  Message Broker  │
        │  (Kafka/AMQP)    │
        └────────┬─────────┘
                 │
      ┌──────────┼──────────┐
      │          │          │
      ▼          ▼          ▼
┌──────────┐ ┌──────────┐ ┌────────────┐
│  Email   │ │  Stock   │ │ Analytics  │
│ Service  │ │ Service  │ │ Service    │
│(Consumer)│ │(Consumer)│ │(Consumer)  │
└──────────┘ └──────────┘ └────────────┘
    │            │             │
    └────┬───────┴─────────────┘
         │
    All react independently to same event
    No direct coupling
```

### Pattern 3: Real-World Streaming Pipeline
```
┌──────────────────────┐
│   External Source    │
│  (Wikimedia API)     │
│  - High volume       │
│  - Continuous stream │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────┐
│   Kafka Producer App    │
│  - SSE connection       │
│  - Event parsing        │
│  - Kafka publishing     │
└──────────┬──────────────┘
           │
    High-volume events
           │
           ▼
┌──────────────────────────┐
│   Kafka Cluster         │
│  - Partitioned topics   │
│  - Distributed buffer   │
│  - Fault tolerance      │
└──────────┬──────────────┘
           │
     Multiple partitions
           │
           ▼
┌──────────────────────────┐
│   Kafka Consumer App    │
│  - Event deserialization│
│  - Data transformation  │
│  - Database persistence │
└──────────┬──────────────┘
           │
           ▼
┌──────────────────────────┐
│   Database              │
│  - Event archive        │
│  - Historical data      │
│  - Analytics source     │
└──────────────────────────┘
```

---

## 🛠️ Technology Stack

### Message Brokers
- **Apache Kafka** - Distributed streaming platform
  - High throughput (millions of events/sec)
  - Persistent log-based storage
  - Consumer group support
  - Partitioning for scalability
  - Best for: Event sourcing, real-time streaming, audit logs
  
- **RabbitMQ** - Message broker with AMQP
  - Flexible routing (Exchanges)
  - Queue-based messaging
  - Acknowledgment guarantees
  - Better for complex routing scenarios
  - Best for: Traditional queuing, microservices, job distribution

### Spring Ecosystem
- **Spring Boot** - Application framework
- **Spring Kafka** - Kafka integration
- **Spring AMQP** - RabbitMQ integration
- **Spring Data JPA** - Persistence layer

### Infrastructure
- **Docker & Docker Compose** - Containerization and orchestration
- **Java 17+** - Programming language
- **Maven** - Build tool
- **ZooKeeper** - Kafka coordination (included with Kafka)

---

## 📊 Comparison Matrix

| Feature | Kafka Tutorial | RabbitMQ Tutorial | Kafka Microservices | RabbitMQ Microservices | Real-World Pipeline |
|---------|---|---|---|---|---|
| **Broker** | Kafka | RabbitMQ | Kafka | RabbitMQ | Kafka |
| **Pattern** | Producer-Consumer | Producer-Consumer | Event-Driven Services | Event-Driven Services | Streaming Pipeline |
| **Services** | 1 app | 1 app | 3+ microservices | 3 microservices | 2 apps |
| **Complexity** | Beginner | Beginner | Intermediate | Intermediate | Advanced |
| **Data Source** | Manual | Manual | Internal (orders) | Internal (orders) | External (Wikimedia) |
| **Scale** | Small | Small | Medium | Medium | Large (high volume) |
| **Use Case** | Learning | Learning | Production-ready | Production-ready | Real production |
| **Learning Time** | 1-2 days | 1-2 days | 1 week | 1 week | 2+ weeks |

---

## 🚀 Quick Start Guides

### Getting Started with Kafka
```bash
# 1. Start Kafka locally using kafka-server-local setup
cd kafka-server-local
# Follow the setup instructions

# 2. Run Kafka Tutorial
git clone https://github.com/Cortadai/kafka-tutorial.git
cd kafka-tutorial
mvn clean install
mvn spring-boot:run

# 3. Publish a message
curl "http://localhost:8080/api/v1/kafka/publish?message=Hello%20Kafka"

# 4. Check console for message consumption
```

### Getting Started with RabbitMQ
```bash
# 1. Start RabbitMQ (Docker)
docker run -d --name rabbitmq \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management

# 2. Access RabbitMQ Management UI
# Open browser: http://localhost:15672
# Username: guest
# Password: guest

# 3. Run RabbitMQ Tutorial
git clone https://github.com/Cortadai/rabbitmq-tutorial.git
cd rabbitmq-tutorial
mvn clean install
mvn spring-boot:run

# 4. Publish a message via REST
curl -X POST http://localhost:8080/api/v1/rabbitmq/publish \
  -H "Content-Type: application/json" \
  -d '{"id":1,"firstName":"John","lastName":"Doe"}'
```

### Running Microservices with Docker Compose
```bash
# For Kafka Microservices
git clone https://github.com/Cortadai/springboot-microservices-kafka.git
cd springboot-microservices-kafka
docker-compose up -d

# Services available at:
# - Order Service: http://localhost:8080
# - Kafka: localhost:9092
```

---

## 🎯 Learning Path

### Week 1: Foundations
#### Day 1-2: Message Broker Basics
- What are message brokers?
- Synchronous vs Asynchronous communication
- Event-driven architecture basics
- Kafka vs RabbitMQ comparison

#### Day 3-4: Apache Kafka Deep Dive
- Topics, partitions, and replicas
- Producers and consumers
- Consumer groups
- Spring Kafka integration
- **Hands-on:** Run kafka-tutorial

#### Day 5: RabbitMQ Concepts
- AMQP protocol
- Exchanges (Topic, Fanout, Direct)
- Queue bindings
- Spring AMQP integration
- **Hands-on:** Run rabbitmq-tutorial

### Week 2: Microservices
#### Day 6-7: Event-Driven Microservices
- Service decoupling
- Event publishing patterns
- Multiple consumers
- Handling failures
- **Hands-on:** Run kafka-microservices

#### Day 8-9: Alternative Implementations
- RabbitMQ microservices comparison
- When to use each broker
- Trade-offs analysis
- **Hands-on:** Run rabbitmq-microservices

#### Day 10: Real-World Patterns
- Streaming pipelines
- Event sourcing
- CQRS patterns
- **Hands-on:** Review real-world-project

### Week 3: Advanced Topics
#### Day 11-12: Production Considerations
- Scaling strategies
- Monitoring and observability
- Error handling and retries
- Data consistency
- Disaster recovery

#### Day 13-14: Integration & Operations
- Multi-broker setups
- Kafka Streams
- Kafka Connect
- Monitoring with Prometheus/Grafana

#### Day 15: Capstone Project
- Design and implement your own event-driven system
- Choose between Kafka and RabbitMQ
- Implement multiple consumers
- Handle failures gracefully

---

## 🔗 Key Concepts Explained

### Event (Message)
```java
// A piece of data that represents something that happened
public class OrderCreatedEvent {
    private UUID orderId;           // Unique identifier
    private LocalDateTime timestamp; // When it happened
    private String status;          // Current state
    private OrderDto order;         // Associated data
}
```

### Topic (Kafka) / Exchange (RabbitMQ)
- **Kafka Topic:** Named channel where events are published
- **RabbitMQ Exchange:** Router that distributes messages to queues
- Purpose: Decouple producers from consumers

### Consumer Group (Kafka Specific)
- Multiple consumers can subscribe to same topic
- Each consumer group gets all messages
- Messages are partitioned across group members
- Enables scaling and fault tolerance

### Routing Key (RabbitMQ Specific)
- Metadata used by exchanges to route messages
- TopicExchange supports wildcard patterns
- Example: `order.*.confirmed` matches `order.payment.confirmed`

### Partition (Kafka Specific)
- Topics are split into partitions
- Each partition is ordered independently
- Different consumers can read different partitions
- Enables horizontal scaling

---

## 📈 When to Use Each Broker

### Use Kafka When:
- ✅ High-volume event streaming (millions/sec)
- ✅ Need to replay events (immutable log)
- ✅ Building event sourcing systems
- ✅ Implementing CQRS
- ✅ Long-term data retention required
- ✅ Complex stream processing needed
- ✅ Building data pipelines

### Use RabbitMQ When:
- ✅ Complex routing logic needed
- ✅ Traditional message queuing patterns
- ✅ Moderate message volume
- ✅ Simpler setup and operations
- ✅ Task distribution (job queue pattern)
- ✅ Need built-in retry mechanisms
- ✅ Flexible exchange types (Topic, Direct, Fanout)

---

## 🧪 Testing Event-Driven Systems

### Unit Testing Producers
```java
@Test
void testOrderPublishing() {
    // Arrange
    Order order = new Order(...);
    
    // Act
    orderService.createOrder(order);
    
    // Assert
    verify(kafkaTemplate).send(eq("orders-topic"), any(OrderEvent.class));
}
```

### Integration Testing Consumers
```java
@SpringBootTest
@EmbeddedKafka(partitions = 1, brokerProperties = {
    "listeners=PLAINTEXT://localhost:9092",
    "port=9092"
})
class OrderConsumerTest {
    
    @Test
    void testOrderEventProcessing() {
        // Send event to embedded Kafka
        // Verify consumer processed it
        // Assert database state changed
    }
}
```

---

## 💡 Best Practices

### 1. Event Design
- ✅ Include event ID and timestamp
- ✅ Use semantic versioning for events
- ✅ Make events immutable
- ✅ Include all necessary context

### 2. Consumer Reliability
- ✅ Implement idempotency
- ✅ Handle redelivery gracefully
- ✅ Use DLQ (Dead Letter Queue) for failures
- ✅ Log all important events

### 3. Scalability
- ✅ Use partitions to scale consumption
- ✅ Monitor consumer lag
- ✅ Implement backpressure handling
- ✅ Use consumer groups for parallel processing

### 4. Monitoring
- ✅ Track message throughput
- ✅ Monitor consumer lag
- ✅ Alert on failures
- ✅ Log all events for audit trail

### 5. Error Handling
- ✅ Implement retry logic
- ✅ Use Dead Letter Queues
- ✅ Circuit breakers for failures
- ✅ Graceful degradation

---

## 📚 Related Collections
- [Spring Boot Basics](https://github.com/Cortadai/spring-boot-basics) - REST API foundations
- [Microservices Architecture](https://github.com/Cortadai/microservices-architecture) - Complete microservices setup
- [Spring Security Course](https://github.com/Cortadai/spring-security-course) - Securing event systems

---

## 🏷️ Topics Applied
All projects tagged with:
- `#event-driven` - Event-driven architecture
- `#messaging` - Message brokers
- `#microservices` - Microservices patterns
- `#learning` - Educational material
- `#tutorial` - Tutorial style

Specific topics per project:
- Kafka projects: `#kafka`, `#streaming`
- RabbitMQ projects: `#rabbitmq`, `#amqp`
- Microservices: `#spring-boot`, `#distributed-systems`
- Real-world: `#production`, `#real-world-project`

---

## 📊 Project Stats
| Metric | Value |
|--------|-------|
| **Total Projects** | 6 |
| **Message Brokers** | 2 (Kafka, RabbitMQ) |
| **Microservices** | 6 (across all projects) |
| **Learning Level** | Beginner to Advanced |
| **Estimated Learning Time** | 3-4 weeks |

---

## 🎓 Learning Outcomes
After completing this collection, you'll understand:
- ✅ Asynchronous event-driven architecture
- ✅ Apache Kafka concepts and usage
- ✅ RabbitMQ message brokering
- ✅ Producer-consumer patterns
- ✅ Building event-driven microservices
- ✅ Real-time event streaming
- ✅ Handling high-volume events
- ✅ Event persistence and audit trails
- ✅ Consumer groups and scaling
- ✅ Error handling in async systems
- ✅ Comparing broker architectures
- ✅ Production deployment patterns

---

## 📬 Additional Resources

### Official Documentation
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [RabbitMQ Documentation](https://www.rabbitmq.com/documentation.html)
- [Spring Kafka Documentation](https://spring.io/projects/spring-kafka)
- [Spring AMQP Documentation](https://spring.io/projects/spring-amqp)

### Tutorials & Guides
- [Kafka Quick Start](https://kafka.apache.org/quickstart)
- [RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)
- [Event-Driven Architecture Patterns](https://www.eventdrivenarchitecture.io/)

### Tools & Utilities
- [Kafdrop](https://github.com/obsidiandynamics/kafdrop) - Kafka UI
- [RabbitMQ Management UI](https://www.rabbitmq.com/management.html)
- [Kafka Cat (kcat)](https://github.com/edenhill/kcat) - Command-line tool

---

## 🎯 Next Steps
1. **Start with tutorials:** kafka-tutorial and rabbitmq-tutorial
2. **Understand basics:** Message brokers, producers, consumers
3. **Move to microservices:** See how multiple services communicate
4. **Explore real-world:** Learn from kafka-real-world-project
5. **Build your own:** Create an event-driven application
6. **Optimize:** Learn scaling, monitoring, and operations

---

*Last updated: November 2025*
*Hub: Event-Driven & Messaging Architecture v1.0*
