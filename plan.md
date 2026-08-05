# plan.md -- Infy LearnX (Online Learning Platform)

## 1. Project Overview

Infy LearnX is a Spring Boot microservices-based online learning
platform. The implementation follows the engineering standards, MVP
decisions, and functional specification defined in the supporting
documents. The solution uses Spring Cloud Gateway, Spring Cloud Consul,
Spring Cloud LoadBalancer, and Resilience4j.

### Objectives

-   Deliver scalable learning platform
-   Independent microservices
-   High availability and maintainability
-   Centralized configuration
-   Service discovery and resilience

## 2. Functional Requirement Summary

Implements FR-001 through FR-018: - Learner & Instructor Registration -
Course Creation, Approval and Publishing - Course Search & Enrollment -
Learning Content Access - Progress Tracking - Quiz & Assessment -
Certificate Generation - User Management - API Gateway Routing - Consul
Discovery - Load Balancing - Circuit Breaker - Centralized Configuration

## 3. Microservices Architecture Overview

Services: - api-gateway - learner-service - instructor-service -
course-service - enrollment-service - assessment-service -
certificate-service - report-service - Spring Cloud Consul - Consul
Configuration

Each service owns its database and communicates through REST APIs.

## 4. Service Responsibility Matrix

### learner-service

Owns learner profile and learner data.

### instructor-service

Owns instructor profile.

### course-service

Owns course details, publishing and approval status.

### enrollment-service

Handles enrollment workflow, validation and duplicate prevention.

### assessment-service

Manages quizzes and assessments.

### certificate-service

Generates certificates after completion.

### report-service

Provides reports and summaries.

### api-gateway

Routes requests without business logic.

## 5. Architecture Flow

1.  Client sends request to API Gateway.
2.  Gateway routes using service names.
3.  Services register with Consul.
4.  Services fetch configuration from Consul.
5.  Enrollment Service validates learner and course.
6.  Assessment Service evaluates learner.
7.  Certificate Service generates certificate.
8.  Report Service aggregates information.
9.  Spring Cloud LoadBalancer resolves instances.
10. Resilience4j provides fallback responses.

## 6. Layer Design Per Service

Each service contains: - Controller - Service - Repository - DTO -
Entity - Mapper - Exception - Validation - Configuration - REST Client

## 7. Modules / Components

Business: - learner-service - instructor-service - course-service -
enrollment-service - assessment-service - certificate-service -
report-service

Infrastructure: - API Gateway - Consul Discovery - Consul
Configuration - Spring Cloud LoadBalancer - Resilience4j - Common Error
Response - Common Validation

## 8. Business Workflows

-   Learner Registration
-   Instructor Registration
-   Course Creation
-   Course Approval
-   Course Publishing
-   Course Enrollment
-   Duplicate Enrollment Validation
-   Learning Content Access
-   Progress Tracking
-   Quiz Creation
-   Assessment Submission
-   Certificate Generation
-   Reporting
-   Service Failure Handling

## 9. API Gateway Strategy

-   Single external entry point
-   Service-name routing
-   Request forwarding
-   No business logic
-   No persistence

## 10. Service Discovery Strategy

-   Register services with Consul
-   Lookup using logical names
-   No hardcoded URLs

## 11. Centralized Configuration Strategy

-   Shared properties
-   Service-specific properties
-   Consul configuration
-   Refresh assumptions

## 12. Load Balancing Strategy

-   Spring Cloud LoadBalancer
-   Client-side balancing
-   Logical service names

## 13. Resilience Strategy

-   Circuit breakers
-   Fallback responses
-   Timeout handling
-   Failure isolation

## 14. Security Approach

-   Basic role-based security
-   Learner access
-   Instructor access
-   Administrator access
-   Basic authentication assumptions

## 15. Validation Strategy

-   Bean Validation
-   Business Validation
-   Duplicate enrollment validation
-   Course availability validation
-   Assessment eligibility validation

## 16. Transaction Strategy

-   Local transactions
-   Independent databases
-   No distributed transactions
-   Eventual consistency

## 17. Logging Strategy

-   Request logs
-   Response logs
-   Error logs
-   Business event logs

## 18. Test Strategy

### Unit Tests

Business logic and validations.

### Controller Tests

REST endpoints and HTTP responses.

### Integration Tests

Gateway, Consul, LoadBalancer, Resilience4j.

### Manual Tests

End-to-end workflows.

## 19. Risks and Mitigations

  Risk                   Mitigation
  ---------------------- ---------------------------
  Service unavailable    Circuit Breaker
  Gateway failure        Route validation
  Configuration error    Centralized Consul config
  Duplicate enrollment   Business validation
  Network failure        Retry/Fallback

## 20. Requirement Mapping

  -----------------------------------------------------------------------------
  Business Goal                        FR               Service
  ------------------------------------ ---------------- -----------------------
  Learning Platform                    FR-001--FR-013   Business Services

  Scalable Architecture                FR-014--FR-018   Gateway &
                                                        Infrastructure

  Reliable Communication               FR-015--FR-017   Consul, LoadBalancer,
                                                        Resilience4j

  Centralized Configuration            FR-018           Consul Config
  -----------------------------------------------------------------------------

## Exclusions

-   Source code
-   DTO definitions
-   Database schema
-   SQL scripts
-   Full API contracts
-   Deployment scripts
