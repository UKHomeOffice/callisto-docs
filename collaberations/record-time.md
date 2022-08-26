# Record time
Jira ref - 

![Callisto containers](./../images/timecard-container.png)

The recording of time involves the creation and/or modification of `TimeEntry` resources. A `TimeEntry` can be created via three routes which a differentiated by how they are initiated -

## Scheduler initiated 

 1. A Scheduler rosters a TimeCard `Person` onto a shift
 2. The Scheduled Events Producer publishes a `ScheduledEntry` event to it's topic 
 3. The TimeCard container receives the `ScheduledEntry` event via it's topic subscription 
 4. The TimeCard container uses the data in the `ScheduledEntry` event to create a `TimeEntry` entity in it's internal Database

## User initiated 

**TODO - person identity resolution**

 1. A user wants to record or amend a `TimeEntry` that they own
 2. The Single-Page Application makes a RESTful request to the TimeCard container asking for the set of `TimePeriodType` resources that are appropriate for the owner to see
 3. The user  selects the `TimePeriodType`
 4. The Single-Page Application uses metadata in the `TimePeriodType` to render data entry fields that are appropriate for the selected `TimePeriodType`
 5. The user enters the appropriate data values and submits them
 6. The Single-Page Application makes a RESTful request to the TimeCard container asking it to create the `TimeEntry` resource
 7. The TimeCard container returns success or failure to the user

## Manager initiated 

 1. A manager of a given owner wants to record or amend a `TimeEntry` belonging to that owner
 2. The Single-Page Application makes a RESTful request to the TimeCard container asking for the set of `TimePeriodType` resources that are appropriate for the owner to see
 3. The user or manager selects the `TimePeriodType`
 4. The Single-Page Application uses metadata in the `TimePeriodType` to render data entry fields that are appropriate for the selected `TimePeriodType`
 5. The user or manager enters the appropriate data values and submits them
 6. The Single-Page Application makes a RESTful request to the TimeCard container asking it to create the `TimeEntry` resource
 7. The TimeCard container returns success or failure to the user or manager
