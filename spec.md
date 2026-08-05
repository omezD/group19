# spec.md -- Infy LearnX (Online Learning Platform)

# 1. Feature Overview

Infy LearnX is a microservices-based online learning platform that
enables learners to discover courses, enroll, access learning content,
complete quizzes and assessments, track progress, and receive
certificates. Instructors create and manage courses and assessments,
while administrators approve courses and manage platform users.

The platform is composed of independent microservices such as User
Service, Course Service, Enrollment Service, Content Service, Assessment
Service, Progress Service, Certificate Service, API Gateway, Consul
Service Registry, and Centralized Configuration. Each service owns its
own data and communicates through REST APIs using service discovery.

# 2. Actors

  -----------------------------------------------------------------------
  Actor                               Responsibilities
  ----------------------------------- -----------------------------------
  Student                             Search courses, enroll, learn, take
                                      quizzes, view progress, download
                                      certificates

  Instructor                          Create courses, upload content,
                                      manage quizzes, monitor learners

  Administrator                       Manage users, approve/reject
                                      courses, monitor platform

  System                              Service discovery, routing,
                                      configuration, notifications,
                                      resilience
  -----------------------------------------------------------------------

# 3. Scope

## In Scope

-   Student and instructor registration
-   Course creation, approval and publishing
-   Course search and enrollment
-   Learning content access
-   Progress tracking
-   Quiz and assessment management
-   Certificate generation
-   User management
-   API Gateway
-   Consul service discovery
-   Centralized configuration
-   Load-balanced service communication
-   Circuit breaker fallback

## Out of Scope

-   Payment processing
-   Live video classes
-   AI recommendations
-   Distributed transactions
-   Production deployment
-   Event streaming
-   Advanced authentication unless specified

# 4. Functional Requirements

  ID       Requirement
  -------- -------------------------------------------------
  FR-001   Register learner profile
  FR-002   Register instructor profile
  FR-003   Create course
  FR-004   Approve or reject course
  FR-005   Publish approved course
  FR-006   Search available courses
  FR-007   Enroll learner into course
  FR-008   Access learning materials
  FR-009   Track learner progress
  FR-010   Create and manage quizzes
  FR-011   Attempt assessments
  FR-012   Generate completion certificate
  FR-013   Manage users
  FR-014   Route requests through API Gateway
  FR-015   Discover services using Consul
  FR-016   Load balance service communication
  FR-017   Return fallback responses using circuit breaker
  FR-018   Retrieve centralized configuration

Each FR shall define inputs, processing, outputs, preconditions and
postconditions during detailed design.

# 5. Non-Functional Requirements

-   High availability
-   Horizontal scalability
-   Loose coupling
-   Independent deployment
-   Resilience through Resilience4j
-   Centralized logging and monitoring
-   Externalized configuration
-   Maintainability
-   Testability
-   REST API consistency

# 6. Validation Rules

-   Student ID must exist.
-   Instructor ID must exist.
-   Course ID must exist.
-   Course must be approved and published before enrollment.
-   Duplicate enrollment is not allowed.
-   Quiz must be active.
-   Assessment must be available.
-   Certificate generated only after successful completion.

# 7. Business Rules

-   Only instructors create courses.
-   Only administrators approve courses.
-   Students enroll only in published courses.
-   One student can enroll only once per course.
-   Certificates are generated only after course completion.
-   Fallback response shall be returned if dependent services are
    unavailable.

# 8. Microservices Rules

-   Every service owns its database.
-   No direct database sharing.
-   Communication only through REST APIs.
-   Services use Consul registered names.
-   API Gateway is the single external entry point.
-   Resilience4j protects inter-service calls.
-   Every service contains Controller, Service, Repository, DTO, Mapper,
    Exception and Configuration layers.

# 9. Edge Cases

-   Invalid student
-   Invalid instructor
-   Course not found
-   Course not approved
-   Course not published
-   Duplicate enrollment
-   Quiz unavailable
-   Assessment already submitted
-   Certificate generation failure
-   Empty search result
-   Service unavailable
-   Gateway routing failure
-   Missing configuration
-   Service not registered with Consul

# 10. Assumptions

-   Users have valid identities.
-   Services are registered in Consul.
-   REST APIs are available.
-   Courses follow Draft → Approved → Published lifecycle.
-   Certificates depend on successful completion.

# 11. Acceptance Criteria

  AC ID    FR Mapping   Acceptance Criteria
  -------- ------------ -----------------------------------------------
  AC-001   FR-001       Learner registration succeeds with valid data
  AC-002   FR-002       Instructor registration succeeds
  AC-003   FR-003       Course is created successfully
  AC-004   FR-004       Administrator can approve/reject course
  AC-005   FR-005       Approved course becomes searchable
  AC-006   FR-006       Student can search courses
  AC-007   FR-007       Enrollment succeeds once only
  AC-008   FR-008       Learner accesses course materials
  AC-009   FR-009       Progress updates correctly
  AC-010   FR-010       Quiz is created successfully
  AC-011   FR-011       Assessment submission succeeds
  AC-012   FR-012       Certificate generated after completion
  AC-013   FR-013       Admin manages users
  AC-014   FR-014       Requests route through gateway
  AC-015   FR-015       Services discover each other
  AC-016   FR-016       Calls are load balanced
  AC-017   FR-017       Fallback returned on failure
  AC-018   FR-018       Configuration retrieved centrally

# 12. Error Codes

  Error Code                     Description
  ------------------------------ -----------------------------------
  STUDENT_NOT_FOUND              Student does not exist
  INSTRUCTOR_NOT_FOUND           Instructor does not exist
  COURSE_NOT_FOUND               Course unavailable
  COURSE_NOT_APPROVED            Course awaiting approval
  COURSE_NOT_PUBLISHED           Course unavailable for enrollment
  DUPLICATE_ENROLLMENT           Already enrolled
  QUIZ_NOT_FOUND                 Quiz missing
  QUIZ_NOT_AVAILABLE             Quiz inactive
  ASSESSMENT_ALREADY_SUBMITTED   Duplicate submission
  CERTIFICATE_NOT_AVAILABLE      Certificate unavailable
  USER_NOT_AUTHORIZED            Access denied
  VALIDATION_ERROR               Invalid request
  SERVICE_UNAVAILABLE            Dependent service unavailable
  GATEWAY_ROUTE_ERROR            Routing failed
  CONFIGURATION_NOT_FOUND        Missing configuration
  INTERNAL_SERVER_ERROR          Unexpected server error

# 13. User Stories

-   As a Student, I want to search courses so that I can learn new
    skills.
-   As a Student, I want to enroll in a course so that I can start
    learning.
-   As a Student, I want to track my progress so that I know my
    completion status.
-   As an Instructor, I want to create courses so that learners can
    access them.
-   As an Instructor, I want to create quizzes so that I can evaluate
    learners.
-   As an Administrator, I want to approve courses so that only quality
    content is published.
-   As an Administrator, I want to manage users so that the platform
    remains secure.

# 14. Requirement Traceability Matrix

  Business Goal            Functional Requirement   Acceptance Criteria
  ------------------------ ------------------------ ---------------------
  Online learning          FR-001--FR-013           AC-001--AC-013
  Scalable architecture    FR-014--FR-018           AC-014--AC-018
  Reliable communication   FR-015--FR-017           AC-015--AC-017
  Centralized management   FR-018                   AC-018
