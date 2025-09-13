# Practical Coding Exercises - Course Search API

## Exercise 1: Add Instructor Search (Junior Level)
**Time: 15-20 minutes**

### Problem Statement
Add the ability to search courses by instructor name. The instructor information should be added to the `CourseDocument` model and the search functionality should support finding courses by instructor name.

### Required Changes
1. Add instructor field to `CourseDocument`
2. Update search service to include instructor search
3. Update controller to accept instructor parameter

### Sample Solution
```java
// In CourseDocument.java
@Field(type = FieldType.Text)
private String instructor;

// In CourseSearchController.java
@GetMapping
public CourseSearchResponse searchCourses(
    // ... existing parameters
    @RequestParam(required = false) String instructor
) {
    return courseSearchService.searchCourses(
        q, category, type, minAge, maxAge, minPrice, maxPrice, 
        startDate, instructor, page, size, sort
    );
}

// In CourseSearchService.java
if (instructor != null && !instructor.isBlank()) {
    criteria = criteria.and(new Criteria("instructor").matches(instructor));
}
```

### Follow-up Questions
- How would you handle partial instructor name matches?
- What if we want to search by multiple instructors?
- How would you optimize this for performance?

---

## Exercise 2: Implement Auto-Complete (Mid Level)
**Time: 25-30 minutes**

### Problem Statement
Create an auto-complete endpoint that suggests course titles as users type. The endpoint should return the top 10 matching course titles for a given prefix.

### Requirements
- New endpoint: `GET /api/autocomplete?prefix={text}`
- Return up to 10 course title suggestions
- Should be fast (under 100ms)
- Case-insensitive matching

### Expected Approach
```java
@RestController
@RequestMapping("/api")
public class AutoCompleteController {
    
    @GetMapping("/autocomplete")
    public List<String> autoComplete(@RequestParam String prefix) {
        // Implementation here
    }
}

// Service implementation using Elasticsearch suggestions
// or prefix queries with proper caching
```

### Evaluation Criteria
- Proper endpoint design
- Efficient query implementation
- Consideration of caching
- Error handling for edge cases

---

## Exercise 3: Advanced Filtering (Mid-Senior Level)
**Time: 30-35 minutes**

### Problem Statement
Implement a "smart filter" that allows users to search with natural language queries like:
- "Math courses for kids under $50"
- "Programming courses starting next month"
- "Art classes for teenagers"

### Requirements
1. Parse natural language input
2. Extract filters (subject, price, age, date)
3. Apply to existing search functionality
4. Return structured search results

### Discussion Points
- How would you parse natural language?
- What libraries or approaches would you use?
- How would you handle ambiguous queries?
- How would you test this functionality?

---

## Exercise 4: Performance Optimization (Senior Level)
**Time: 40-45 minutes**

### Problem Statement
The search API is experiencing slow response times (2-3 seconds) under high load. Analyze the current implementation and propose optimizations.

### Current Issues to Identify
1. No caching implementation
2. Inefficient Elasticsearch queries
3. No connection pooling configuration
4. Lack of pagination limits
5. Missing indices optimization

### Required Deliverables
1. **Analysis:** Identify performance bottlenecks
2. **Solutions:** Propose specific optimizations
3. **Implementation:** Show code for 2-3 key improvements
4. **Monitoring:** How would you measure improvements?

### Sample Optimization Areas
```java
// 1. Implement caching
@Cacheable(value = "searchResults", key = "#root.methodName + #q + #category")
public CourseSearchResponse searchCourses(/*...*/) {
    // existing implementation
}

// 2. Optimize Elasticsearch query
// Use specific fields instead of full document retrieval
// Add proper index configurations
// Implement search result highlighting

// 3. Add connection pool configuration
// Configure proper timeouts and retry policies
```

---

## Exercise 5: Error Handling & Validation (All Levels)
**Time: 20-25 minutes**

### Problem Statement
The current API lacks proper error handling and validation. Implement comprehensive error handling with meaningful error messages.

### Requirements
1. Validate all input parameters
2. Handle Elasticsearch connection failures
3. Return proper HTTP status codes
4. Provide meaningful error messages

### Expected Implementation
```java
// Custom exception handling
@ControllerAdvice
public class ApiExceptionHandler {
    
    @ExceptionHandler(InvalidParameterException.class)
    public ResponseEntity<ErrorResponse> handleInvalidParameter(
        InvalidParameterException ex) {
        // Implementation
    }
    
    @ExceptionHandler(ElasticsearchConnectionException.class)
    public ResponseEntity<ErrorResponse> handleElasticsearchError(
        ElasticsearchConnectionException ex) {
        // Implementation
    }
}

// Validation annotations
public CourseSearchResponse searchCourses(
    @RequestParam(required = false) @Size(min = 2, max = 100) String q,
    @RequestParam(required = false) @Min(0) @Max(99) Integer minAge,
    @RequestParam(required = false) @DecimalMin("0.0") Double minPrice
    // ...
) {
    // Implementation
}
```

---

## Exercise 6: System Design Challenge (Senior Level)
**Time: 45-60 minutes**

### Problem Statement
Design a distributed course search system that can handle:
- 10 million courses
- 100,000 concurrent users
- Real-time course updates
- Multi-region deployment

### Components to Design
1. **Architecture Overview**
   - Load balancers
   - API Gateway
   - Service instances
   - Data layer

2. **Data Management**
   - Elasticsearch cluster setup
   - Data synchronization
   - Backup and recovery

3. **Scalability**
   - Horizontal scaling strategies
   - Caching layers
   - CDN integration

4. **Monitoring & Operations**
   - Health checks
   - Metrics and alerting
   - Deployment strategies

### Deliverables
- Architecture diagram
- Technology stack justification
- Scalability plan
- Monitoring strategy

---

## Code Review Exercise
**Time: 15-20 minutes**

### Problem Statement
Review the following code snippet and identify issues and improvements:

```java
@GetMapping("/search")
public List<CourseDocument> searchCourses(
    @RequestParam String query,
    @RequestParam int page) {
    
    try {
        Criteria criteria = new Criteria("title").matches(query);
        PageRequest pageable = PageRequest.of(page, 100);
        CriteriaQuery criteriaQuery = new CriteriaQuery(criteria, pageable);
        
        SearchHits<CourseDocument> hits = 
            elasticsearchOperations.search(criteriaQuery, CourseDocument.class);
        
        List<CourseDocument> results = new ArrayList<>();
        for (SearchHit<CourseDocument> hit : hits) {
            results.add(hit.getContent());
        }
        
        return results;
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
        return new ArrayList<>();
    }
}
```

### Issues to Identify
1. Large page size (100) without limits
2. Poor error handling
3. Missing null checks
4. No validation on parameters
5. Direct return of domain objects
6. Console logging instead of proper logging
7. Generic exception catching

### Expected Improvements
- Add proper validation
- Implement proper error responses
- Use DTOs instead of domain objects
- Add proper logging
- Limit page size
- Handle specific exceptions

---

## Assessment Guidelines

### Junior Developer (0-2 years)
- **Focus:** Basic implementation and understanding
- **Expect:** Working code with guidance
- **Look for:** Learning attitude, basic problem-solving

### Mid-Level Developer (2-5 years)
- **Focus:** Code quality and best practices
- **Expect:** Independent implementation
- **Look for:** Error handling, performance awareness

### Senior Developer (5+ years)
- **Focus:** Architecture and leadership
- **Expect:** Optimal solutions with trade-off discussions
- **Look for:** System thinking, mentoring capability

### Interview Tips
1. **Start Simple:** Begin with easier exercises and increase complexity
2. **Observe Process:** Focus on problem-solving approach, not just final solution
3. **Encourage Discussion:** Ask about trade-offs and alternative approaches
4. **Real-world Context:** Relate exercises to actual production scenarios
5. **Time Management:** Adjust complexity based on available time

### Common Red Flags
❌ Cannot implement basic functionality
❌ Ignores error handling completely
❌ No consideration for edge cases
❌ Cannot explain their code
❌ Resistant to feedback or suggestions

### Positive Indicators
✅ Asks clarifying questions
✅ Considers edge cases
✅ Thinks about performance
✅ Writes clean, readable code
✅ Open to feedback and iteration