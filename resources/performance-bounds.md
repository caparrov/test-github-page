# Performance bounds in the roofline plot


## Background: Roofline model

## Modeling bottlenecks from the scheduled DAG

We leverage the detailed per-cycle analysis of the scheduled DAG
presented in Chapter 2 to turn these additional execution cycles into hardware-related performance
bounds and include them as additional roofs into the roofline plot. Our approach to modeling
performance bounds and integrating them into the roofline plot is based on the following three
main ideas.

### Issue bounds
The issue bottleneck is the performance penalty that results from not fully utilizing the
throughput ignoring latency-, stall- and overlap-related effects. We model the issue
performance bound associated with a node type x as the maximum achievable performance if
execution consisted only of issue cycles
