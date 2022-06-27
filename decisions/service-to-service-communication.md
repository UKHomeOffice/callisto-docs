# Service to service communication

## Summary

### Issue

We want our system to be resiliant such that if a service moves into an unhealthy state other services in the system are not compromised. We also want our services to be able to avoid getting swamped with requests so that they do not become unresponsive and negativly impact the end user's experience

In addtion we do not want to have to change a producer service if another service needs to be invlvoed in a workflow

choreography

### Decision
WIP

### Status
Gathering information. 

## Details

### Assumptions
We prefer to adopt an existing solution or set of solutions as opposed to developing our own

### Constraints
Whatever we select must work in the context of ACP, Kubernetes and Kafka

### Options
1. Event driven
2. REST API

### Selected Option
Event driven

### Implications
Hard to debug 
Hard see workflow dependencies
Eventual consistency
Event ordering
Duplicate events

## Related

### Related decisions
scaling-application.md

### Related requirements
resilaiant
independent deplyoment
scaling

## Notes
