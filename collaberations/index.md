# Collaborations


In a loosely coupled system it is common and likely that individual containers will need to work together to achieved a system activity or service an end user request.

A number of the key routes through the system have been illustrated in the following diagrams to how the various services are choreographed to perform some  of the activities required. 

These use cases focus on:

- The triggering event from the user or a system event.
- The communication of data to subsidiary services required to complete the activity.
- The propagation of event information to other parts of the system. 


The use cases detailed subject to elaboration with Business Analysts are:

- Submit Time Entry
- Communicate Schedule
- Onboarding
- Ingest Agreement (from TAMS)
- Consume Agreement
- Onboarding a user
	- METIS data
	- TAMS data
	- Manager configuration
- Absences
	- Callisto flow
	- METIS communication (to & from)

## Submit Time Entry

The TimeEntry can be stored independently of Accruals being calculated. This decoupling permits Callisto to service the priority business need even if Accruals was unavailable.

Decoupling is achieved by using Kafka queues to perform asynchronous communications. Also of note is that the Accruals API is the end point responsible for both the business logic and for persistently storing data to the database.

Monitoring of  the Timecard event queue will permit Accruals to scale up or down as required to match the load.

![Submit Time Entry](../images/submitTimeEntry.png)

## Communicate Schedule

The drawbacks of the decoupling are clear here although it is thought to be minor in comparison to the benefits. Whilst the schedule is persistently stored by the Scheduler API it will not be reflected in the TimeCard or Accruals until the Scheduler Event Queue and the TimeCard Event queue are cleared. In normal operation this is not expected to be an issue but under failure conditions there could be a lag and furthermore, for some time it would be possible for Schedule, TimeCard or Accruals to hold a different set of data at any one time. 

The use of a unique identifier for the triggering action carried as part of the payload would help permit any part of the system that aggregated either Schedule, TimeCard and/or Accruals to perform a consistency check and is recommended.

##Ingest Agreement (from TAMS)

For Callisto to operate during Private Beta and beyond will require data from both the existing Time & Attendance system TAMS as well as from the Metis HR System.  

This is an interim MVP solution, the intention is that in the future agreements will be built within the ‘Agreements Service’ which is logically part of Metis, the Home Office HR System. For this reason, it is critical that the agreements service is entirely separately from core Callisto. 

The agreement data must be accessible to Callisto to allow it to track accrual balances for an individual against their agreement as this is their target. 

This information must be imported (or entered) into Callisto when: 

An individual is onboarded into Callisto as part of the private Beta onboarding process. 

- An individual is onboarded into Callisto as part of the private Beta onboarding process
- A new agreement for the next agreement year is created. 
- An agreement is modified. 


![Ingest Agreement](../images/ingestAgreement.png)

## Consume Agreement

## Onboarding a user

## METIS data

## TAMS data

## Manager configuration

## Absences

## Callisto flow

## METIS communication (to & from)
