# Reference data

## Summary

Example of Ref Data plus best guess at usage 

Grades		UI,TimeCard, Scheduler, Reporting
OperationalRoles		Scheduler
AdjustmentCreditTypes		UI,TimeCard, Scheduler, Reporting
Location		Scheduler, PersonUI, Reporting
Location Types		UI
Adjustment Types		UI, Person, Accruals, Timecard?
Other Credit Types		UI, Person, Accruals, Timecard?
Shift Type		UI,TimeCard, Scheduler, Reporting
Absence Types		UI, Absence, Timecard?
Time Period Type	SRD, NWD, Absence, Training etc	UI,TimeCard, Scheduler, Reporting
Flex Change Types		UI,TimeCard, Scheduler, Reporting

### Issue
We want to be able to expose and manage reference data (static or slowly changing data) in Callisto. 

We'd like to have a consistent approach to dealing with reference data so that end users have a familiar approach when managing all reference data. 


### Decision
WIP

### Status
WIP

## Details

### Assumptions
All reference data will have a common set of management functions.

### Constraints
Reference data is segregated by tenant. Different tenant's cannot share reference data.

### Options
**Centralised reference data model**
Have a single container that exists to manage reference data

| pros | cons |
|------|------|
| well known location | single point of failure |
| consistent approach to management | may become complex if different management models are needed |
|      | introduces coupling into Callisto |

**Decentralised reference data model** 
Each container within Callisto manages its own reference data and publishes changes to that data just as it does with the other kinds of data that it owns

| pros | cons |
|------|------|
| no direct container coupling | copies of reference data may "drift" over time |
| mitigates to some degree the single point of failure issue | it may not be obvious which container should own a given type of reference data |
| allows each container to deal with reference data in its own way | inconsistency of approach to dealing with common reference data functionality |

### Selected Option

### Implications

## Related

### Related decisions

### Related requirements
**TODO** - We need a requirement that talks about the need to prevent tenants from accessing each other's data
