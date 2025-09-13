# Course Search API - Interview Questions

This document contains a comprehensive set of interview questions based on the Course Search API repository. The questions are categorized by difficulty level and technical domain to help assess candidates at different levels.

## Table of Contents
1. [Junior Level Questions](#junior-level-questions)
2. [Mid-Level Questions](#mid-level-questions)
3. [Senior Level Questions](#senior-level-questions)
4. [System Design Questions](#system-design-questions)
5. [Practical Coding Questions](#practical-coding-questions)

---

## Junior Level Questions

### Spring Boot Fundamentals

**Q1:** What is Spring Boot and why is it used in this project?
- **Expected Answer:** Spring Boot is a framework that simplifies Spring application development by providing auto-configuration, embedded servers, and production-ready features. In this project, it's used to create a REST API with minimal configuration.

**Q2:** Explain the purpose of the `@RestController` annotation in `CourseSearchController`.
- **Expected Answer:** `@RestController` combines `@Controller` and `@ResponseBody`, indicating that this class handles HTTP requests and returns data directly in the response body (typically JSON) rather than returning view names.

**Q3:** What does the `@RequestParam` annotation do in the search endpoint?
- **Expected Answer:** `@RequestParam` binds HTTP request parameters to method parameters. In this case, it extracts query parameters like `q`, `category`, `type`, etc., from the URL.

**Q4:** What is the difference between `@RequestParam(required = false)` and `@RequestParam(defaultValue = "0")`?
- **Expected Answer:** 
  - `required = false` makes the parameter optional
  - `defaultValue = "0"` makes the parameter optional and provides a default value if not provided

### Basic Java & Lombok

**Q5:** What is the purpose of the `@Data` annotation in `CourseDocument`?
- **Expected Answer:** `@Data` is a Lombok annotation that automatically generates getters, setters, `toString()`, `equals()`, and `hashCode()` methods, reducing boilerplate code.

**Q6:** Explain the data types used in `CourseDocument` and why they were chosen.
- **Expected Answer:** 
  - `String` for text fields (id, title, description)
  - `Integer` for numeric ranges (minAge, maxAge)
  - `Double` for price (supports decimal values)
  - `ZonedDateTime` for date handling with timezone support

### Basic Elasticsearch

**Q7:** What is the purpose of the `@Document(indexName = "courses")` annotation?
- **Expected Answer:** It marks the class as an Elasticsearch document and specifies that instances will be stored in the "courses" index.

**Q8:** What's the difference between `FieldType.Text` and `FieldType.Keyword` in Elasticsearch?
- **Expected Answer:**
  - `Text` fields are analyzed and used for full-text search
  - `Keyword` fields are not analyzed and used for exact matching, filtering, and aggregations

---

## Mid-Level Questions

### Spring Data Elasticsearch

**Q9:** Explain how the search functionality works in `CourseSearchService`. Walk through the query building process.
- **Expected Answer:** The service uses `Criteria` API to build complex queries dynamically. It starts with an empty criteria and adds conditions based on provided parameters using methods like `matches()`, `is()`, `greaterThanEqual()`, etc.

**Q10:** What is the purpose of fuzzy matching in the search, and how is it implemented?
- **Expected Answer:** Fuzzy matching allows for typo tolerance. The fuzziness is dynamically set based on query length (1 for short queries, 2 for longer ones) to balance accuracy and tolerance.

**Q11:** How does the pagination work in this API, and what are the benefits?
- **Expected Answer:** Pagination uses `PageRequest.of(page, size, sort)` to limit results. Benefits include reduced memory usage, better performance, and improved user experience by loading data in chunks.

### REST API Design

**Q12:** Evaluate the API design of the search endpoint. What are its strengths and potential improvements?
- **Expected Answer:**
  - **Strengths:** RESTful design, comprehensive filtering, proper HTTP methods
  - **Improvements:** Could add response status codes, error handling, API versioning, input validation

**Q13:** How would you handle validation for the search parameters?
- **Expected Answer:** Use `@Valid` annotation with custom validation classes, or implement manual validation in the service layer. For example, validate date formats, ensure minAge ≤ maxAge, positive prices, etc.

### Error Handling & Best Practices

**Q14:** What issues do you see with the current error handling in `CourseSearchService`?
- **Expected Answer:** 
  - Date parsing errors are only logged to console, not returned to client
  - No proper exception handling
  - Should return meaningful error messages to API consumers

**Q15:** How would you improve the logging in this application?
- **Expected Answer:** Use proper logging framework (SLF4J), add structured logging, include request IDs, log performance metrics, and use appropriate log levels.

### Data Modeling

**Q16:** Analyze the `CourseDocument` data model. What relationships or additional fields might be useful?
- **Expected Answer:** Could add instructor information, course ratings, enrollment count, prerequisites, difficulty level, course duration, location/format details.

---

## Senior Level Questions

### Architecture & Design Patterns

**Q17:** How would you architect this system to handle millions of courses and thousands of concurrent users?
- **Expected Answer:** 
  - Implement caching (Redis)
  - Use connection pooling
  - Add load balancing
  - Consider read replicas for Elasticsearch
  - Implement circuit breakers
  - Add monitoring and metrics

**Q18:** What design patterns are used in this codebase, and how would you extend them?
- **Expected Answer:**
  - Dependency Injection (Spring)
  - Repository pattern (implied)
  - Could add: Strategy pattern for sorting, Factory pattern for query builders, Observer pattern for notifications

**Q19:** How would you implement multi-tenancy in this system (e.g., different schools/organizations)?
- **Expected Answer:** 
  - Add tenant ID to documents
  - Implement tenant-specific indices
  - Add security filters
  - Consider tenant isolation strategies

### Performance & Optimization

**Q20:** What performance bottlenecks do you identify, and how would you address them?
- **Expected Answer:**
  - Elasticsearch query optimization
  - Caching frequently accessed data
  - Index optimization and mapping improvements
  - Connection pool tuning
  - Query result pagination

**Q21:** How would you implement caching for this API? What would you cache and with what TTL?
- **Expected Answer:**
  - Cache popular search results (15-30 minutes)
  - Cache category/type listings (1 hour)
  - Use distributed cache for consistency
  - Implement cache warming strategies

### Security & Production Readiness

**Q22:** What security considerations are missing from this implementation?
- **Expected Answer:**
  - Authentication/Authorization
  - Input validation and sanitization
  - Rate limiting
  - HTTPS enforcement
  - SQL injection prevention (though using Elasticsearch)
  - Cross-origin resource sharing (CORS) configuration

**Q23:** How would you make this application production-ready?
- **Expected Answer:**
  - Add health checks
  - Implement proper logging and monitoring
  - Add metrics and alerting
  - Configure proper error handling
  - Add API documentation (OpenAPI/Swagger)
  - Implement graceful shutdown

### Testing Strategy

**Q24:** Design a comprehensive testing strategy for this application.
- **Expected Answer:**
  - Unit tests for service logic
  - Integration tests with Testcontainers
  - API testing with MockMvc
  - Performance testing
  - Contract testing for API consumers

---

## System Design Questions

### Scaling & Infrastructure

**Q25:** Design a system that can handle course search for a platform like Coursera or Udemy. What components would you need?
- **Expected Answer:**
  - Load balancers
  - API Gateway
  - Multiple service instances
  - Elasticsearch cluster
  - Caching layer
  - Content Delivery Network (CDN)
  - Database for structured data
  - Message queues for async processing

**Q26:** How would you handle real-time updates to course information (price changes, availability, etc.)?
- **Expected Answer:**
  - Event-driven architecture
  - Message queues (Kafka/RabbitMQ)
  - Change Data Capture (CDC)
  - Elasticsearch bulk update APIs
  - Cache invalidation strategies

### Data Management

**Q27:** Design a data pipeline to keep course information synchronized between multiple data sources.
- **Expected Answer:**
  - ETL/ELT pipelines
  - Data validation and cleansing
  - Conflict resolution strategies
  - Data versioning
  - Rollback mechanisms

**Q28:** How would you implement analytics and reporting on course search behavior?
- **Expected Answer:**
  - Event tracking
  - Data warehouse
  - Stream processing
  - Machine learning for recommendations
  - A/B testing framework

---

## Practical Coding Questions

### Implementation Challenges

**Q29:** Implement a feature to search for courses by instructor name. Show the code changes needed.

**Q30:** Add a feature to boost search results based on course popularity. How would you modify the search logic?

**Q31:** Implement auto-complete functionality for course titles. What data structures and algorithms would you use?

**Q32:** Write code to handle bulk course updates efficiently without blocking the search API.

### Code Review Questions

**Q33:** Review this search query and suggest improvements:
```java
// Show the existing search method and ask for optimization
```

**Q34:** How would you refactor the `CourseSearchService` to make it more testable and maintainable?

**Q35:** Implement proper exception handling for the search API with custom error responses.

---

## Advanced Scenarios

### Integration Questions

**Q36:** How would you integrate this search API with:
- A recommendation engine
- A user authentication system
- A payment processing system
- A content management system

**Q37:** Design an API versioning strategy for this service as it evolves.

**Q38:** How would you implement A/B testing for different search algorithms?

### Operations & Monitoring

**Q39:** What metrics would you track for this API in production?

**Q40:** How would you implement distributed tracing for search requests?

**Q41:** Design a deployment strategy that ensures zero downtime during updates.

---

## Assessment Guidelines

### Junior Level (Q1-Q8)
- Focus on basic Spring Boot and Java concepts
- Understanding of annotations and basic patterns
- Basic knowledge of REST APIs

### Mid-Level (Q9-Q16)
- Deeper understanding of Spring ecosystem
- API design principles
- Error handling and validation
- Basic performance considerations

### Senior Level (Q17-Q24)
- System architecture and scalability
- Performance optimization
- Security and production readiness
- Testing strategies and best practices

### System Design (Q25-Q28)
- Large-scale system design
- Data architecture
- Infrastructure planning
- Real-time processing

### Practical Coding (Q29-Q35)
- Hands-on implementation
- Code quality and best practices
- Problem-solving approach
- Refactoring skills

### Advanced Scenarios (Q36-Q41)
- Integration patterns
- Operations and monitoring
- DevOps practices
- Leadership and architectural decisions

---

## Interview Tips

1. **For Interviewers:**
   - Start with easier questions and progressively increase difficulty
   - Ask follow-up questions to gauge depth of understanding
   - Focus on thought process, not just final answers
   - Encourage discussion and different approaches

2. **For Candidates:**
   - Understand the business context behind technical decisions
   - Think about trade-offs and alternatives
   - Consider scalability and maintainability
   - Ask clarifying questions when needed

3. **Time Allocation:**
   - Junior: 30-45 minutes
   - Mid-Level: 45-60 minutes  
   - Senior: 60-90 minutes
   - Include time for coding exercises and system design discussions