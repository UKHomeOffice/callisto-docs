
# Callisto characteristics

This page describes the non-functional properties of the Callisto system. Characteristics are divided into two categories -

1.  Container - characteristics that any container in Callisto must be designed to embody. Note that a container can be a bespoke piece of software developed by the Callsto delivery team or it can also be a third party product that forms part of the system (eg file storage, database, message queue)
2.  System - characteristics that the Callisto system as a whole must be designed to embody

For each type of characteristic the what, why and how are described. The "how" element is intended to be a rough strawman approach. Over time the "how" will be refined but the "what" describing a characteristic and "why" is the characteristic needed will remain static

## Container characteristics

### fault tolerant

**what** - a container should be designed to expect it's dependencies to fail. A container should be able to gracefully handle one or more of it's external dependencies becoming unavailable for an indeterminate length of time

**why** - Callisto containers communicate over a network. It should be expected network partitions will occur or some other event that renders containers unreachable. Alternatively a container may be reachable but may not acknowledge a request in a timely manner. In all of these cases a failure to communicate successfully with a dependency should not cause the client container to fail which in turn may lead to a failure propagation across cooperating containers in Callisto.

**how** - circuit breaker, retry, timeout. In addition for async requests an acknowledgement must be received. If one is not (following retry and timeout strategy) then the dead letter queue concept can be used to store unsuccessfully published messages

### scalable

**what** - a container should be able to scale to meet the demand profile (**TODO** - usage profile to be defined) and to process a request within the required time (**TODO** - NFR metric to be defined). Additionally the container should be capable of scaling-back as demand drops

**why** - if an individual container cannot scale to ensure that as demand rises it does not underperform against metrics then end users may not be able to complete their tasks in a timeframe that fits with their ways of working. In addition if the container can increase capacity but not decrease capacity as demand drops then unnecessary costs may be incurred dur to the consumption of redundant resources

**how** -

-   file system (cloud provider) -  **TODO**
-   database (cloud provider)-  **TODO**
-   message queue (cloud provider) -  **TODO**
-   business container (Callisto native) - when a container is handling requests where preserving some kind of order is important then the scaling approach should be designed around a partition. A partition is a characteristic of the inputs and outputs of a container which define related data where order needs to be preserved. For example in Callisto a natural partition may be at the person level. In this way there could be several instances of a business container each dealing with a different sets of people

### secure

**what** - a container must be secure such that unauthorised access is not permitted

**why** - Callisto is made up of a series of collaborating but distributed containers. Each container is a potential target for attack. Without securing each container business processes and data could be compromised. Security begins at design-time, takes in development, build and deployment as well as runtime considerations

**how** -

-   code level - employ SAST and DAST testing as part of the CI pipeline. In addition if not covered by the SAST tool - dependency checking should be employed and where possible an automated upgrade approach taken (eg  [Dependabot](https://dependabot.com/))
-   Docker container level -  **TODO**
-   data level - encrypt in transit eg HTTP TLS 1.2 minimum (**TODO** - Home Office preferred cipher set?).  **TODO**- should we encrypt data at rest?
-   config level - encrypt secrets and do not commit passwords or access credentials of any kind to source control. Consider using a container like Azure Key Vault
-   API level - implement rate limiting to curtail impact of DoS attacks. Potentially this feature could be implemented by an API Gateway later

### cohesive

**what**  - a container should not overlap responsibilities with other containers, delegate its responsibilities to other containers or try to execute tasks not related to it.

**why** - if related responsibilities are spread over different containers then the overall system becomes harder to maintain and reason about. This also impacts agility in that it is slower for the system to adapt to change

**how** - containers should encapsulate related business features. Domain Driven Design is one approach for defining container boundaries and responsibilities based around business need (**TODO** - link off to container definition page when done)

### contract-driven

**what** - inter-container communication should happen through well-defined contracts that do not expose internal implementation

**why** - by defining strong contracts it becomes easier to reason about change and the effects on dependent containers. In addition by hiding the internal implementation containers are free to make any changes they like as long as the contract is not impacted.

**how** -

-   sync -  [OpenAPI](https://www.openapis.org/)  (+ Avro?)
-   async -  [AsyncAPI](https://www.asyncapi.com/) +  [Avro](https://avro.apache.org/)  (**TODO** - link to presentation)

### observable

what

why

how  

### manageable

what

why

how

### discoverable 

what

why

how

## System characteristics

### fault tolerant

**what** - the system should be designed to be capable of self-healing such that manual intervention in the event of a container(s) failure is eliminated or kept to a minimum. Care must be given to handling in-flight requests that were being processed by a container that crashed. If any transactions are logically distributed across container boundaries then consideration may need to be given to how data consistency is maintained in the event of a crash-repair cycle

why -

how

### secure

what

why

how -

-   cluster level
-   cloud provider level - least access

	
	
