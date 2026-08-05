# clarification.md

# Infy LearnX – Clarifications and MVP Decisions

| Area | Clarification Question | MVP Decision |
|------|-------------------------|--------------|
| User Roles | What user roles are supported? | Three roles: Learner, Instructor, and Administrator. |
| User Access | Can users perform actions outside their role? | No. Each role can access only its own features. |
| Learner Registration | Can anyone register as a learner? | Yes, any user can register as a learner. |
| Course Enrollment | Can a learner enroll in any course? | Yes, provided the course is approved and available. |
| Duplicate Enrollment | Can a learner enroll in the same course more than once? | No. Duplicate enrollments are not allowed. |
| Enrollment Limit | Is there a limit on active enrollments? | No limit for the MVP. |
| Course Approval | Who approves courses? | Only administrators approve courses before publication. |
| Course Updates | Can instructors modify approved courses? | Yes. Updates require re-approval if they affect published content. |
| Learning Materials | Who can access course materials? | Only enrolled learners. |
| Assessments | Who can attempt assessments? | Only learners enrolled in the corresponding course. |
| Assessment Retake | Are retakes allowed? | Yes, one retake is allowed for the MVP. |
| Course Completion | When is a course considered complete? | After all required learning content and assessments are completed successfully. |
| Certificate | When is a certificate generated? | Automatically after successful course completion. |
| Reports | Who can view reports? | Administrators can view platform reports; instructors can view their course reports. |
| Microservice Boundaries | What services are expected? | learner-service, instructor-service, course-service, enrollment-service, assessment-service, certificate-service, report-service, api-gateway. |
| Data Ownership | Which service owns which data? | Each service owns its own business data and database. |
| Service Communication | How do services communicate? | Synchronous REST communication. |
| API Gateway | How do clients access services? | All client requests go through the API Gateway. |
| Service Discovery | How are services located? | Spring Cloud Consul is used for service registration and discovery. |
| Centralized Configuration | Where are shared configurations stored? | Spring Cloud Consul Configuration. |
| Load Balancing | How are multiple service instances handled? | Spring Cloud LoadBalancer provides client-side load balancing. |
| Circuit Breaker | What happens if a dependent service is unavailable? | Resilience4j returns a simple fallback response. |
| Data Persistence | How is data stored? | Each microservice has an independent database populated with demo data. |
| Error Handling | How are errors returned? | Standardized JSON responses with meaningful messages. |
| Testing | What testing assumptions are made? | Unit and integration testing are performed using sample/demo data. |

---

## General MVP Assumptions

- Authentication is basic and role-based.
- Every learner, instructor, and course has a unique identifier.
- All microservices are independently deployable.
- REST APIs are used for communication between services.
- Sample data is preloaded for demonstrations.
- The focus is on functionality rather than enterprise-grade security or scalability.

---

**End of clarification.md**
