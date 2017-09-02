<style>
body {
text-align: justify}
</style>


# Modeling performance bounds


## Background: Roofline model


The roofline model [1] identifies and visualizes performance bottlenecks. The plot below shows an example of a roofline plot. It depicts the measured performance of three hypothetical applications appA, appB, and appC against their operational intensity. 
The roofline model
then adds two performance bounds to the roofline plot associated with both the computational
throughput and the memory bandwidth. The roofline model clearly identifies bottlenecks due to the computational throughput (appA) and the memory bandwidth (appC). However, the original model is inherently blind to bottlenecks due to non-throughput resources such as cache capacity, latency of memory accesses or the functional units, and out-of-order (OoO) execution buffers. For appB, for example, the bottleneck is undetermined.

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-source.png"  width="75%" height="75%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 1: Original roofline plot from [1].
</p>
</p>



## Modeling bottlenecks from the scheduled DAG or including additional tighter
We use our [modeling framework] and the detailed per-cycle analysis of the [scheduled DAG](performance-model.md) to turn execution cycles into hardware-related performance
bounds and include additional tighter
bounds on the roofline plot that provide deeper insights into why peak performance is not reached.


### Issue performance bounds
The issue bottleneck is the performance penalty that results from not fully utilizing the
throughput ignoring latency-, stall- and overlap-related effects. We model the issue
performance bound associated with a node type x as the maximum achievable performance if
execution consisted only of issue cycles

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dag-issue-bound.png"   width="50%" height="50%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-plot-issue-bound.png"   width="65%" height="65%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
</p>
</p>


## References

[1] S. Williams, A. Waterman and D. Patterson. "Roofline: an insightful visual performance model for multicore architectures". Communications of the ACM, 2009.
