# api-contract.md -- Infy LearnX (Online Learning Platform)

## 1. API Overview

All external APIs are exposed through **api-gateway**. Internal
microservices expose REST APIs for service-to-service communication
only. Services are discovered using Spring Cloud Consul and invoked
using logical service names with Spring Cloud LoadBalancer.

## 2. Authentication Assumptions

-   Basic role-based authentication for MVP.
-   Roles: Learner, Instructor, Administrator.
-   Gateway forwards authenticated requests.
-   No OAuth/JWT implementation defined for MVP.

## 3. Gateway Route Catalog

  Method   Gateway Endpoint                Target Service
  -------- ------------------------------- ---------------------
  POST     /api/learners                   learner-service
  GET      /api/learners/{learnerId}       learner-service
  POST     /api/instructors                instructor-service
  GET      /api/instructors/{id}           instructor-service
  POST     /api/courses                    course-service
  PUT      /api/courses/{id}/approve       course-service
  GET      /api/courses                    course-service
  POST     /api/enrollments                enrollment-service
  GET      /api/enrollments/{learnerId}    enrollment-service
  GET      /api/assessments/{courseId}     assessment-service
  POST     /api/assessments/{id}/attempt   assessment-service
  GET      /api/certificates/{learnerId}   certificate-service
  GET      /api/reports/platform           report-service

## 4. Service-wise Endpoint Catalog

### learner-service

-   POST /learners
-   GET /learners/{id}
-   PUT /learners/{id}
-   GET /learners/{id}/status

### instructor-service

-   POST /instructors
-   GET /instructors/{id}
-   PUT /instructors/{id}

### course-service

-   POST /courses
-   GET /courses
-   GET /courses/{id}
-   PUT /courses/{id}/approve
-   PUT /courses/{id}/publish

### enrollment-service

-   POST /enrollments
-   GET /enrollments/{learnerId}
-   GET /enrollments/check

### assessment-service

-   POST /assessments
-   GET /assessments/{courseId}
-   POST /assessments/{id}/attempt

### certificate-service

-   GET /certificates/{learnerId}

### report-service

-   GET /reports/platform
-   GET /reports/course/{courseId}

## 5. Request Contracts

### Learner Registration Request

  Field           Type     Required   Validation    Description
  --------------- -------- ---------- ------------- ---------------
  fullName        String   Yes        3-100 chars   Learner name
  email           String   Yes        Valid email   Email
  phoneNumber     String   Yes        10 digits     Mobile
  qualification   String   No         Max100        Qualification

### Course Creation Request

  Field          Type      Required   Validation
  -------------- --------- ---------- ---------------------
  courseTitle    String    Yes        Mandatory
  category       String    Yes        Mandatory
  duration       Integer   Yes        \>0
  instructorId   Long      Yes        Existing instructor

### Enrollment Request

| Field \| Type \| Required \| Validation \|
| learnerId \| Long \| Yes \| Existing learner \|
| courseId \| Long \| Yes \| Published course \|

## 6. Response Contracts

### Success

``` json
{
 "success":true,
 "message":"Request processed successfully",
 "data":{},
 "timestamp":"2026-08-05T10:00:00"
}
```

### Error

``` json
{
 "success":false,
 "errorCode":"COURSE_NOT_FOUND",
 "message":"Course not found",
 "timestamp":"2026-08-05T10:00:00"
}
```

## 7. DTO Definitions

Conceptual DTOs: - LearnerRequestDTO / LearnerResponseDTO -
InstructorRequestDTO / InstructorResponseDTO - CourseRequestDTO /
CourseResponseDTO - EnrollmentRequestDTO / EnrollmentResponseDTO -
AssessmentRequestDTO / AssessmentResponseDTO - QuizAttemptDTO -
CertificateResponseDTO - ReportResponseDTO

## 8. Validation Rules

### Request Level

-   Mandatory fields
-   Email format
-   Phone validation
-   Positive IDs

### Business Level

-   Learner must exist.
-   Instructor must exist.
-   Course must be approved & published.
-   Duplicate enrollment prohibited.
-   Assessment must be active.
-   Certificate generated only after completion.

## 9. Error Responses

  HTTP   Error Code             Scenario                 Service
  ------ ---------------------- ------------------------ --------------------
  400    VALIDATION_ERROR       Invalid input            Owning Service
  404    LEARNER_NOT_FOUND      Missing learner          learner-service
  404    INSTRUCTOR_NOT_FOUND   Missing instructor       instructor-service
  404    COURSE_NOT_FOUND       Missing course           course-service
  409    DUPLICATE_ENROLLMENT   Already enrolled         enrollment-service
  400    COURSE_NOT_PUBLISHED   Course unavailable       course-service
  503    SERVICE_UNAVAILABLE    Dependency unavailable   Any

## 10. Pagination / Filtering

Supported for list APIs: - page - size - sort - category - status -
instructorId

## 11. Inter-Service API Calls

  -----------------------------------------------------------------------
  Caller                              Calls
  ----------------------------------- -----------------------------------
  enrollment-service                  learner-service, course-service

  assessment-service                  enrollment-service

  certificate-service                 enrollment-service,
                                      assessment-service

  report-service                      learner, course, enrollment,
                                      assessment, certificate services
  -----------------------------------------------------------------------

## 12. Load Balancing Assumption

All service calls use logical service names registered in Consul and
resolved through Spring Cloud LoadBalancer.

## 13. Resilience & Fallback

  Service Down          Fallback
  --------------------- ------------------------------------
  learner-service       Learner service unavailable
  course-service        Course information unavailable
  enrollment-service    Enrollment temporarily unavailable
  assessment-service    Assessment unavailable
  certificate-service   Certificate service unavailable
  report-service        Report unavailable

## 14. Requirement Mapping

  Endpoint                        FR          AC
  ------------------------------- ----------- -----------
  POST /learners                  FR-001      AC-001
  POST /instructors               FR-002      AC-002
  POST /courses                   FR-003      AC-003
  PUT /courses/{id}/approve       FR-004      AC-004
  PUT /courses/{id}/publish       FR-005      AC-005
  GET /courses                    FR-006      AC-006
  POST /enrollments               FR-007      AC-007
  GET /assessments/{courseId}     FR-010,11   AC-010,11
  GET /certificates/{learnerId}   FR-012      AC-012
  GET /reports/platform           FR-013      AC-013

## 15. Sample Requests & Responses

### POST /api/enrollments

Request

``` json
{
 "learnerId":1001,
 "courseId":101
}
```

Response

``` json
{
 "success":true,
 "message":"Enrollment successful"
}
```

Negative Response

``` json
{
 "success":false,
 "errorCode":"DUPLICATE_ENROLLMENT",
 "message":"Learner already enrolled"
}
```

## Notes

-   Gateway is the only public entry point.
-   No shared database.
-   No cross-service foreign keys.
-   REST APIs only.
-   Local transactions only.
-   No implementation code included.
