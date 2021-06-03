# Continuous Integration and Deployment

## Continuous Integration

CI is the concept that when code exists in source control, and multiple developers (or even teams) are working on making changes, those changes are integrated with the whole product and tested for regression immediately.

There are a number of small factors to understand in order for a development team to be 'continuous'.

### Branching and Merging

We expect teams will use branches for development work, branches should be short lived and specific to the pull request.

We expect teams to work in vertical slices where possible and aim to create pull requests that are as small as possible, and with as narrow scope as possible.

We expect branched to include passing tests for all work, and all existing tests to have been maintained and to pass.

We expect when a branch is merged into `main`, all test suites are run and pass.

### Other considerations

Teams are encouraged to find ways of working that allow them to produce code of the highest quality, as quickly as possible.

Teams may adopt strategies like trunk based development or other approaches at their discretion, and are expected to utilise pair programming, code reviews and automated testing to give themselves confidence their changes are not introducing regressions and are providing value to users.

Teams are also expected to utilise strategies to provide [quality assurance](./quality_assurance.md) within their teams to build stakeholder and user confidence.

## Continous Deployment

We expect at the point where changes have been merged into `main` and all tests passed that the `main` branch we be automatically deployed to `dev` and `staging` environments.

We expect a manual approval step before deploying to `production` environments, teams will be responsible for ensuring changes are deployed to production as soon as possible after merging, see [quality assurance](./quality_assurance.md)

Teams are strongly encouraged to decouple deployment from releases, and should ensure that they are utilising the ability to seamlessly deploy changes provided by their chosen CI/CD tool so that deployments can be made as soon as changes are assured.

Teams may choose to adopt feature-flagging strategies if there is a need to control user access to changes, though often changes are not disruptive to user experience and can be deployed, and communications sent regularly such as release notes can cover retro-active changes.
