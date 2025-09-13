# Quick Interview Guide - Course Search API

## 15-Minute Technical Screening Questions

### Spring Boot Basics (5 minutes)
1. **What is Spring Boot and why use it over regular Spring?**
2. **Explain the `@RestController` vs `@Controller` difference**
3. **What does `@RequestParam(required = false)` mean in our search API?**

### Elasticsearch Understanding (5 minutes)
4. **What's the difference between `Text` and `Keyword` field types?**
5. **How does fuzzy search work in our implementation?**
6. **Explain the pagination approach used in the search service**

### Code Analysis (5 minutes)
7. **Walk through the search flow from controller to service**
8. **What potential issues do you see in the current error handling?**
9. **How would you improve the current search functionality?**

---

## 30-Minute Technical Interview

### Architecture & Design (10 minutes)
1. **How would you scale this API for 1000+ concurrent users?**
2. **What caching strategy would you implement?**
3. **How would you handle real-time course updates?**

### Implementation (15 minutes)
4. **Code Review:** Review the `CourseSearchService` and suggest improvements
5. **Feature Addition:** Add instructor-based search functionality (whiteboard/live coding)
6. **Testing:** Design test cases for the search API

### Production Readiness (5 minutes)
7. **What's missing to make this production-ready?**
8. **How would you monitor this API in production?**

---

## 60-Minute Senior Interview

### System Design (20 minutes)
- Design a course search system for 10M+ courses (Coursera scale)
- Include: architecture, data flow, scalability, and monitoring

### Deep Technical (25 minutes)
- Elasticsearch optimization strategies
- Multi-tenant architecture design
- Performance bottleneck analysis and solutions
- Security implementation

### Leadership & Process (15 minutes)
- Code review process and standards
- Team collaboration on API design
- Technical debt management
- Mentoring junior developers on this codebase

---

## Red Flags to Watch For

❌ **Avoid candidates who:**
- Can't explain basic Spring Boot concepts
- Don't understand REST API principles
- Can't identify obvious performance issues
- Lack knowledge of basic error handling

✅ **Look for candidates who:**
- Understand trade-offs in technical decisions
- Think about user experience and edge cases
- Consider maintainability and scalability
- Ask clarifying questions about requirements

---

## Quick Assessment Rubric

### Junior (1-2 years)
- ✅ Understands basic Spring Boot annotations
- ✅ Can explain the API flow
- ✅ Identifies basic issues in code
- ✅ Suggests simple improvements

### Mid-Level (3-5 years)
- ✅ All junior requirements
- ✅ Understands Elasticsearch concepts
- ✅ Can design API improvements
- ✅ Considers error handling and validation
- ✅ Thinks about basic scalability

### Senior (5+ years)
- ✅ All mid-level requirements
- ✅ Designs scalable architectures
- ✅ Considers security and production concerns
- ✅ Can lead technical discussions
- ✅ Identifies complex trade-offs
- ✅ Mentors others effectively

---

## Sample Follow-up Questions

**"How would you...?"**
- Handle millions of courses?
- Implement personalized search results?
- Add real-time notifications for course updates?
- Ensure 99.9% uptime?
- Handle multi-language course content?

**"What would you do if...?"**
- Elasticsearch goes down during peak traffic?
- Search response time degrades to 5+ seconds?
- You need to migrate to a different search engine?
- Course data becomes inconsistent across services?

**"Tell me about a time when..."**
- You optimized a slow API
- You had to design a search system
- You worked with large datasets
- You mentored someone on similar technology