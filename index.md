# Callisto

The repository is a place for design documentation for Callisto. If you want to find out how Callisto hangs together you've come to the right place.

We've deliberately kept things light. Reading documentation should not be an arduous task. We've structured the docs in an effort to allow you to find your answer quickly and without fuss. To support us in this goal we use the [C4](https://c4model.com/) way of describing our architecture. 

## What is Callisto?

Callisto is a rostering and time recording system. It allows people to plan out shifts and it allows people to record the time they have worked. The project is being sponsoted by Border Force who have some of the most complex requirements in the Home Office however Callisto is indended to be useable by other organisations within Home Office and so must remain flexible to meet their needs.

### Business concepts

#### Users
* planner - TODO
* worker - TODO
* manager - TODO
* scheduler - TODO

#### Terminology
* Anualised Hours Working (AHW) - TODO
* Accrual - TODO
* Anualised Hours Agreement (Agreement)
* Roster - TODO
* Shift - TODO
* Demand - demand transaltes to number of people who are needed on a shift and the composition of skills that they need to have in order to effectively carry out the duties required of that shift. Demand can be things like expected number of passengers coming through a givne port between a given time period
* Location - TODO

### User goals
Boiling it down, Callisto supports six key areas of functionality 

* Planning - a planner wants to design shifts that are going to meet the demand for the location that the shift will cover. 
* Schedulnig - a scheduler wants to assign people to shifts (roster). The shift will be designed such that it meets demand. A scheduer makes sure that people are assigned to a shift(s) to satisy the demand that the shift has been designed to meet
* Recodring time - a worker wants to record time that they have worked against a shift that they have worked. Sometimes a worker will want to record overtime too.
* Recodring absence - a workder wants to record some planned leave. A worker or a manager wants to record some unplanned (sick) leave. You can imagine that this information is useful to people performing planning and scheduling
* ad-hoc changes to shift - sometimes last minute changes need to be made to a shift. These changes fall outside of the routine planning and scheduling acitvities and are useualy done by a line manager in response to a rapdily changing situation
* view upcoming shifts - a worker needs to be able to view what shofts they have been rostered on to in order align their working hours with their commitments outside of work

For Border Force Callisto has a seventh key area of functionality that is triggered indriectly by a worker recording time -

* Tracking accruals - as a worker records time they have worked their accrual targets are passively updated and thier balances adjusted accordingly. You can imagine that up to data balance information is useful for planners and also for the worker in terms of making sure they the worker is on track to meet their accrual targets for the agreement period


## Repository stucture

This repository holds five types of information. They are presented below in their suggested reading order - 

### Characteristics
There are certain [characteristics](characteristics.md) that we want Callisto to have. Our design is borne of a a need to embody these characteristics as well as the functional requirements found in Jira

### Landscape
[landscape.md](landscape.md) shows the Users and external systems that interact with Callisto

### Containers
We follow the single responsibility principle as far as is pragmatic so you'll see that the Callisto system is make up of a set of collaborating Containers. 

The file [contaners.md](containers.md) lists them out with a brief introduction to their responsibility. If you want more information on a container then you'll find much more info in the dedicated container definition which you can find linked in containers.md

### Decisions
The [decisions](decisions/) folder holds all of the decisions we've made or are trying to make. Here a decision is a software design choice that addresses a significant requirement. Decisions tie back to Jira so that we have requirements traceability and if a ticket changes in Jira we can identify any decisions that might be impacted.

### Collaborations
In the [collaborations](collaborations/) folder you'll find documents that describe Callisto workflows and how Containers work together to support the given workflow. Workflows tie back to Jira so that we have requirements traceability and if a ticket changes in Jira we can identify any workflows that might be impacted

## Local vs Global
This repository is indented to hold documentation that is related to Callisto as a whole. Each Container within Callisto is free to make local design decisions. However any significant decisions should also be documented at that local level in a similar fashion to what is done here.

## Contributing
Designs and decisions are only as good as the information known at the time of their making. It is almost a certainty that as work progresses and we learn more that designs or decisions will need to be revisited. This is encouraged and you are encouraged to keep the designs and the code in sync.
*TODO* - Stephen what would you recommend?