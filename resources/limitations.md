<style>
body {
text-align: justify}
</style>


# Limitations 

### DAG-based performance model



### Generalized roofline plot

* **Application-specific roofs**. The new hardware-related bounds become
specific to program and input and thus such plot can contain only one performance
point. This arises from the fact that T_issue, T_lat, and T_s, from which the bounds are estimated,
cannot be obtained by formulas. Rather, these runtimes depend on the specific execution of the
application on the platform, have to be measured from the scheduled DAG, and are only
reported by tools that perform a detailed per-cycle analysis, like ERM.


* **Roofs only valid at the I of the application**. Even if the roofs are drawn for all values
of I, the performance bounds are only valid at the operational intensity of the application.
The main reason for keeping full roofs is to maintain the relation with the original roofline
plot and thus help understanding bottlenecks in the roofline plot when the original throughput/
bandwidths bounds are not reached. Further, we maintain the property of this model
of clearly identifying computations versus memory bounds, making thus explicit the notion
of compute- and memory-bound.
 

* **Interdependence of bottlenecks**. Another limitation of this analysis is that
all bottleneck lines are interdependent. Modifying only one microarchitectural parameter
implies a rescheduling of the entire computation DAG, and the rooflines may change in
unpredictable ways.

