
# Submit Time Entry

The TimeEntry can be stored independently of Accruals being calculated. This decoupling permits Callisto to service the priority business need even if Accruals was unavailable.

Decoupling is achieved by using Kafka queues to perform asynchronous communications. Also of note is that the Accruals API is the end point responsible for both the business logic and for persistently storing data to the database.

Monitoring of  the Timecard event queue will permit Accruals to scale up or down as required to match the load.


## Event consumers
When a `TimeEntry` is successfully recorded by the TimeCard container it triggers the publication of an event that encapsulates that `TimeEntry` resource along with the type of action that was performed on the resource (TODO - link to event page).

There are a number of Callisto containers that consume these events. The sections below list the containers that are consuming the events and describe what action is taken by the container when the event is received. 

### Accruals
![Submit Time Entry](../images/submitTimeEntry.png)
The Accruals container consumes all `TimeEntry` events and uses them to keep Accrual balances up to date. More detail on this process can be found in the [Accruals TimeEntry consumer design](https://github.com/UKHomeOffice/callisto-accruals-restapi/blob/main/docs/features/annual-target-hours/timecard-timeentry.md)

