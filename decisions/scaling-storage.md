# Storage scaling

## Summary

### Issue
We want our storage layer to scale with minimal manual intervention so that Callisto end users expereince consistent response times

### Decision
WIP

### Status
Gathering information. Trying to understand where the bottlenecks in the system might be from both a component perspective and a process perspective. From there we can look at how we'd detect a bottleneck reaching its acceptable limit and triggering a scaling event

## Details

### Assumptions
We prefer to adopt an existing solution or set of solutions as opposed to developing our own

We prefer a solution that we do not have to patch and upgrade

We prefer a solution where we do not have to explicitly trigger scaling in or out

### Constraints
Whatever we select must work in the context of ACP

### Options
We are researching options now
TODO

### Selected Option
TODO

### Implications
TODO

## Related

### Related decisions
application-scaling.md

### Related requirements