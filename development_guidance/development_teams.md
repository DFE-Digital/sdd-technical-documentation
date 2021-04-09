# Development Teams

This guide contains various concepts which form a set of expectations we have on how our development teams will operate.

We base our ways of working on the [GDS Code of Practice](https://www.gov.uk/government/publications/technology-code-of-practice/technology-code-of-practice), and the rest of this document covers some extra detail specific to our teams.

## Full Stack, Vertical Slices

We aim to create teams of full stack developers, rather than creating teams that specialise in disciplines like front end development, testing, devops etc.

We do this as we believe it allows us to create teams that excel at delivering rapid change (and therefore value) to users.

In order to maximise the benefits of creating teams this way, we need to organise our work deliberately to play to these strengths -
 - Work should sliced 'vertically' meaning a given task is implemented in the back end and front end, and tested
 - Confident testing is part of the acceptance criteria for a given task
 - Tasks are refined to the point where any member of the development team could action them, from new features to infrastructure changes to bugfixes
 - Teams should regularly practice pairing and actively work to avoid knowledge silos
 - Work should have a clear path to production and deploy as often and quickly as possible

## Stacks and Tools

Where we mention we 'prefer' to use certain tools or packages, teams are free to deviate if it is deemed necessary, for other tools or stacks we consider this section mandatory.

We develop all new digital services as single repository .NET Core (latest Long Term Support version) MVC applications, using Razor for templating. Using the latest .NET Core version is acceptable in special circumstances, but prefer the long term supported version if possible.
We have a number of digital services with low complexity requirements that interact with the same central data store, so a simple and consistent stack allows us to develop to a common standard and eventually pass ownership to a centralised live support team.

We use XUnit for unit and integration tests, we prefer to use Moq for mocking interfaces and Fluent Assertions for chaining assertions (though it is arguably an anti-pattern, we prefer the readability it imparts).

We prefer Cypress for UI testing (which we feel covers behavioural testing needs).

We use GitHub for source control and code in the open.

We build using a mixture of Azure DevOps and CloudFoundry, which largely is a historical distinction, and host on GovPaas or Azure. New digital services will use CloudFoundry and GovPaas.

Our digital services all interact with TRAMS as a persistence layer via a RESTful JSON API. No new service will need to manage its own persistence layer, but may introduce new requirements to the TRAMS API.

## Pairing

We believe pair programming to be a particularly valuable tool for a team to ensure knowledge silos form less frequently, to build capability and domain knowledge across the development team, and to promote high quality coding practices. We recommend teams try to pair on any task where doing so is sensible.

[Learn more about how to pair program and the benefits](https://martinfowler.com/articles/on-pair-programming.html)

## Code Review

We believe it is important no developer works in isolation, and that all work is understood by the team (the scope and impact of the changes, the team agrees the implementation and code are of high quality).

Pull requests are one strategy to help achieve this, if the code was worked on solo. We believe pairing, collaboration and showcasing can also be adequate ways to introduce this shared acceptance of any given change, and we encourage teams to balance speed of change with shared concern over quality in a way that works most optimally for them.


## Common Patterns

Teams are encouraged to follow [Clean Architecture](https://g.co/kgs/eW2Cih) as a baseline, this is because Clean Code aims to create a consistent code style that optimises enabling change and maintenance. By practicing TDD over the course of the implementation, we expect all individual code components to be unit tested in isolation, and component interactions to be tested in a mockist style.
As a further testing baseline, teams are encouraged to ensure there are tests that give confidence the code components work when orchestrated as a whole system, and tests that give confidence the behaviour of the system for users is correct using something like Cypress.

Additional testing around things like integration with external systems, contract testing, infrastructure testing or any other tests the team feel will be valuable are to be added at the team's discretion.
