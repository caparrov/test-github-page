<style>
body {
text-align: justify}
</style>


# DAG-based Performance Model


## Background

Approaches to analyzing the performance of applications range from high-level analytical models
that provide coarse estimates of the performance of a small set of numerical kernels running
on a simple model of a processing platform, to sophisticated tools that provide accurate
performance estimations or measurements of actual execution on a given
platform.



Figure 1 illustrates this spectrum of performance analysis techniques, and the position of the proposed DAG-based model to analyzing performance. As shown in the figure, our work builds on
prior high-level DAG-based analytical models and thus is a *DAG-based model approach*. Our
model, however, considers a much more comprehensive set of platform parameters to model the
processor, such as out-of-order (OoO) execution buffers, latency and bandwidths of a multi-level
memory hierarchy, or instruction fetch bandwidth. Thus, it achieves more accurate performance
estimates and yields deeper insights into the interaction of the DAG with the modeled hardware
resources, without the need to use a full-fledged simulator or access to a given platform.
Due to the increased complexity of our microarchitectural model, the performance is not estimated
by formulas as in classical methods, but by a tool (ERM) that both dynamically generates and
schedules the DAG on the modeled microarchitecture. 



ERM builds on a DAG-based performance model that considers the execution of the computation
DAG (nodes are floating-point operations and memory accesses, and edges denote true data dependencies) on the abstraction of a single-core processor that captures the main features of modern microarchitectures.  Our goal is to achieve higher accuracy and deeper insights into the interaction
of the application with the modeled hardware resources than prior classical models which only consider ...

ERM is based on a novel DAG-based performance model that extends
classical analytical high-level DAG-based models to consider a more detailed abstraction
of a processor core

<!---

ERM, similarly to microarchitectural
simulators, models the execution of an application on a cycle-by-cycle basis; in contrast to
microarchitectural simulators, it only models inherent properties of the application, and highlevel
features of a microarchitecture. For example, it considers floating-point computations and
memory accesses, but does not model machine-specific address calculation operations, or calling
conventions. As another example, it considers a multi-level cache hierarchy, but does not model
the details of cache associativity or the virtual memory system.
-->




<div style="width:image width px; font-size:90%; text-align:center;">
<a href="url"><img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/perf-model-overview.png" align="left" width="99%" height="99%"></a>
Figure 1: Overview of performance analysis techniques.
</div>




## Microarchitecture model
Figure 2 shows the microarchitecture model used in our proposed analysis. 
It extends the external memory
model [], which only considers, with some of the resources of modern superscalar microarchitectures such the out-of-order execution buffers (green
boxes), execution units for different types of arithmetic operations (red boxes), or multi-level memory subsystem (blue boxes). and assume they are fully associative with least recently used (LRU) replacement policy.
More specifically, we
choose a set of X parameters to model the microarchitecture. 



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/uarch-model.png"   width="60%" height="60%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: Overview of performance analysis techniques.
</p>
</p>


The choice of
parameters is a trade-off between abstraction and accuracy. For example, our model does capture
latency and throughput information of the execution units and multi-level memory hierarchy, as
well as the out-of-order execution buffers. It does not model however cache associativity, details
of branch prediction (assumed to be perfect) or address translation mechanisms.
**Overall, the microarchitectural model and the tool that embodies it (presented in Section 2.4)
have a modular and flexible design: Every parameter of the model can be set to zero or infinity to
remove the associated hardware feature, and it is straightforward to extend it to include additional
execution units, levels of cache, or to define a different mapping of nodes in the computation DAG
to execution units.**. **Section X describes how to model two modern microarchitecutres.**


<!---
<div style="width:image width px; font-size:90%; text-align:center;">
<a href="url"><img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/uarch-model.png" align="center" width="60%" height="60%"></a>
<p></p>
Figure 2: Overview of performance analysis techniques.
</div>

 -->


## DAG scheduling

We use Tomasulo’s greedy algorithm to schedule the dynamically unfolding computation
onto the resources listed in Table 2.2. We adapt the algorithm to incorporate the additional OoO
execution buffers defined above and to obey the additional constraints on the memory reordering
imposed by the platform’s memory model.

ERM uses stack reuse distance analysis [70] to model associative caches.



## Properties of the scheduled DAG
<!---
![alt text](https://raw.githubusercontent.com/caparrov/ERM-4.0.1/master/resources/images/livermore-kernel-23.png width="48")
![alt text](https://raw.githubusercontent.com/caparrov/ERM-4.0.1/master/resources/images/scheduled-DAG.png "")
 -->




The result of the previous analysis is a scheduled DAG as shown in Fig. 2.10(b) for the numerical
kernel in Fig. 2.10(a). The DAG contains five different types of nodes and, in contrast to the
DAGs shown in Fig. 2.1, the nodes’ execution cycle is determined by both data dependences
and the input microarchitectural constraints. For example, the execution of node (4) is delayed
due to memory bandwidth availability (only two nodes of that type can be executed per cycle
assuming L1-ld = 2 loads/cycle), and the length of node (3) is five cycles due to the latency of
the corresponding functional unit (mul = 5 cycles). In the following, we define the properties of

### Issue time of a node type x
x ) is the number of cycles in which nodes of type x
are being issued. It can be estimated as:


### Latency time of a node type x



### Latency time of a node type x


### Performance.
The performance of the scheduled DAG, P, is given as the ratio of arithmetic
computations per unit of execution time:

