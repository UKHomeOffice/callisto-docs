# Communicate Schedule


The drawbacks of the decoupling are clear here although it is thought to be minor in comparison to the benefits. Whilst the schedule is persistently stored by the Scheduler API it will not be reflected in the TimeCard or Accruals until the Scheduler Event Queue and the TimeCard Event queue are cleared. In normal operation this is not expected to be an issue but under failure conditions there could be a lag and furthermore, for some time it would be possible for Schedule, TimeCard or Accruals to hold a different set of data at any one time. 

The use of a unique identifier for the triggering action carried as part of the payload would help permit any part of the system that aggregated either Schedule, TimeCard and/or Accruals to perform a consistency check and is recommended.