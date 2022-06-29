# Environment variable configuration

## Summary

### Issue
We want our applications to be configurable beyond artifacts/binaries/source, such that one build can behave differently depending on its deployment environment.

  * To accomplish this, we want to use environment variable configuration.

  * We want to manage the configuration by using files that we can version control.

  * We want to provide some developer experience support, such as knowing what can be configured and any relevant defaults.


### Decision
TODO

### Status

## Details

### Assumptions
We favor separating the application code and environment code. We assume the app needs to work differently in different environments, such as in a development environment, test environment, demo environment, production environment, etc.

We favor the industry practice of "12 factor app" and even more the related practice of "15 factor app".

### Constraints
We want to keep secrets out of our source code management (SCM) version control system (VCS).

We want to aim for compatibility with popular software frameworks and libraries.

### Options

### Selected Option

### Implications

## Related

### Related decisions

### Related requirements