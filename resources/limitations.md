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



## Adding tighter performance bounds
We use our [modeling framework] and the detailed per-cycle analysis of the [scheduled DAG](performance-model.md) to turn execution cycles into hardware-related performance
bounds and include additional tighter
bounds on the roofline plot that provide deeper insights into why peak performance is not reached.


### Issue performance bounds
The issue bottleneck is the performance penalty that results from not fully utilizing the
throughput ignoring latency-, stall- and overlap-related effects. We model the issue
performance bound associated with a node type x as the maximum achievable performance if
execution consisted only of issue cycles

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dag-issue-bound.png"   width="36%" height="36%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-plot-issue-bound.png" width="62%" height="62%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
</p>
</p>



### Latency performance bounds

The latency bottleneck quantifies the performance loss due to latency-only cycles, i.e., cycles in
which execution is stalled due to dependences with long-latency operations.
In the scheduled DAG in Fig. 4.3(a), for example, cycles 13, 15–18, 22–23 and 25–26 are idle cycles due to the
latency of the arithmetic computations. We model the performance bound associated with latency
effects considering the issue bound as the maximum achievable performance, and derive it as the
performance achievable if T = Tissue
x + Tlat
x :

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dag-latency-bound.png"   width="36%" height="36%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-plot-latency-bound.png" width="62%" height="62%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
</p>
</p>



### Stall Performance Bound
The stall bottleneck quantifies the fraction of execution time in which buffers are full. In our
proposed performance model, these cycles are quantified by the stall times

Stall nodes, as opposed to computation and memory nodes, do not correspond one-to-one
to any of the existing roofs in the roofline plot. To include them into the plot, we consider the
runtime Ts
x defined in (2.14) that combines both cycles of a given node type x and cycles of a
specific stall s. As with the latency bound, we define the stall bound taking the issue bound as a
reference;

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dag-stall-bound.png"   width="36%" height="36%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-plot-stall-bound.png" width="62%" height="62%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
</p>
</p>




### Overlap Performance Bound
The overlap bottleneck quantifies the loss of performance due to imperfect overlap between compute
and memory nodes. In contrast to the previous bottlenecks, it is defined for pairs of node
types including pairs of memory nodes


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dag-overlap-bound.png"   width="36%" height="36%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/roofline-plot-overlap-bound.png" width="62%" height="62%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
</p>
</p>



## Generalized Roofline plot

The result is a
generalization of the roofline plot that integrates all derived bottlenecks as bounds into a single
viewgraph.  


In the next section we discuss the main limitations of this representation, but first we emphasize
the following properties of the generalized roofline plot:

* **Progressively tighter bottlenecks**: Since the bounds are obtained by breaking down
execution time into different runtime components, the composition of those runtime components
eventually yield the total execution time and, thus, in most cases there is a bound
that is tight, i.e., it hits the performance of the application. If the bounds do not hit the performance
of the application, it is because we only consider overlap between pairs of nodes,
and the total execution time may be defined by the overlap between a larger set of nodes.

* **Relative position of bounds** The relative position of the hardware-related bounds with
respect to the performance of the application in the individual roofline plots shown in 4.9
is preserved when merging the plots into the generalized roofline plot in Fig. 4.13.
 

* **Utilization-based bottlenecks**: Our notion of utilization enables the handling of code
with different phases (e.g., parts dominated by memory operations, parts by computation).

* **Bounds ordering




## References

[1] S. Williams, A. Waterman and D. Patterson. "Roofline: an insightful visual performance model for multicore architectures". Communications of the ACM, 2009.
