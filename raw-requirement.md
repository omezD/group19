# Infy LearnX – Raw Requirements

## 1. Business Objective

Infy LearnX is an online learning platform that enables learners to access quality education through a centralized digital system. The platform connects learners, instructors, and administrators to support the complete learning lifecycle, including course creation, enrollment, content delivery, assessments, progress tracking, certification, and reporting.

The objective is to build a scalable, secure, and user-friendly Spring Boot microservices-based application that provides seamless learning experiences while simplifying course management and platform administration.

---

## 2. Users / Actors

### Learner
- Register and log in to the platform.
- Browse available courses.
- Enroll in eligible courses.
- Access learning materials.
- Attempt quizzes and assessments.
- Track learning progress.
- View completed courses.
- Download or view certificates after successful completion.

### Instructor
- Create new courses.
- Update course information.
- Upload learning materials.
- Create quizzes and assessments.
- Monitor learner enrollments.
- Track learner performance.
- Manage published courses.

### Administrator
- Manage learners and instructors.
- Approve or reject newly created courses.
- Monitor platform activities.
- View enrollment statistics.
- Generate reports and summaries.
- Maintain overall platform quality.

---

## 3. Key Business Features

### User Management
- Support learner, instructor, and administrator accounts.
- Allow user registration and authentication.
- Maintain user profiles.

### Course Management
- Instructors can create and manage courses.
- Courses require administrator approval before publication.
- Learners can browse approved courses.

### Course Enrollment
- Learners can enroll in available courses.
- Eligibility validation should be performed before enrollment.
- Duplicate enrollments must not be allowed.
- Enrollment status should be maintained.

### Learning Management
- Learners can access course materials after enrollment.
- Learning progress should be tracked.
- Learners can continue learning from previous progress.

### Assessment Management
- Instructors can create quizzes and assessments.
- Learners can attempt assessments.
- Assessment results should be recorded.

### Certification
- Certificates should be generated only after successful course completion.
- Learners should be able to access completed certificates.

### Reporting
- Administrators should be able to view enrollment reports.
- Reports should summarize learning activities.
- Platform usage statistics should be available.

### Error Handling
- All services should return meaningful and consistent JSON responses.
- Appropriate validation messages should be provided for business rule violations.

---

## 4. High-Level Microservices Expectations

The application should follow a Microservices Architecture.

### Expected Services
- learner-service
- instructor-service
- course-service
- enrollment-service
- assessment-service
- certificate-service
- report-service
- api-gateway

### Platform Components
- Centralized Configuration using Spring Cloud Consul
- Service Discovery using Spring Cloud Consul
- Client-side Load Balancing using Spring Cloud LoadBalancer
- API Gateway using Spring Cloud Gateway
- Resilience using Resilience4j Circuit Breaker

---

## 5. Business Rules

- Only registered learners can enroll in courses.
- Only approved courses should be available for enrollment.
- Duplicate enrollment in the same course is not allowed.
- Enrollment eligibility must be validated before confirmation.
- Learning materials are accessible only after successful enrollment.
- Only enrolled learners can attempt assessments.
- Certificates are generated only after successful completion.
- Only instructors can create and manage courses.
- Only administrators can approve or reject courses.
- The system should return standardized JSON responses.

---

## 6. Assumptions

- Authentication and authorization will be implemented.
- Each user has a unique identifier.
- Each course has a unique Course ID.
- Databases contain sample users, courses, and enrollments.
- Services communicate using REST APIs.

---

## 7. Open Questions

1. What are the eligibility criteria for course enrollment?
2. Can learners enroll in multiple courses?
3. Can instructors modify a course after approval?
4. Can learners retake failed assessments?
5. What reports should administrators generate?
6. Should notifications be sent to users?

---

**End of raw-requirement.md**
