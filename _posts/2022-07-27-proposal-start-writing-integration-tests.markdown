---
layout: post
title:  "Proposal To Start Writing Integration Tests"
date:   2022-07-27 11:00:00 +0300
categories: jekyll update
---

## Proposal

I'm proposing to start writing integration tests if the project doesn't have already.

Integration tests ensure the implementation is actually working reliably and correctly in various scenarios, end-to-end.

Unit tests also provide a fundamental level of testing, but it still has blind spots for majority of correctness and reliability problems. In other words, the end-to-end functionality may not work at all even if there is a high unit test coverage and all unit tests passing. Because the actual end-to-end functionality involves much more potential points of failure that unit tests will never be able to cover by design. Therefore, integration tests are cruical to make sure a service API is actually working correctly in various scenarios. Unit tests have their place and benefits too, unit tests should be still written for other reasons, but the actual guarantee about "whether if a service is working or not" can only be provided by integration tests.

There are numerous high impact reasons to write integration tests as part of the software development workflow.

# Prevents Implementation Issues At The Time Of Development

An implementation issue could be a bug, a configuration problem, or any other part of the implementation that ends up with failure of the service API use cases.

In the SDLC (software development lifecycle), an implementation change goes through several steps such as development on local environment, developer testing, deployment to dev environment, verification in dev environment, deployment to test environment, tests done by testers, deployment to preprod environment, UAT tests, deployment to prod, for example.

When there is an issue with the implementation, it can get noticed in any of the SDLC phases. In the best case, the issue can be noticed during the first "development on local environment" phase. In the worst case, the issue can be noticed after prod deployment, by the prod users.

When integration tests are written together with code changes at the time of development, it will uncover many problems with the implementation.

* This minimizes the cost of fixing any implementation problem since the change is at the earliest - development phase.
* This maximizes the quality of the product under development.
* This maximizes the trust of the stakeholders by shipping significantly reliable software and systems.
* This allows all involved stakeholders to use their time on meaningful work and have impact, instead of wasting time on problems that could be avoided during the development (opportunity cost of the other stakeholders' wasted potential).
* This prevents shipping a change that doesn't work.

# Prevents Implementation Issues After The Time Of Development

Once an implementation change is merged to codebase and deployed to all environments, it can still get broken by the future changes on the same codebase.

Manual regression tests are very costly and hard to have sufficient coverage because the codebase will always have more scenarios to cover over time, and those scenarios are often mixture of technical points of failure and functional points of failure. Due to its low feasibility, manual regression tests are either not done at all or done rarely with a minimal scope, in practice in the real world projects. Because of that, manual regression tests often doesn't provide any significant mechanism to ensure quality.

Integration tests on the other hand, have zero cost to re-run. Usually integration tests are configured to run in CI/CD pipelines as a separate pipeline step, or as part of the build. This way, each change triggers a regression test on its own in real time. Since it is automated it would take only minutes instead of days with the manual regression tests.

* This prevents any breaking change to be introduced.
* Allows building blocks on top of each other in a solid & reliable manner, rather than as jenga blocks.

# Unblocks Achieving CI/CD (Continuous Integration / Continuous Delivery)

CI/CD pipelines automate deployment of the changes instead of requiring devs and other stakeholders to manually deploy changes to each environment. This saves time of developers, as well as makes the changes ready in the environments as soon as possible, to uncover issues as soon as possible, minimizing the time loss until issues are observed.

CI/CD pipelines highly accelerate release cycles to environments and make changes quickly available in required environments. This minimizes the time team members are getting blocked by waiting for each others' change to be deployed to an environment.

However in order to automate deployments to the environments, there needs to be a some quality gate that would decide if the change is a breaking change or not. Exactly at this point, implementation tests enable CI/CD pipelines to decide if a change is working or not, or breaking the existing code or not.

By simply having an additional check step to run integration tests in the pipeline, integration tests can be integrated with CI/CD pipelines to achieve deployment automation. Once a code review is approved and merged, it will be available in the lower environments as soon as possible. (For the higher environments, there are other quality gate mechanisms like metrics and baking time, which can be covered in another proposal about CI/CD specifically.)

Another way to integrate integration tests with a CI/CD pipeline is triggering the integration test run from a build step. While it is suboptimal compared to the first option, it still provides a simple fallback option if the first option isn't available for a reeason.

Automated CI/CD pipelines can be easily achieved once the service API has sufficient integration test coverage.

* This allows devs to quickly become unblocked when they are blocked by deployment of some other change by another dev.
* This allows issues to be discovered as quick as possible in the environments by making the change available as soon as possible as long as it is safe.
* This allows devs to focus their time, effort and attention on productive work instead of manually triggering and tracking deployments, moving changes between environments for each change for each environment.

# Integration Tests Are Simple Yet Extremely Effective

When writing unit tests, a developer needs to consider what dependencies a class and function has. For each of those dependency classes, functions, refereneces, a developer needs to find how to mock them. Sometimes it will need mocking a function with a stub (mock function implementation) that is stateful. Sometimes it will require using a 'spy' construct to perform further verifications. All of these constructs are beneficial for the purpose of unit testing but also adds significant complexity when simply maintaining a codebase.

When writing integration tests, a develoepr needs to only consider the request payload, the expected response payload, and how to clean up side effects after the test execution. This is often simpler than writing unit tests since there is no need for a full understanding of interdependencies inside the codebase. It only needs a high level understanding of the systems design of the service API under test.

Integration tests are simple yet extremely effective at end-to-end testing functionality of a service API.

* This allows teams to easily start writing integration tests.
* This allows teams to easily add more integration test coverage, easily maintain their codebase, without requiring deep analysis of the implementation code for each test.
* The simplicity of the integration tests allows teams to focus on making the most effective real world use of the tests, simply and directly, instead of spending effort on e.g. how to write a complex unit test requiring e.g. 5 different unit test specific abstractions (mocks,stubs,spies,static reference workarounds,...), which at the end of the day can fail to detect significant issues.

# Next Steps

1. Start with a POC integration test, with below minimal logic for example (any test framework can be used e.g. JUnit, TestNG, jest):
    1. Make a service call to create a test entity
    1. Make another service call to retrieve the entity back from the service API
    1. Make one last service call to clean up the test entity from the service API
1. Expand to more use cases, leveraging the test framework
1. DevOps improvements
    1. Integrate with CI/CD pipeline
    1. Enable notifications to the team when the pipeline integ test run fails
    1. Optionally enable automatic promotions in CI/CD pipeline up to some environment based on integ test pass status
