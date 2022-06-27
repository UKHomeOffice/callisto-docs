# Callisto characteristics

This page describes the non-functional properties of the Callisto system. Characteristics are divided into two categories -

1.  Service - characteristics that any service in Callisto must be designed to embody. Note that a service can be a bespoke piece of software developed by the Callsto delivery team or it can also be a third party product that forms part of the system (eg file storage, database, message queue)
2.  System - characteristics that the Callisto system as a whole must be designed to embody

For each type of characteristic the what, why and how are described. The "how" element is intended to be a rough strawman approach. Over time the "how" will be refined but the "what" describing a characteristic and "why" is the characteristic needed will remain static

## Service characteristics

### fault tolerant

**what** - a service should be designed to expect it's dependencies to fail. A service should be able to gracefully handle one or more of it's external dependencies becoming unavailable for an indeterminate length of time

**why** - Callisto services communicate over a network. It should be expected network partitions will occur or some other event that renders services unreachable. Alternatively a service may be reachable but may not acknowledge a request in a timely manner. In all of these cases a failure to communicate successfully with a dependency should not cause the client service to fail which in turn may lead to a failure propagation across cooperating services in Callisto.

**how** - circuit breaker, retry, timeout. In addition for async requests an acknowledgement must be received. If one is not (following retry and timeout strategy) then the dead letter queue concept can be used to store unsuccessfully published messages

### scalable

**what** - a service should be able to scale to meet the demand profile (**TODO** - usage profile to be defined) and to process a request within the required time (**TODO** - NFR metric to be defined). Additionally the service should be capable of scaling-back as demand drops

**why** - if an individual service cannot scale to ensure that as demand rises it does not underperform against metrics then end users may not be able to complete their tasks in a timeframe that fits with their ways of working. In addition if the service can increase capacity but not decrease capacity as demand drops then unnecessary costs may be incurred dur to the consumption of redundant resources

**how** -

-   file system (cloud provider) -  **TODO**
-   database (cloud provider)-  **TODO**
-   message queue (cloud provider) -  **TODO**
-   business service (Callisto native) - when a service is handling requests where preserving some kind of order is important then the scaling approach should be designed around a partition. A partition is a characteristic of the inputs and outputs of a service hich define related data where order needs to be preserved. For example in Callisto a natural partition may be at the person level. In this way there could be several instances of a business service each dealing with a different sets of people

### secure

**what** - a service must be secure such that unauthorised access is not permitted

**why** - Callisto is made up of a series of collaborating but distributed services. Each service is a potential target for attack. Without securing each service business processes and data could be compromised

**how** -

-   code level - employ SAST and DAST testing as part of the CI pipeline. In addition if not covered by the SAST tool - dependency checking should be employed and where possible an automated upgrade approach taken (eg  [Dependabot](https://dependabot.com/))
-   container level -  **TODO**
-   data level - encrypt in transit eg HTTP TLS 1.2 minimum (**TODO** - Home Office preferred cipher set?).  **TODO**- should we encrypt data at rest?
-   config level - encrypt secrets and do not commit passwords or access credentials of any kind to source control. Consider using a service like Azure Key Vault
-   API level - implement rate limiting to curtail impact of DoS attacks. Potentially this feature could be implemented by an API Gateway later

### cohesive

**what**  - a service should not overlap responsibilities with other services, delegate its responsibilities to other services or try to execute tasks not related to it.

**why** - if related responsibilities are spread over different services then the overall system becomes harder to maintain and reason about. This also impacts agility in that it is slower for the system to adapt to change

**how** - services should encapsulate related business features. Domain Driven Design is one approach for defining service boundaries and responsibilities based around business need (**TODO** - link off to service definition page when done)

### contract-driven

**what** - inter-service communication should happen through well-defined contracts that do not expose internal implementation

**why** - by defining strong contracts it becomes easier to reason about change and the effects on dependent services. In addition by hiding the internal implementation services are free to make any changes they like as long as the contract is not impacted.

**how** -

-   sync -  [OpenAPI](https://www.openapis.org/)  (+ Avro?)
-   async -  [AsyncAPI](https://www.asyncapi.com/) +  [Avro](https://avro.apache.org/)  (**TODO** - link to presentation)

### observable  
###manageable
### discoverable 
**Question** - (is this really needed - Stephen?)

## System characteristics

### fault tolerant

**what** - the system should be designed to be capable of self-healing such that manual intervention in the event of a service(s) failure is eliminated or kept to a minimum. Care must be given to handling in-flight requests that were being processed by a service that crashed. If any transactions are logically distributed across service boundaries then consideration may need to be given to how data consistency is maintained in the event of a crash-repair cycle

why -

how

### secure

what

why

how -

-   cluster level
-   cloud provider level - least access

### modular


Service characteristics

	fault tolerant

	scalable

	secure

	cohesive

	discoverable (is this really needed - Stephen?)

	contract/API
	
	observable
	
	managable
	
	
System considerations

	infrastructure
	
	configuration
	
	release (planned and unplanned) deployment 
	
	DR
	
System characteristics

	eventual consistency (does this apply to services too)?

	stateless communication (no session state)?
	
	favour services are not directly coupled (interservice comms ie async/event driven)
	
Detail for devs

	REST API structure including reporting errors
	
		are we always going to have CRUD/Document centric view
		approach for "operations"
	
	