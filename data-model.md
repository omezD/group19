# data-model.md -- Infy LearnX (Online Learning Platform)

## 1. Data Ownership Overview

  -----------------------------------------------------------------------
  Microservice            Owned Entities          Description
  ----------------------- ----------------------- -----------------------
  learner-service         Learner                 Learner profile and
                                                  status

  instructor-service      Instructor              Instructor profile and
                                                  specialization

  course-service          Course, CourseContent   Course lifecycle and
                                                  learning materials

  enrollment-service      Enrollment              Course enrollments and
                                                  completion progress

  assessment-service      Assessment, QuizAttempt Assessments, quizzes
                                                  and learner attempts

  certificate-service     Certificate             Course completion
                                                  certificates

  report-service          Derived Reports         Aggregated reports
                                                  generated from service
                                                  APIs

  api-gateway             None                    Routes requests only

  Consul                  None                    Service discovery and
                                                  configuration only
  -----------------------------------------------------------------------

## 2. Service-wise Entities

### learner-service

**Learner** \| Field \| Type \| Required \| Validation \| Example \|
\|---\|---\|---\|---\|---\| \| learnerId \| Long \| Yes \| Unique \|
1001 \| \| fullName \| String \| Yes \| 3-100 chars \| Rahul Sharma \|
\| email \| String \| Yes \| Valid & Unique \| rahul@mail.com \| \|
phoneNumber \| String \| Yes \| 10 digits \| 9876543210 \| \|
qualification \| String \| No \| Max 100 chars \| B.Tech \| \|
registrationDate \| Date \| Yes \| Past/Current \| 2026-08-01 \| \|
learnerStatus \| Enum \| Yes \| ACTIVE/INACTIVE \| ACTIVE \|

### instructor-service

**Instructor** - instructorId, fullName, email, specialization,
experience, joiningDate, instructorStatus

### course-service

**Course** - courseId, courseTitle, description, category, duration,
instructorId(reference), approvalStatus, publicationStatus, createdDate

**CourseContent** - contentId, courseId(reference), title, contentType,
resourceUrl, sequenceNumber

### enrollment-service

**Enrollment** - enrollmentId, learnerId(reference),
courseId(reference), enrollmentDate, enrollmentStatus,
completionPercentage

### assessment-service

**Assessment** - assessmentId, courseId(reference), title, totalMarks,
passingMarks, assessmentStatus, availableFrom, availableUntil

**QuizAttempt** - attemptId, learnerId(reference),
assessmentId(reference), obtainedMarks, attemptNumber, submissionTime,
resultStatus

### certificate-service

**Certificate** - certificateId, learnerId(reference),
courseId(reference), issueDate, certificateStatus, certificateUrl

### report-service

No persistent business entity. Reports are derived dynamically through
service APIs.

## 3. Fields

For every entity include: - Field Name - Data Type - Description -
Required/Optional - Validation Rule - Example Value

## 4. Relationships

-   Learner enrolls in Course.
-   Instructor creates Course.
-   Course contains CourseContent.
-   Course has Assessments.
-   Enrollment stores learnerId and courseId as logical references.
-   Assessment stores courseId as logical reference.
-   Certificate stores learnerId and courseId as logical references.
-   No cross-service foreign keys.

## 5. Keys

### Primary Keys

learnerId, instructorId, courseId, contentId, enrollmentId,
assessmentId, attemptId, certificateId

### Unique Constraints

-   Learner email
-   Instructor email
-   learnerId + courseId (Enrollment)
-   learnerId + assessmentId + attemptNumber (QuizAttempt)

### Logical Reference Keys

learnerId, instructorId, courseId, assessmentId

## 6. Constraints

-   Learner email must be unique.
-   Instructor email must be unique.
-   Course must be approved before publication.
-   Course must be published before enrollment.
-   Duplicate enrollment not allowed.
-   Assessment must be ACTIVE.
-   Certificate generated only after successful completion.

## 7. Validations

### Entity Validation

-   Mandatory fields
-   Email format
-   Phone format
-   Date validation

### Business Validation

-   Course approval
-   Course publication
-   Enrollment eligibility
-   Duplicate enrollment
-   Assessment eligibility
-   Certificate eligibility

## 8. Enums

-   LearnerStatus: ACTIVE, INACTIVE, SUSPENDED
-   InstructorStatus: ACTIVE, INACTIVE
-   CourseApprovalStatus: PENDING, APPROVED, REJECTED
-   CoursePublicationStatus: DRAFT, PUBLISHED, ARCHIVED
-   EnrollmentStatus: ENROLLED, COMPLETED, CANCELLED
-   AssessmentStatus: UPCOMING, ACTIVE, CLOSED
-   QuizResultStatus: PASSED, FAILED
-   CertificateStatus: GENERATED, REVOKED

## 9. Sample Data

### Learner

1001, Rahul Sharma, ACTIVE

### Instructor

501, Priya Nair, Java

### Course

C101, Spring Boot Microservices

### Enrollment

E1001, Learner 1001, Course C101

### Assessment

A101, Spring Boot Assessment

### Certificate

CERT101, Learner1001, Course C101

## 10. Requirement-to-Entity Mapping

  FR           Service               Entity
  ------------ --------------------- -------------------------
  FR-001       learner-service       Learner
  FR-002       instructor-service    Instructor
  FR-003-005   course-service        Course
  FR-006-007   enrollment-service    Enrollment
  FR-008-011   assessment-service    Assessment, QuizAttempt
  FR-012       certificate-service   Certificate
  FR-013       report-service        Reports
  FR-014-018   Infrastructure        Gateway, Consul

## 11. JPA Mapping Suggestions

-   Use @Entity and @Table
-   Use @Id and @GeneratedValue
-   Use @Column constraints
-   Use @Enumerated(EnumType.STRING)
-   Use Bean Validation annotations
-   Store only logical IDs across services
-   Avoid JPA relationships between different services

## 12. Cross-Service Data Consistency Notes

-   Every service owns its own database.
-   No shared database.
-   No cross-service foreign keys.
-   Services validate reference IDs through REST APIs.
-   Local transactions only.
-   Eventual consistency is assumed.
-   Report service aggregates data through APIs.
-   API Gateway and Consul own no business data.
