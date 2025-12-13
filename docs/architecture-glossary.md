# Architecture Quick Reference & Glossary

## Quick Decision Framework

### When You Need to Choose an Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Decision Tree                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Team Size < 5? ──YES──> Monolithic or Layered             │
│      │                                                      │
│      NO                                                     │
│      │                                                      │
│      ▼                                                      │
│  Complex Business Logic? ──YES──> Domain-Driven Design     │
│      │                                                      │
│      NO                                                     │
│      │                                                      │
│      ▼                                                      │
│  High Scalability Needs? ──YES──> Microservices            │
│      │                                                      │
│      NO                                                     │
│      │                                                      │
│      ▼                                                      │
│  Real-time Requirements? ──YES──> Event-Driven             │
│      │                                                      │
│      NO                                                     │
│      │                                                      │
│      ▼                                                      │
│  Variable/Unpredictable Load? ──YES──> Serverless          │
│      │                                                      │
│      NO                                                     │
│      │                                                      │
│      ▼                                                      │
│  Start with Layered Architecture                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Essential Patterns Quick Reference

### Communication Patterns

| Pattern | When to Use | Pros | Cons |
|---------|-------------|------|------|
| **Synchronous API** | Need immediate response, simple request-response | Easy to understand, guaranteed response | Can create bottlenecks, tight coupling |
| **Asynchronous Messaging** | Background processing, eventual consistency OK | Better performance, loose coupling | More complex, potential message loss |
| **Event Sourcing** | Audit trail needed, complex business logic | Complete history, replay capability | Storage overhead, complexity |
| **CQRS** | Read/write patterns very different | Optimized queries, independent scaling | Data synchronization complexity |

### Integration Patterns

| Pattern | Best For | Example Use Case |
|---------|----------|------------------|
| **API Gateway** | Multiple services, cross-cutting concerns | Authentication, routing, rate limiting |
| **Service Mesh** | Microservices communication | Traffic management, security, observability |
| **Message Queue** | Decoupled communication | Order processing, notification systems |
| **Event Bus** | Multiple interested parties | User registration triggers (email, analytics, etc.) |

### Data Patterns

| Pattern | When to Use | Trade-offs |
|---------|-------------|------------|
| **Database per Service** | True service independence | Data consistency challenges |
| **Shared Database** | Strong consistency needs | Tight coupling, harder to scale |
| **Data Lake** | Analytics, multiple data sources | Storage efficiency vs. query performance |
| **CQRS** | Different read/write needs | Query optimization vs. complexity |

## Architecture Anti-Patterns Reference

### 🚫 **The Big Ball of Mud**
**Problem:** No clear structure, everything depends on everything
**Solution:** Identify boundaries, refactor incrementally
**Warning Signs:** Hard to understand, changes break unrelated features

### 🚫 **Distributed Monolith**
**Problem:** Services that must be deployed together
**Solution:** Identify true service boundaries, remove sync dependencies
**Warning Signs:** All services need to be updated together

### 🚫 **Chatty Interfaces**
**Problem:** Too many small API calls
**Solution:** Design coarser-grained APIs, batch operations
**Warning Signs:** Network latency dominates response time

### 🚫 **Shared Database**
**Problem:** Multiple services directly accessing same database
**Solution:** Service-owned data, well-defined APIs
**Warning Signs:** Database becomes the API

## Glossary of Architecture Terms

### Core Concepts

**API (Application Programming Interface)**
> The contract between different software components, defining how they can interact.
> *Like:* A restaurant menu - it tells you what you can order, but not how it's made.

**Microservices**
> Small, independent services that communicate over a network to form a larger application.
> *Like:* A food court with different specialized restaurants working together.

**Monolith**
> A single deployable unit containing all functionality of an application.
> *Like:* A traditional department store with everything under one roof.

**Service-Oriented Architecture (SOA)**
> Architecture style where functionality is grouped into services that communicate through well-defined interfaces.
> *Like:* A city with different districts, each with its own purpose but connected by roads.

### Communication & Integration

**Asynchronous Communication**
> Communication where the sender doesn't wait for an immediate response.
> *Like:* Sending an email - you don't wait for the person to read it before doing other things.

**Synchronous Communication**
> Communication where the sender waits for a response before continuing.
> *Like:* A phone call - you wait for the other person to answer before proceeding.

**Event-Driven Architecture**
> System where components communicate by producing and consuming events.
> *Like:* A news system where publishers broadcast news and subscribers listen for topics they care about.

**Message Queue**
> A buffer that stores messages between sender and receiver, allowing asynchronous communication.
> *Like:* A post office mailbox - messages wait there until the recipient picks them up.

**API Gateway**
> A single entry point that routes requests to appropriate services and handles cross-cutting concerns.
> *Like:* A hotel concierge who directs guests to the right services and handles common requests.

**Load Balancer**
> Distributes incoming requests across multiple instances of a service.
> *Like:* A restaurant host who seats customers at different tables to distribute the workload.

### Data & Storage

**Database per Service**
> Pattern where each service owns and manages its own database.
> *Like:* Each department in a company having its own filing system.

**Data Lake**
> Storage repository that holds vast amounts of raw data in its native format.
> *Like:* A warehouse that stores items in their original containers until needed.

**Data Warehouse**
> Structured repository of integrated data from multiple sources, optimized for analysis.
> *Like:* A well-organized library with categorized and indexed information.

**CQRS (Command Query Responsibility Segregation)**
> Pattern that separates read and write operations, potentially using different models.
> *Like:* Having separate windows at a bank - one for deposits/withdrawals, another for account inquiries.

**Event Sourcing**
> Storing changes to application state as a sequence of events rather than just the current state.
> *Like:* Keeping a complete transaction history instead of just your current bank balance.

### Scalability & Performance

**Horizontal Scaling (Scale Out)**
> Adding more servers/instances to handle increased load.
> *Like:* Opening more checkout lanes at a grocery store during busy times.

**Vertical Scaling (Scale Up)**
> Adding more power (CPU, memory) to existing servers.
> *Like:* Training a single cashier to work faster instead of adding more cashiers.

**Caching**
> Storing frequently accessed data in fast storage for quick retrieval.
> *Like:* Keeping your favorite snacks on the kitchen counter instead of in the pantry.

**CDN (Content Delivery Network)**
> Distributed network that delivers content from locations closest to users.
> *Like:* Having local branches of a store instead of one central location.

**Circuit Breaker**
> Pattern that prevents calls to a failing service, allowing it time to recover.
> *Like:* An electrical circuit breaker that stops electricity flow when there's a problem.

### Design Patterns

**Repository Pattern**
> Encapsulates data access logic and provides a uniform interface for accessing data.
> *Like:* A librarian who knows where all the books are stored and retrieves them for you.

**Factory Pattern**
> Creates objects without specifying their exact classes.
> *Like:* A car factory that makes different types of cars based on the order received.

**Observer Pattern**
> Allows objects to notify other objects about changes in their state.
> *Like:* A newsletter subscription where subscribers get notified of new content.

**Adapter Pattern**
> Allows incompatible interfaces to work together.
> *Like:* A power adapter that lets you use a US plug in a European outlet.

### Architecture Styles

**Layered Architecture (N-Tier)**
> Organizes code into horizontal layers, each with specific responsibilities.
> *Like:* A corporate building with different floors for different departments.

**Hexagonal Architecture (Ports & Adapters)**
> Places business logic at the center, with external concerns (UI, database) as adapters around the outside.
> *Like:* A fortress with the important stuff in the center and different gates for different purposes.

**Clean Architecture**
> Architecture that emphasizes separation of concerns and dependency inversion.
> *Like:* A well-organized house where the living areas don't depend on the specific appliances or utilities.

**Domain-Driven Design (DDD)**
> Approach that focuses on modeling software to match business domain.
> *Like:* Organizing a company structure to match how the actual business operates.

### DevOps & Deployment

**Container**
> Lightweight, portable package that includes everything needed to run an application.
> *Like:* A shipping container that can be moved between different trucks, ships, or trains.

**Orchestration**
> Automated management, coordination, and deployment of containerized applications.
> *Like:* A conductor managing all the musicians in an orchestra.

**Blue-Green Deployment**
> Deployment strategy with two identical environments, switching between them for zero-downtime updates.
> *Like:* Having two identical stages, performing on one while preparing the next show on the other.

**Canary Deployment**
> Gradually rolling out changes to a small subset of users before full deployment.
> *Like:* Testing a new recipe with a few customers before putting it on the full menu.

## Quick Architecture Health Check

Use this checklist to evaluate any system:

### 📋 **Structure Health**
- [ ] **Clear boundaries:** Can you easily identify what each component does?
- [ ] **Consistent patterns:** Are similar problems solved in similar ways?
- [ ] **Appropriate abstraction:** Is complexity hidden at the right levels?

### 📋 **Communication Health**
- [ ] **Minimal coupling:** Can components change independently?
- [ ] **Clear interfaces:** Are contracts between components well-defined?
- [ ] **Error handling:** What happens when things go wrong?

### 📋 **Data Health**
- [ ] **Data ownership:** Is it clear which service owns what data?
- [ ] **Consistency strategy:** How is data kept consistent across services?
- [ ] **Performance:** Are data access patterns optimized for usage?

### 📋 **Operational Health**
- [ ] **Observability:** Can you see what the system is doing?
- [ ] **Deployability:** How easy is it to release changes?
- [ ] **Scalability:** Can the system handle growth?

## Common Architecture Smells

### 🔍 **Code Smells at Architecture Level**

**Shotgun Surgery**
> Making a change requires modifications across many services
> *Fix:* Better service boundaries or shared libraries

**Inappropriate Intimacy**
> Services knowing too much about each other's internals
> *Fix:* Better encapsulation and interface design

**Large Class/Service**
> One service doing too many things
> *Fix:* Split responsibilities based on business boundaries

**Data Clumps**
> Same group of data passed around everywhere
> *Fix:* Create proper data structures or service boundaries

## Emergency Architecture Patterns

When things go wrong, these patterns can help:

### 🚨 **Performance Crisis**
1. **Add caching** at multiple levels
2. **Implement circuit breakers** to prevent cascade failures
3. **Scale horizontally** the bottleneck components
4. **Optimize database queries** and add indexes

### 🚨 **Scalability Crisis**
1. **Identify the bottleneck** (usually database or a specific service)
2. **Implement read replicas** for database-bound systems
3. **Add load balancing** for compute-bound systems
4. **Consider asynchronous processing** for non-critical operations

### 🚨 **Reliability Crisis**
1. **Add health checks** and monitoring
2. **Implement graceful degradation** for non-critical features
3. **Set up proper alerting** based on user impact
4. **Create runbooks** for common failure scenarios

---

## Quick Reference Cards

### Architecture Decision Template
```
Context: What situation requires a decision?
Options: What are the viable choices?
Decision: What did we choose?
Rationale: Why did we choose this?
Consequences: What does this mean going forward?
```

### Service Boundary Checklist
```
□ Single Responsibility: Does it do one thing well?
□ Business Alignment: Does it match business functions?
□ Data Ownership: Does it own its data?
□ Team Ownership: Can one team own it?
□ Independent Deployment: Can it be deployed alone?
```

### Integration Pattern Selector
```
Need immediate response? → Synchronous API
Background processing OK? → Message Queue
Multiple subscribers? → Event Bus
Complex routing? → API Gateway
High performance? → Direct service calls
```

[🏠 Return to Main Guide](../README.md)