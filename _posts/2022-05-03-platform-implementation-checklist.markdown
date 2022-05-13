---
layout: post
title:  "Platform API Implementation Checklist"
date:   2022-05-03 10:39:07 +0300
categories: jekyll update
---

1. Correctness
    1. Is there any edge case that this implementation wouldn't work correctly?
    1. Is there any function that is mutating its input argument where it is not supposed to?
    1. Is there any missing equals() and hashCode() definition that would cause false object equality?
        1. e.g. Any use of data structures like map or set with an object but without defining equals() etc?
    1. (javascript) Is there any if condition that requires more strict check e.g. === instead of ==, or if (x !== undefined) instead of just if (x)?
    1. (javascript) Is there any async method call lacking await?
    1. (javascript) Is there any mixed use of async/await and then/catch?
1. Logging
    1. Is there a proper logging framework in use?
    1. Are the logs using the proper log levels?
    1. Are the logs utilizing structured logging, e.g. as JSON?
    1. When logging an exception, is the stack trace properly visible in the logs?
    1. Are the logs sufficient to troubleshoot an unexpected issue?
    1. Are the logs including details about input parameters or only printing a generic message without any/sufficient parameter values?
1. Performance
    1. Is there a suitable use case for caching, e.g. some downstream API responses?
1. Unit Tests
    1. Is there full branch coverage?
    1. (~Devops) Is the code review tool integrated with coverage results, so that code review can make e.g. missed branches clearly visible?
    1. During the verification of function calls, besides the count, are the parameters also verified?
1. Testability
    1. Are there any concrete class that makes an untestable static call or system call (e.g. System.now() / new Date())?
    1. Are all dependencies of the class injected through dependency injection?
1. Configuration
    1. If environment variables are being used, is there any mismatch between where they're configured in the configs and used in the code?
        1. Any type mismatches e.g. forgetting to parse as number in the code?
1. Naming (readability)
    1. For a constant with a time unit (e.g. seconds) or size unit (e.g. bytes), is the naming clear about what is the unit of it?
1. Coding style (readability)
    1. Is there a linter configured to automate code style checks?
    1. Is the indentation consistent?
1. Dependency management
    1. Is there any deprecated version of a dependency trying to be added in a new change?

Please also check [Platform Design Checklist]({% post_url 2022-05-03-platform-design-checklist %}).
