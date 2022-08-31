# Callisto

The repository is a place for design documentation for Callisto. If you want to find out how Callisto hangs together you've come to the right place.

We've deliberately kept things light. Reading documentation should not be an arduous task. We've structured the docs in an effort to allow you to find your answer quickly and without fuss. To support us in this goal we use the [C4](https://c4model.com/) way of describing our architecture. 

We've also adopted the "diagrams as code" approach which allows us to create multiple diagrams from a single model. We've chosen the [Structurizr](https://structurizr.org/) tool to help us turn our models into diagrams. Structurizr is a web-based tool which has it's own domain specific language (DSL) that can be used to build a model. From there Structurizr can use the model to render a range of different diagrams. 

The Structurizr DSL is itself built with C4 concepts at its core and so is a natural fit with how we're describing the architecture of Callisto. You'll find a [model.c4](model.c4) file in the root of this repository. The model.c4 was used to generate the images found in the [images](images) directory that give an overview of the containers within Callisto. In addition container specific repositories will have their own model.c4 files which drill in to more detail about the specific container.


## What is Callisto?

Callisto is a rostering and time recording system. It allows people to plan out shifts and it allows people to record the time they have worked. The project is being sponsored by Border Force who have some of the most complex requirements in the Home Office however Callisto is indented to be useable by other organisations within Home Office and so must remain flexible to meet their needs.

### Business concepts

#### Users
* planner - TODO
* worker - TODO
* manager - TODO
* scheduler - TODO

#### Terminology
* Annualized Hours Working (AHW) - TODO
* Accrual - TODO
* Annualized Hours Agreement (Agreement)
* Roster - TODO
* Shift - TODO
* Demand - demand translates to number of people who are needed on a shift and the composition of skills that they need to have in order to effectively carry out the duties required of that shift. Demand can be things like expected number of passengers coming through a given port between a given time period
* Location - TODO

### User goals
Boiling it down, Callisto supports six key areas of functionality 

* Planning - a planner wants to design shifts that are going to meet the demand for the location that the shift will cover. 
* Scheduling - a scheduler wants to assign people to shifts (roster). The shift will be designed such that it meets demand. A scheduler makes sure that people are assigned to a shift(s) to satisfy the demand that the shift has been designed to meet
* [Recording time](./collaberations/record-time.md) - a worker wants to record time that they have worked against a shift that they have worked. Sometimes a worker will want to record overtime too.
* Recording absence - a worker wants to record some planned leave. A worker or a manager wants to record some unplanned (sick) leave. You can imagine that this information is useful to people performing planning and scheduling
* ad-hoc changes to shift - sometimes last minute changes need to be made to a shift. These changes fall outside of the routine planning and scheduling activities and are usually done by a line manager in response to a rapidly changing situation
* view upcoming shifts - a worker needs to be able to view what shifts they have been rostered on to in order align their working hours with their commitments outside of work

For Border Force Callisto has a seventh key area of functionality that is triggered indirectly by a worker recording time -

* Tracking accruals - as a worker records time they have worked their accrual targets are passively updated and their balances adjusted accordingly. You can imagine that up to data balance information is useful for planners and also for the worker in terms of making sure they the worker is on track to meet their accrual targets for the agreement period


## Repository structure

This repository holds five types of information. They are presented below in their suggested reading order - 

### Characteristics
There are certain [characteristics](characteristics.md) that we want Callisto to have. Our design is borne of a a need to embody these characteristics as well as the functional requirements found in Jira

### Context
[context.md](context.md) shows the Users and external systems that interact with Callisto

### Containers
We follow the single responsibility principle as far as is pragmatic so you'll see that the Callisto system is make up of a set of collaborating Containers. 

The file [containers.md](containers.md) lists them out with a brief introduction to their responsibility. If you want more information on a container then you'll find much more info in the dedicated container definition which you can find linked in containers.md

### Decisions
The [decisions](decisions/) folder holds all of the decisions we've made or are trying to make. Here a decision is a software design choice that addresses a significant requirement. Decisions tie back to Jira so that we have requirements traceability and if a ticket changes in Jira we can identify any decisions that might be impacted.

### Choreography
[choreography](/choreography/index.md) describe Callisto workflows and how Containers work together to support the given workflow. Workflows tie back to Jira so that we have requirements traceability and if a ticket changes in Jira we can identify any workflows that might be impacted

## Local vs Global
This repository is indented to hold documentation that is related to Callisto as a whole. Each Container within Callisto is free to make local design decisions. However any significant decisions should also be documented at that local level in a similar fashion to what is done here.

## Contributing
Designs and decisions are only as good as the information known at the time of their making. It is almost a certainty that as work progresses and we learn more that designs or decisions will need to be revisited. This is encouraged and you are encouraged to keep the designs and the code in sync.
*TODO* - Stephen what would you recommend?
