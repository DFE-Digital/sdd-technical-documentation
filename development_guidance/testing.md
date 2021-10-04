# Testing Strategy

This document presents principles behind our testing methodology. There is more specific guidance available in the SDD Testing Strategy document hosted in the DfE Sharepoint instance. The link to this is [here](https://educationgovuk.sharepoint.com.mcas.ms/sites/ServiceDeliveryDirectorate/Shared%20Documents/Forms/AllItems.aspx?cid=e7a83aa3%2D8334%2D41d2%2Dba2b%2D7255c0fe550f&RootFolder=%2Fsites%2FServiceDeliveryDirectorate%2FShared%20Documents%2FSDD%20Cross%20cutting%2FTechnology&FolderCTID=0x012000265B42F1C27524438F6B1CC14FCF9E05).

## Types of Testing

We refer to the ['test pyramid'](https://martinfowler.com/bliki/TestPyramid.html) as a convenient way to understand common types of testing we can perform, and to also articulate how different types of testing can be faster or slower to implement and execute, and can be more or less costly in terms of investment to create and maintain.

Items at the top of the pyramid are typically slower and more costly, so we aim to write fewer of those tests, though they also typically provide high coverage and confidence, so are a useful tool.

The testing pyramid and how we test each layer are elaborated further in the SDD Testing Strategy document.

## Test Properties

Credit to [Kent Beck](https://medium.com/@kentbeck_7670/test-desiderata-94150638a4b3)

**Isolated** — tests should return the same results regardless of the order in which they are run.

**Composable** — if tests are isolated, then I can run 1 or 10 or 100 or 1,000,000 and get the same results.

**Fast** — tests should run quickly.

**Inspiring** — passing the tests should inspire confidence
Writable — tests should be cheap to write relative to the cost of the code being tested.

**Readable** — tests should be comprehensible for reader, invoking the motivation for writing this particular test.

**Behavioral** — tests should be sensitive to changes in the behavior of the code under test. If the behavior changes, the test result should change.

**Structure-insensitive** — tests should not change their result if the structure of the code changes.

**Automated** — tests should run without human intervention.

**Specific** — if a test fails, the cause of the failure should be obvious.

**Deterministic** — if nothing changes, the test result shouldn’t change.

**Predictive** — if the tests all pass, then the code under test should be suitable for production.

## What should I test?

It's important we understand the goal of testing, which ultimately is to provide enough automated feedback that we have confidence our change will not break.

The level and types of testing we need to implement in order to generate this confidence vary from team to team and project to project, as confidence is a factor of code quality and design, team collaboration and safety and test effectiveness.

A team that works extremely well together and delivers high quality output but has no tests, or lots of slow tests, or a test suite that is ineffective may find they have still confidence to release quickly and have a very low change failure rate due to built up trust and knowledge. Should a new team take on support of that service they may find that without that 'built up' safety they need to establish a testing baseline. A team may also conversely find that starting with a test first mindset allows them to build that trust and ultimately move faster. There is no right or wrong way to implement test strategies, and no minimum or maximum level of test coverage.

Each team will use Test Driven Development to implement tests using the testing pyramid as a guide, to the level that gives them confidence to release changes rapidly. They will use the tools and metrics at their disposal to examine their test strategy constantly, and to iterate upon it to improve and balance _lead time to change, deployment frequency and change failure rate_.

## Test Driven Development

All developers will be active practitioners of [Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development), and many resources exist describing how to develop effectively using TDD and [why we'd choose that paradigm](https://www.codica.com/blog/test-driven-development-benefits/). We believe that practicing TDD deliberately and consistently provides code that is _easier to change_ and therefore easier to maintain, which is ultimately what will allow our code to have a long shelf life - optimising to keep cost of change low will allow us to adapt to changing user needs over time.
