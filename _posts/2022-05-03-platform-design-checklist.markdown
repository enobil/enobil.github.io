---
layout: post
title:  "Platform API Design Checklist"
date:   2022-05-03 10:39:07 +0300
categories: jekyll update
---

1. Functional Requirements
    1. What is the business problem this API will solve?
1. Technical Requirements  
    1. How many requests are expected per second for this API?  
    1. What should be the TPS limit of this API?  
        1. What is the potential max throughput of this API's downstream dependencies? (Since TPS limit of this API has to be lower than that.)
    1. What should be the availability SLA of this API?
    1. What should be the latency SLA of this API?
1. Use Cases
    1. What are the use cases of this API?
1. Systems Design
    1. What is the end to end flow between the components?
    1. Should this API have a sync flow or async flow? i.e. Maybe for large volume requests it will take too much time to process and it should be an async API instead, that just starts an async process.
1. Database Design
    1. Is the DB design normalized or denormalized?
    1. If denormalized, will this design be able to maintain the denormalized database properly under all use cases, will it be able to remain consistent?
    1. If denormalized, is it really needed? Because normalized db design is more maintainable.
1. Data Model
    1. What are the key data models about this API?
1. Reliability
    1. Are there any edge cases that this solution will not work?
    1. Is there any possibility of data inconsistency?
    1. If the requests are made concurrently will the solution still work?
        1. Or it requires some additional locking design such as optimistic or pessimistic locking?
        1. Can there be some thread safety problems?
    1. If the requests are received out of order, would this design still be able to handle as if the order of requests were correct?
1. Availability
    1. How the solution is meeting availability SLA in the technical requirements?
    1. Are there some scenarios that would require downtime?
        1. During the deployments, would there be any downtime or misbehavior?
        1. Can schema/model changes cause downtime during the deployment?
    1. Can this design cause downtime on an external service?
1. Scalability
    1. How much max potential throughput this API can achieve with this design?
        1. If this is a redesign, how does it compare with the existing?
    1. What are the bottlenecks that are limiting the potential throughput?
    1. Does this solution require some capacity planning?
        1. Can this solution handle the projected load of upcoming years?
    1. What is the scaling model (serverless / horizontal scaling / vertical scaling)?
    1. On the flow of the systems design, what is the limitation of the other components in the flow in terms of
        1. What is the allowed execution time / latency budget for each component? (e.g. APIGW has 30 seconds hard limit)
        1. What is the payload size limit of each component? (e.g. 6MB limit on Lambda sync invocation, 256KB limit on Lambda async invocation)
1. Performance
    1. How the solution is meeting latency SLA in the technical requirements?
1. Maintainability & Future Extensibility
    1. How easy is it to handle new use cases, or adapt to modifications on existing use cases?
1. Testability
    1. How this solution will be unit tested?
    1. How this solution will be ad-hoc end to end tested?
    1. How this solution will be integration tested (automated)?
1. Cost
    1. What are the projections on infrastructure cost?
    1. If this is a redesign, how this design's infra. cost compare with the previous one's?
1. Monitoring
    1. Performance metrics
        1. How latency of the API will be monitored?
    1. Reliability metrics
        1. How error count and error rate of the API will be monitored? (Internal server errors)
    1. Usage metrics
        1. How the quota utilizations of this API will be monitored?
            1. Utilization of downstream services, i.e. utilized TPS percent out of the TPS limit
    1. Custom metrics
        1. What should be the KPIs (key performance indicators) for this API, also considering the business impact?
1. Alarms
    1. Are alarms planned to be created for each monitored key metric?
    1. How the team will get notified by the alarms and resolve?
        1. Email notifications
        1. Ticketing system integration (e.g. JIRA REST API)
1. Security
    1. What is the payload size limit for this API?
    1. How the TPS limit will be applied for this API?
        1. API level (basic, minimum requirement)?
        1. Per-client (advanced)?
    1. Is the data encrypted at transit and at rest?
1. Development Effort
    1. If long term outcome of multiple design options are equivalent, how they compare about the effort needed to implement?
1. Backwards compatibility / Impact (if this is a change on an existing API)
    1. How this design makes sure none of the existing clients will break?
1. Change management
    1. What would be the procedure to roll back this change?
1. DevOps
    1. How the integration tests will be included in the CI/CD pipelines?

Please also check [Platform Implementation Checklist]({% post_url 2022-05-03-platform-implementation-checklist %}).
