# Practical Exercises & Real-World Scenarios

## Learning by Doing: Your Architecture Gym

Just like learning to drive, you can read all the theory you want, but real understanding comes from hands-on practice. These exercises will help you apply architectural thinking to realistic scenarios you'll encounter in your career.

## Exercise 1: The Growing Startup Challenge

### Scenario
You're the lead developer at a startup that began with a simple web application. The monolithic system worked great for the first year, but now:

- **User base:** Grew from 1,000 to 100,000 users
- **Team size:** Expanded from 3 to 15 developers
- **Features:** Originally just user profiles, now includes messaging, file sharing, and analytics
- **Performance:** Response times increasing, some features slow
- **Development:** Teams stepping on each other's toes, deployment takes 2 hours

### Your Mission
Design an evolution path from the current monolith to a more scalable architecture.

### Step-by-Step Approach

#### Step 1: Current State Analysis (20 minutes)
Map out the existing monolith:

```
Current Monolithic Structure:
┌─────────────────────────────────────────────────────────┐
│                    Web Application                      │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────┐   │
│  │    User     │ │  Messaging  │ │   File Storage   │   │
│  │   Module    │ │   Module    │ │     Module       │   │
│  └─────────────┘ └─────────────┘ └─────────────────┘   │
│  ┌─────────────────────────────────────────────────┐   │
│  │             Analytics Module                    │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│              Single Database                            │
└─────────────────────────────────────────────────────────┘
```

**Questions to Consider:**
- Which modules change most frequently?
- Which have different scalability needs?
- Where are the natural boundaries?
- What data is shared between modules?

#### Step 2: Identify Service Boundaries (30 minutes)
Based on your analysis, define potential services:

**Suggested Services:**
1. **User Service** - Authentication, profiles, preferences
2. **Messaging Service** - Real-time chat, message history
3. **File Service** - Upload, storage, sharing permissions
4. **Analytics Service** - Usage tracking, reporting

**Boundary Analysis:**
- **User Service:** Core identity, used by all others
- **Messaging Service:** High-frequency writes, real-time needs
- **File Service:** Large data handling, different storage needs
- **Analytics Service:** Read-heavy, different query patterns

#### Step 3: Design Migration Strategy (45 minutes)

**Phase 1: Extract Analytics Service**
- Least critical for core functionality
- Good learning experience
- Can be developed in parallel

**Phase 2: Extract File Service**
- Clear boundaries
- Different infrastructure needs
- Reduces load on main application

**Phase 3: Extract Messaging Service**
- More complex due to real-time requirements
- Requires event-driven patterns

**Phase 4: Optimize User Service**
- Now smaller and more focused
- Can optimize for authentication performance

### Your Solution Template

```markdown
## Service Extraction Plan

### Phase 1: Analytics Service
- **Timeline:** 2-3 weeks
- **Team:** 2 developers
- **Approach:** 
  - Create new service with read-only database replica
  - Dual-write from main application during transition
  - Gradually move analytics queries to new service
- **Risks:** Data consistency during transition
- **Rollback Plan:** Continue using main application

### [Continue for each phase...]

### Communication Strategy
- **Synchronous:** User authentication (critical path)
- **Asynchronous:** Analytics events, file processing notifications
- **Data Consistency:** Eventual consistency for analytics, strong consistency for user data

### Technology Choices
- **API Gateway:** [Your choice and reasoning]
- **Message Queue:** [Your choice and reasoning]
- **Databases:** [Shared vs. separate, and why]
```

## Exercise 2: The E-Commerce Platform Architecture

### Scenario
Design the architecture for an e-commerce platform that needs to handle:

- **Peak traffic:** 10,000 concurrent users during flash sales
- **Catalog:** 1 million products across thousands of categories
- **Geography:** Customers in 20+ countries
- **Integrations:** Payment processors, shipping providers, inventory systems
- **Mobile:** 70% of traffic comes from mobile apps

### Key Requirements
1. **Performance:** Sub-200ms response times for product searches
2. **Availability:** 99.9% uptime (max 8.7 hours downtime per year)
3. **Scalability:** Handle 10x traffic spikes during sales events
4. **Consistency:** Inventory must be accurate, no overselling
5. **Security:** PCI compliance for payment processing

### Your Architecture Challenge

#### Step 1: Service Identification (30 minutes)
Identify the core services needed:

**Core Services:**
- User Management
- Product Catalog
- Inventory Management
- Shopping Cart
- Order Processing
- Payment Processing
- Shipping Management
- Notification Service

**For each service, define:**
- Primary responsibilities
- Data it owns
- Key performance requirements
- Integration needs

#### Step 2: Data Architecture (45 minutes)
Design the data strategy:

**Questions to Address:**
- Which services need their own databases?
- How will you handle data consistency between services?
- What caching strategies will you use?
- How will you handle inventory updates during high traffic?

**Example Data Flow for "Add to Cart":**
```
1. User clicks "Add to Cart"
2. Frontend → API Gateway → Cart Service
3. Cart Service checks Inventory Service
4. If available, cart updated
5. Inventory reserved (with timeout)
6. Response to user
```

#### Step 3: Handling Peak Traffic (30 minutes)
Design for scalability:

**Strategies to Consider:**
- Read replicas for product catalog
- Caching layers (Redis, CDN)
- Async processing for non-critical operations
- Circuit breakers for external services
- Auto-scaling policies

### Advanced Scenarios

#### Scenario A: Flash Sale Management
During a flash sale for limited inventory:
- How do you prevent overselling?
- How do you handle the traffic spike?
- What happens if services go down during the sale?

#### Scenario B: Global Expansion
Expanding to new countries with different:
- Currencies and payment methods
- Shipping regulations
- Tax requirements
- Language and localization needs

## Exercise 3: Legacy System Modernization

### Background
You've inherited a 10-year-old system that's critical to the business but becoming increasingly difficult to maintain:

- **Technology:** Written in older framework, database schema with 200+ tables
- **Documentation:** Minimal, many original developers have left
- **Performance:** Slow queries, long deployment times
- **Business Impact:** New features take months to implement
- **Risk:** System outages directly impact revenue

### The Challenge
Create a modernization strategy that:
- Doesn't disrupt ongoing business operations
- Allows parallel development of new features
- Reduces technical debt incrementally
- Provides measurable improvements

### Step-by-Step Modernization

#### Phase 1: Understanding and Stabilization (4-6 weeks)
1. **System Analysis:**
   - Map current system boundaries
   - Identify critical business processes
   - Document data flows
   - Catalog technical debt

2. **Risk Assessment:**
   - Identify single points of failure
   - Prioritize by business impact
   - Create monitoring and alerting

3. **Quick Wins:**
   - Performance optimizations
   - Database query improvements
   - Basic monitoring setup

#### Phase 2: Strangler Fig Pattern (3-6 months)
Gradually replace parts of the legacy system:

```
Legacy System → API Facade → New Services
                     ↓
              Route requests to:
              • Legacy (for old features)
              • New services (for new features)
```

**Implementation Strategy:**
- Start with least risky components
- Implement new features as microservices
- Gradually migrate existing features
- Maintain data consistency during transition

### Your Modernization Plan Template

```markdown
# Legacy Modernization Strategy

## Current State Assessment
- **Technology Stack:** [List current technologies]
- **Pain Points:** [Ranked by severity]
- **Business Dependencies:** [Critical features that cannot go down]

## Modernization Phases

### Phase 1: Stabilization
- **Goals:** [What you want to achieve]
- **Actions:** [Specific steps]
- **Success Metrics:** [How you'll measure success]
- **Timeline:** [Realistic timeframe]

### Phase 2: Parallel Development
- **New Architecture:** [Target state]
- **Migration Strategy:** [How you'll move features]
- **Risk Mitigation:** [What could go wrong and how to handle it]

### Phase 3: Legacy Retirement
- **Decommissioning Plan:** [How to safely remove old system]
- **Data Migration:** [How to handle historical data]
- **Rollback Strategy:** [What if something goes wrong]
```

## Exercise 4: Performance Architecture Challenge

### The Problem
Your successful social media application is experiencing performance issues:

- **Feed Loading:** Takes 3-5 seconds for users with many followers
- **Image Upload:** Sometimes times out on mobile connections
- **Search:** Slow for popular keywords
- **Notifications:** Delayed by several minutes
- **Peak Usage:** 7-9 PM sees 300% traffic increase

### Performance Optimization Strategy

#### Step 1: Identify Bottlenecks (30 minutes)
For each performance issue, analyze:

**Feed Loading Analysis:**
- Database queries: Are we fetching too much data?
- N+1 problems: Multiple queries per feed item?
- Caching: Are we recalculating feeds every time?
- Data structure: Is our schema optimized for reads?

**Your Task:** Apply similar analysis to each problem area.

#### Step 2: Architecture Solutions (45 minutes)

**Feed Performance Solutions:**
```
Option 1: Pre-computed Feeds
User posts → Background job → Pre-compute all follower feeds → Cache

Option 2: Real-time Assembly with Smart Caching  
Request → Check cache → Assemble missing pieces → Cache result

Option 3: Hybrid Approach
Popular users → Pre-computed feeds
Regular users → Real-time assembly
```

**Image Upload Solutions:**
- Async processing pipeline
- Progressive upload with resumption
- Multiple image sizes generated in background
- CDN integration for global delivery

#### Step 3: Implementation Priority (20 minutes)
Rank solutions by:
- Impact on user experience
- Implementation complexity
- Resource requirements
- Risk level

## Reflection Questions

After completing these exercises, consider:

1. **Pattern Recognition:** Which architectural patterns appeared across multiple exercises?

2. **Trade-off Analysis:** What were the common trade-offs you had to consider?

3. **Decision Criteria:** What factors most influenced your architectural decisions?

4. **Evolution Thinking:** How did you balance current needs with future flexibility?

5. **Risk Management:** How did you handle uncertainty and potential failures?

## Next Steps: Building Your Architecture Toolkit

Based on your exercise experience, create your personal architecture decision framework:

### Decision Framework Template
```markdown
# My Architecture Decision Framework

## Key Questions to Always Ask
1. [Your top question based on exercises]
2. [Second most important consideration]
3. [Continue...]

## Common Patterns That Work
- [Pattern 1]: Good for [scenario] because [reason]
- [Pattern 2]: Good for [scenario] because [reason]

## Red Flags to Watch For
- [Warning sign 1]: Usually indicates [problem]
- [Warning sign 2]: Usually indicates [problem]

## My Go-To Solutions
For [common scenario]: I typically choose [solution] because [reasoning]
```

---

## Key Takeaways
- Architecture decisions are about trade-offs, not right/wrong answers
- Context matters more than following rigid patterns
- Evolution and migration are often more important than perfect initial design
- Performance and scalability problems have predictable patterns and solutions
- Real-world constraints (time, budget, team skills) heavily influence decisions

[Return to Main Guide](../README.md) | [Continue to Resources & Next Steps →](06-resources-next-steps.md)