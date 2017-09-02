# Performance bounds in the roofline plot


## Background: Roofline model


The roofline model [1] identifies and visualizes performance bottlenecks. The plot below shows an example of a roofline plot. It depicts the performance of three applications as a function of their operational intensity. The roofline model clearly identifies bottlenecks due to the computational throughput (appA) and the memory bandwidth (appC). However, the original model is inherently blind to bottlenecks due to non-throughput resources such as cache capacity, latency of memory accesses or the functional units, and out-of-order (OoO) execution buffers: e.g., for appC in the figure below the bottleneck is undetermined.


<div style="width:image width px; font-size:90%; text-align:center;">
<a href="url"><img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-source.png" align="left" width="99%" height="99%"></a>
Figure 1:
</div>

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

## References

[1] S. Williams, A. Waterman and D. Patterson. "Roofline: an insightful visual performance model for multicore architectures". Communications of the ACM, 2009.
