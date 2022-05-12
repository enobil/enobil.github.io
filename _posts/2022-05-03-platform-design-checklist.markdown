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
        1. What is the potential max throughput of this API's downstream dependencies?
    1. What should be the availability SLA of this API?
    1. What should be the latency SLA of this API?
1. Use Cases
    1. What are the use cases of this API?
1. Systems Design
    1. What is the end to end flow between the components?
1. Data Model
    1. What are the key data models about this API?
1. Reliability
    1. Are there any edge cases that this solution will not work?
    1. Is there any possibility of data inconsistency?
    1. If the requests are made concurrently will the solution still work?
        1. Or it requires some additional locking design such as optimistic or pessimistic locking?
        1. Can there be some thread safety problems?
1. Availability
    1. How the solution is meeting availability SLA in the technical requirements?
    1. Are there some scenarios that would require downtime?
        1. During the deployments, would there be any downtime or misbehavior?
        1. Can schema/model changes cause downtime during the deployment?
    1. Can this design cause downtime on an external service?
1. Scalability
1. Performance
1. Maintainability
1. Testability
1. Cost
1. Monitoring
1. Alarms
1. Security
1. Development Effort
1. Future Extensibility

Please also check [Platform Implementation Checklist]({% post_url 2022-05-03-platform-implementation-checklist %}).
