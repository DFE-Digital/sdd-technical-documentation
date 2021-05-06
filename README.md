# SDD Technical Documentation

This repository is intended to provide a central point for technical reference for services specific to SDD.

## [Architecture Decision Records](./adrs)

Details how and why we have chosen to our SDD specific architecture.

## [Development Guidance](./development_guidance)

Documentation covering the ways our development teams work and the technical standards we work against.

This set of documentation is division specific, we base our standards on [DfE Digital Guidance](https://technical-guidance.education.gov.uk/documentation/guides/default-technology-stack.html#the-dfe-digital-technology-stack), any areas not covered by the DfE Digital guidance or contradictory to this documentation, we take this set of documentation as precedent.

## [TRAMS API Document](./trams_api_spec)

The TRAMS (Trust and Academy Management System) system acts as a data source for SDD's digital services supporting Academies and Free Schools. We're working on a set of RESTful APIs to allow digital services to consume data from TRAMS and to feed back data necessary for completing projects.

The document is maintained here as a central contract so systems can develop against a consistent API while it is in development. It's generated and viewable using (Swagger)[swagger.io]

Use `docker run -d -p 80:8080 -v $(pwd):/tmp -e SWAGGER_FILE=/tmp/trams_api_spec/trams_api_spec.yaml swaggerapi/swagger-editor` to run the editor with the trams api spec pre loaded, on localhost:80
