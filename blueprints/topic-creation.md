
# Topic creation

This guidance is intended to aid in the decision around when a new topic should be created and whether or not explicit partitioning is required.

**As a rule of thumb each Container has a single topic that all events are published on**

Sometimes the above rule will not be applicable. Below are some points to consider before you choose to follow the default.

### When to create an additional container topic
The new event type(s) to be published would add significantly more throughout to the existing topic thus placing more burden on consumers to filter/handle those additional event type instances. It is difficult to be precise about a definition of "significant". On this basis where this scenario does arise it will need to be considered on its own merits.

The event type encapsulates data that we do not want all consumers of a topic to see. This may bring it's own challenges and will need to be considered on a case by case basis

### When to partition a topic
Partitioning is Kafka's way of achieving higher throughout. If a single consumer is creating a bottleneck then it might be worth considering partitioning a topic and adding a consumer group that holds multiple consumers. 

Does the data need to be consumed in some sort of order. If not then consider using the uncorrelated/random partitioning strategy (**TODO** link).

If it does need to be ordered then the partition design needs to be carefully considered. In particular bear the following in mind - 

Depending upon how messages are spread across partitions there may be an unequal distribution of messages such that some partitions have many more messages than others. These so called "hot" partitions have a number of disadvantages -

1. consumers of the "hot" (higher throughput) partitions will have to process more messages than other consumers in the consumer group, potentially leading to processing and networking bottlenecks
2. depending on the [RPO](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective) then topic retention must be sized for the partition with the highest data rate, which can result in increased disk usage across other partitions in the topic

#### Partition key design
As stated above it is possible to end up with "hot" partitions. If message ordering really is needed and throughput is sub-optimal such that partitioning is needed then you need to define a partition key that will result in the most equal distribution of messages across the partitions. That needs to be done on a case by case basis.

#### Consumer group considerations
Whenever a consumer enters or leaves a consumer group, the brokers rebalance the partitions across consumers.

By default, when a rebalance happens, all consumers drop their partitions and are reassigned new ones (which is called the “eager” protocol). If you have an application that has a state associated with the consumed data you need to drop that state and start fresh with data from the new partition. If it's important to keep a partition with a specific consumer instance then the `StickyAssignor` or `CooperativeStickyAssignor` (**TODO** link) may be able to help maintain that relationship.

Instead of using a consumer group, you can directly assign partitions through the consumer client, which does not trigger rebalances. Of course, in that case, you must balance the partitions yourself and also make sure that all partitions are consumed.

### Topic creation checklist
When you are creating a new topic you need to make sure that the following tasks are done

 - Name the topic following the naming convention of topic name = callisto-[publishing container-name]
 - Document the topic [here]() **TODO** 
 - Deploy the topic **TODO** deployment checklist (partition number, securing it to prevent unauthorised access)

## Resources
- https://inoio.de/blog/2021/05/21/kafka-topic-design-guidelines/
- https://newrelic.com/blog/best-practices/kafka-best-practices
- https://newrelic.com/blog/best-practices/effective-strategies-kafka-topic-partitioning
