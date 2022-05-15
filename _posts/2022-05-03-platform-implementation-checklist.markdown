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
1. Error handling
    1. Are there sufficient input validations?
    1. (lambda) If there is an internal server error, is lambda exit status properly unsuccessful, or is it exiting with successful code by mistake?
    1. Retries
        1. Custom exceptions should have information whether they're retriable or not.
        1. (SQS) When handling an SQS message batch, successfully processed messages must be deleted. Messages with a retriable error shouldn't be deleted. Messages with a non-retriable error should be deleted and moved to a dead-letter queue.
    1. Rollback
        1. As part of error handling, is it missing any required roll back operations?
            1. (aws state machine & lambda) It is good to perform this rollbacks within the lambda in form of try catch but that's not always sufficient. Lambda can also get terminated due to timeout, out of memory, or another crash. In those cases, state machine error handling should perform the necessary rollbacks.
1. Logging
    1. Is there a proper logging framework in use?
    1. Are the logs using the proper log levels?
    1. Are the logs utilizing structured logging, e.g. as JSON?
    1. When logging an exception, is the stack trace properly visible in the logs?
    1. Are the logs sufficient to troubleshoot an unexpected issue?
        1. Might be useful to have trace/debug logs before and after major function calls.
    1. Are the logs including details about input parameters or only printing a generic message without any/sufficient parameter values?
    1. Is the log message understandable for a person with limited context?
1. Performance
    1. Is there a suitable use case for caching, e.g. some downstream API responses?
1. Unit Testability
    1. Are there any concrete class that makes an untestable static call or system call (e.g. System.now() / new Date())?
    1. Are all dependencies of the class injected through dependency injection?
1. Unit Tests
    1. Is there full branch coverage?
    1. Each test must target only one concrete class as the subject and mock its dependencies. Are there cases where one test tries to test multiple classes at once? If so the concrete implementation must be refactored to have separate classes for different levels of responsibilities.
    1. (~Devops) Is the code review tool integrated with coverage results, so that code review can make e.g. missed branches clearly visible?
    1. During the verification of function calls, besides the count, are the parameters also verified?
1. Ad hoc e2e tests
    1. Is the change tested end to end, at least manually?
1. Integration tests
    1. Is the change covered with integration tests?
1. Configuration
    1. If environment variables are being used, is there any mismatch between where they're configured in the configs and used in the code?
        1. Any type mismatches e.g. forgetting to parse as number in the code?
1. Naming (readability)
    1. For a constant with a time unit (e.g. seconds) or size unit (e.g. bytes), is the naming clear about what is the unit of it?
1. Coding style (readability)
    1. Is there a linter configured to automate code style checks?
    1. Is the indentation consistent?
    1. Are the constant values extracted to a defined constant, instead of used directly as a magic number/literal?
    1. Instead of nested ifs, prefer at most one level of nested ifs and return quickly.
    1. Instead of one long function, divide into smaller helper functions or even to helper classes. That will provide not only readability and maintainability but also testability.
1. Dependency management
    1. Is there any deprecated version of a dependency trying to be added in a new change?
1. Maintainability & Extensibility
    1. Can it be made more generic for future reuse?
1. Configuration
    1. Lambda
        1. Is it using a proper timeout value?
        1. Is it using a proper memory size?

Please also check [Platform Design Checklist]({% post_url 2022-05-03-platform-design-checklist %}).
