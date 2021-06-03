# Quality Assurance

How do we expect teams to assure the quality of their code, and the user value of their changes?


## Code Quality

The most general and accepted strategy is a combination of peer review of work at the pull-request stage, and to use automated code tools such as
 - Static analysis
 - Codescene style analysis tools
 - Linting

 We also believe an important tool in the quality toolbox is pair programming, and subscribe to the belief that PR stage code review can be more of a detriment to quality than a benefit, or potentially a time gate that isn't really adding much.

 We balance this with the expectation that no developer works alone, so code that was written in isolation should undergo at least a peer review.

 Code written in a pair or group is assumed to have been reviewed during development and teams can use their own discretion in this case.

## Change Quality

Our teams have subject matter experts and product specialists embedded within them, and often access to end users to verify the value of each change.

Before source code integration, and when integrated, we expect comprehensive test suites which give confidence of code components, component integration, system stability and user journey.

Move much of our quality focus into this 'pre deployment' testing phase, and approaching our code in a test driven way, with a testing first mindset, we invest heavily in confidence our code does what _is right for the end user_.

Once these investments are in place, it allows the team to move incredibly quickly when changing or adding to this strong foundation, which lessens the need for time-consuming manual verification, especially when changing established code,

For new features in particular, but for all deliverables, we expect teams to utilise the availability of users and specialists to verify the work meets the understood user need, and does not introduce regressions or degradation of experience. An opportune time is when the change is deployed to the production-like environment, before deployment (or deployed behind a feature flag preventing unsuspecting users from encountering any potential degradation).

When utilising feature flagging, and testing proves that the system still otherwise behaves as it did, it's advisable to deploy into production as soon as possible and assure that change in a real production environment. This helps ensure you're verifying the real experience of users, and any issues that arise can be quickly fixed forward rather than rolling back.

It is ultimately for teams to decide how to blend these strategies and even techniques not described here, to ensure they balance providing the most consistent and valuable user experience with delivering changes and additions. 
