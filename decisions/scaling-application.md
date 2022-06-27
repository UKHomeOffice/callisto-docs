# Application scaling

## Summary

### Issue
We want our application layer to scale with minimal manual intervention so that Callisto end users expereince consistent response times

### Decision
WIP

### Status
Gathering information. Trying to understand where the bottlenecks in the system might be from both a component perspective and a process perspective. From there we can look at how we'd detect a bottleneck reaching its acceptable limit and triggering a scaling event. 

## Details

### Assumptions
We prefer to adopt an existing solution or set of solutions as opposed to developing our own

### Constraints
Whatever we select must work in the context of ACP, Kubernetes and Kafka

### Options
We are researching options now
TODO

### Selected Option
TODO

### Implications
TODO

## Related

### Related decisions
storage-scaling.md

### Related requirements

## Notes
Think about what an atomic processing unit is for each of our services - as data crosses a transactional boundary 