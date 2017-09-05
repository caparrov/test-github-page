<style>
body {
text-align: justify}
</style>


# DAG-Based Performance Model


## Background

Approaches to analyzing the performance of applications range from high-level analytical models
that provide coarse estimates of the performance of a small set of numerical kernels running
on a simple model of a processing platform, to sophisticated tools that provide accurate
performance estimations or measurements of actual execution on a given
platform.



Figure 1 illustrates the spectrum of performance analysis techniques, and the position of 
ERM's underlying performance model. As shown in the figure, the model builds on
prior high-level DAG-based analytical models (e.g., work-depth model [1]) and thus is a *DAG-based model approach*. The
model, however, considers a much more detailed abstraction of a processing core that 
includes features such as out-of-order (OoO) execution buffers, latency and bandwidths of a multi-level
memory hierarchy, or instruction fetch bandwidth. Thus, it achieves more accurate performance
estimates and yields deeper insights into the interaction of the DAG with the modeled hardware
resources, without the need to use a full-fledged simulator [2, 3] or access to a given platform [4].
Due to the increased complexity of the microarchitectural model, the performance is not estimated
by formulas as in classical methods, but by a tool (ERM).


<div style="width:image width px; font-size:90%; text-align:center;">
<a href="url"><img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/perf-model-overview-pink.png" align="center" width="110%" height="110%"></a>
Figure 1: Overview of performance analysis techniques.
</div>

<!---
As shown in the figure, it considers a more comprehensive set of platform parameters to model a processing core than the external memory
model [] with some of the resources of modern superscalar microarchitectures such the out-of-order execution buffers (green
boxes), execution units for different types of arithmetic operations (red boxes), or multi-level memory subsystem (blue boxes). and assume they are fully associative with least recently used (LRU) replacement policy.
More specifically, we
choose a set of X parameters to model the microarchitecture. 
 -->



## Microarchitecture model
Figure 2 illustrates the high-level microarchitecture model used in ERM. The processor core is modelled by a total of 40 parameters that define common features of modern superscalar microarchitectures. The choice of
parameters is a trade-off between abstraction and accuracy. For example, the model does capture
latency and throughput information of the execution units (pink boxes) and multi-level memory hierarchy (blue boxes), as
well as the out-of-order execution buffers (green boxes). It does not model however cache associativity, details
of branch prediction (assumed to be perfect) or address translation mechanisms.

Click [here](https://github.com/caparrov/test-github-page/blob/master/resources/uarch-configurations.md) for the complete list of parameters and how to configure them in ERM. 


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/uarch-model-pink.png"   width="65%" height="65%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>



<!---
<div style="width:image width px; font-size:90%; text-align:center;">
<a href="url"><img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/uarch-model.png" align="center" width="60%" height="60%"></a>
<p></p>
Figure 2: Overview of performance analysis techniques.
</div>

 -->


## DAG scheduling

ERM uses Tomasulo’s greedy algorithm to schedule the dynamically unfolding computation
onto the resources of the processor core's model (Figure 2): Instructions are scheduled (issued) in the first cycle in which both operands and resources
are available. ERM uses stack reuse distance analysis [5] to model fully associative caches with least recently used (LRU) replacement policy.


The result is a scheduled DAG as shown in Figure 3. The scheduled DAG contains different types of nodes and the nodes’ execution cycle is determined by both data dependences
and the input microarchitectural constraints. For example, the execution of the third L1 load (L1 ld) is delayed
due to memory bandwidth availability (only two nodes of that type can be executed per cycle
assuming L1 load bandwidth of 2 loads/cycle), and the length of multiplications (M) is five cycles due to the latency of
the corresponding functional unit. 


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/code.png"   width="50%" height="50%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/scheduled-DAG-pink.png"   width="42%" height="42%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 3: 2-D implicit hydrodynamics fragment (Livermore kernel 23) and associated scheduled DAG (small extract).
</p>
</p>



## Properties of the scheduled DAG


We define the following properties of the 
scheduled DAG, which are used by ERM to [model performance bounds](resources/performance-bounds.md).


###### Node types
> We distinguish the following nodes in the scheduled DAG: 
* Arithmetic computations: additions, multiplications, divisions, and their corresponding vector types.
* Vector computations: shuffle and blend.
* Memory loads and stores: L1, L2, L3, and mem.
* Stalls due to five out-of-order execution buffers: reservation station (RS), reorder buffer (ROB), load buffer (LB), store buffer (sb), and line-fill buffer (LFB). 


###### Issue time of a node type (T_issue)
> Number of cycles in which nodes of a certain type are being issued.


###### Latency time of a node type (T_lat)
> Number of cycles in which nodes of a certain type are executed
but not issued, that is, latency-only cycles.

 
###### Overlap cycles

> Overlap cycles can be defined for every set of node types, although for the generalized roofline plot ERM only considers overlap between pairs of node types (ERM can be configured to report overlap between all combinations of sets of nodes).


###### Stall cycles (T_stall)
> Number of execution cycles in which at least one of the
OoO execution buffers is full. The stall time T_stall can be further broken down into stalls due to the
respective OoO buffers.



###### Execution time or runtime (T)
> Number of cycles (length) of the scheduled DAG.


###### Performance
> The performance of the scheduled DAG is given as the ratio of work per unit of execution time (T).

The focus of ERM is on floating-point
numerical kernels. Thus, work is always measured in number of single-precision or
double-precision floating-point operations (flops), execution time is measured in cycles and
performance is always reported in flops/cycle.

## References

[1] G. E. Blelloch, “Programming parallel algorithms”, Communications of the ACM, vol. 39,
pp. 85–97, 1996.

[2] A. Patel, F. Afram, S. Chen, and K. Ghose, “MARSSx86: A full system simulator for x86
CPUs”, in Proceedings of the Design Automation Conference (DAC), pp. 1050–1055, 2011.

[3] N. Binkert, B. Beckmann, G. Black, S. K. Reinhardt, A. Saidi, A. Basu, J. Hestness, D. R.
Hower, T. Krishna, S. Sardashti, R. Sen, K. Sewell, M. Shoaib, N. Vaish, M. D. Hill, and
D. A. Wood, “The gem5 simulator”, SIGARCH Computer Architecture News, pp. 1–7, 2011.

[4] “Intel®VTune™Amplifier XE 2013”.

[5] C. Ding and Y. Zhong, “Predicting whole-program locality through reuse distance analysis”,
in Proceedings of the Programming Language Design and Implementation (PLDI), pp. 245–
257, 2003.
