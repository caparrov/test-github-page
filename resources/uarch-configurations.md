<style>
body {
text-align: justify}
</style>

# Microarchitectural configurations

The microarchitectural model (and its embodiment in ERM)  have a modular and flexible design: Every parameter of the model can be set to zero or infinity to
remove the associated hardware feature, and it is straightforward to extend it to include additional
execution units, or cache leves.
Section X  More specifically, we
choose a set of X parameters to model the microarchitecture.



| Command-line argument  |Description |
|-------------------|--------|
|  -address-generation-units=<uint>  |     Specify the number of address generation units. Default value is infinity|
|  -cache-line-size=<uint>           |     Specify the cache line size (B). Default value is 64 B|
|  -debug                             |    Generate debug information to allow debugging IR.|
|  -execution-units-latency=<number>  |    Specify the execution latency of the nodes(cycles). Default value is 1 cycle|
|  -execution-units-parallel-issue=<int> | Specify the number of nodes that can be executed in parallel based on ports execution. Default value is -1 |
|  -execution-units-throughput=<number> |  Specify the execution bandwidth of the nodes(ops executed/cycles). Default value is -1 cycle|
|  -force-interpreter                   |  Force interpretation: disable JIT|
|  -function=<string>                   |  Name of the function to be analyzed|
|  -help                                |  Display available options (-help-hidden for more)|
|  -instruction-fetch-bandwidth=<int>   |  Specify the size of the reorder buffer. Default value is infinity|
|  -l1-cache-size=<uint>                |  Specify the size of the L1 cache (in bytes). Default value is 32 KB|
|  -l2-cache-size=<uint>                |  Specify the size of the L2 cache (in bytes). Default value is 256 KB|
|  -line-fill-buffer-size=<uint>        |  Specify the size of the fill line buffer. Default value is infinity|
|  -llc-cache-size=<uint>               |  Specify the size of the L3 cache (in bytes). Default value is 20 MB|
|  -load-buffer-size=<uint>             |  Specify the size of the load buffer. Default value is infinity|
|  -mem-access-granularity=<uint>       |  Specify the memory access granularity for the different levels of the memory hierarchy (bytes). Default value is memory word size|
  -memory-word-size=<uint>             |  Specify the size in bytes of a data item. Default value is 8 (double precision)|
|  -reorder-buffer-size=<uint>           | Specify the size of the reorder buffer. Default value is infinity|
|  -reservation-station-size=<uint>      | Specify the size of a centralized reservation station. Default value is infinity|
|  -store-buffer-size=<uint>            |  Specify the size of the store buffer. Default value is infinity|



## Sandy Bridge microarchitecture



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/uarch-model-pink.png"   width="65%" height="65%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>



## ARM



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/arm-frontend-illustrated.png"   width="95%" height="95%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>


## POWER8
