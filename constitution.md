# constitution.md

# Infy LearnX – Engineering Constitution

## Purpose

This document defines the engineering standards and architectural principles for the **Infy LearnX Online Learning Platform**.

All developers should follow these standards throughout implementation to ensure consistency, maintainability, scalability, and clear service ownership.

---

# 1. Microservices Architecture

- Every microservice must have a single, well-defined business responsibility.
- Every service should be independently deployable.
- Every service should own its own business logic and data.
- Every service should contain its own Controller, Service, Repository, DTO, Mapper, Exception Handling, and Configuration layers.
- Services must never directly access another service's database.
- Inter-service communication must occur only through REST APIs or approved service clients.
- Services should remain loosely coupled.

---

# 2. Service Boundary Rules

## learner-service
- Owns learner information and learner profile.

## instructor-service
- Owns instructor information and instructor profile.

## course-service
- Owns course details, content, and approval status.

## enrollment-service
- Owns enrollment workflow, eligibility validation, and duplicate enrollment prevention.

## assessment-service
- Owns quizzes, assessments, and assessment results.

## certificate-service
- Owns certificate generation and certificate records.

## report-service
- Owns reports, enrollment summaries, and platform statistics.

## api-gateway
- Single external entry point.
- Routes requests.
- Contains no business logic.

---

# 3. Spring Cloud Consul

- Register every service with Consul.
- Use Consul for service discovery.
- Use centralized configuration where applicable.
- Keep service names consistent.

---

# 4. Spring Cloud Gateway

- All client requests must go through the API Gateway.
- Gateway performs routing only.
- Gateway must not contain business logic.

---

# 5. Spring Cloud LoadBalancer

- Use service names instead of fixed host/port values.
- Use client-side load balancing for inter-service communication.

---

# 6. Resilience4j

- Protect inter-service calls with Circuit Breakers.
- Configure meaningful fallback responses.
- Prevent cascading failures.

---

# 7. Controller Responsibilities

- Handle request mapping.
- Trigger validation.
- Call service layer.
- Return standardized JSON responses.
- Do not implement business logic.

---

# 8. Service Responsibilities

- Implement business rules.
- Coordinate workflows.
- Validate business conditions.
- Communicate with other services using REST clients.
- Handle business exceptions.

---

# 9. Repository Responsibilities

- Access only the owning service database.
- Perform persistence operations only.
- No business logic.

---

# 10. DTO Standards

- Use DTOs for requests and responses.
- Never expose entity classes through APIs.
- Include only required fields.

---

# 11. Validation Standards

- Use Bean Validation for request validation.
- Perform business validation in the service layer.
- Validate enrollment eligibility, duplicate enrollment, course availability, assessment eligibility, and certificate generation rules.

---

# 12. Exception Handling

- Centralized exception handling in every service.
- Consistent JSON error responses.
- Clear validation and business error messages.

---

# 13. Transactions

- Use local transactions inside each service.
- Avoid distributed transactions.
- Assume eventual consistency where needed.

---

# 14. Testing Standards

## Unit Testing
- Service business logic
- Validation rules

## Controller Testing
- Request validation
- Response format
- HTTP status codes

## Integration / Manual Testing
- API Gateway
- Service Discovery
- Consul registration
- Load Balancer
- Circuit Breaker
- Inter-service communication

---

# General Engineering Principles

- Follow clean code practices.
- Keep services loosely coupled.
- Maintain high cohesion.
- Follow separation of concerns.
- Use consistent naming conventions.
- Keep the solution simple and suitable for a training/demo project.

---

**End of constitution.md**
