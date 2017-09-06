<style>
body {
text-align: justify}
</style>

# Microarchitectural configurations

The [high-level microarchitectural model](#performance-model.md) used by ERM has a modular and flexible design: every parameter of the model can be set to zero or infinity to
remove the associated hardware feature, and it is straightforward to extend it to include additional
execution units, or cache levels. 


The following table lists the parameters that define a microarchitecure:



| Command-line argument  |Description (default value)|
|-------------------|--------|
|  -execution-units-parallel-issue={\<int\>} | List of number of nodes that can be executed in parallel (inf) |
|  -execution-units-throughput={\<float\>} |  List of execution bandwidth of the nodes in ops/cycles (inf)|
|  -execution-units-latency={\<number\>}  |   List of Execution latency of the nodes in cycles (1) |
|  -memory-word-size=\<uint\>             |  Size in bytes of a data item (8)|
|  -cache-line-size=\<uint\>           |     Cache line size in bytes (64)|
|  -register-file-size=\<uint\>             | Size of the register file (0)
|  -l1-cache-size=\<uint\>                |  Size of the L1 cache in bytes (32 KB) |
|  -l2-cache-size=\<uint\>                |  Size of the L2 cache in bytes (256 KB) |
|  -llc-cache-size=\<uint\>               |  Size of the L3 cache in bytes (20 MB)|
|  -mem-access-granularity=\<uint\>       |  Memory access granularity for the different levels of the memory hierarchy, in bytes ()|
|  -reorder-buffer-size=\<uint\>           | Size of the reorder buffer (inf)|
|  -reservation-station-size=\<uint\>      | Size of a centralized reservation station (inf)|
|  -load-buffer-size=\<uint\>             |  Size of the load buffer (inf)|
|  -store-buffer-size=\<uint\>            |  Size of the store buffer (inf)|
|  -line-fill-buffer-size=\<uint\>        |  Size of the line-fill buffer (inf)|
|  -address-generation-units=\<uint\>  |     Number of address generation units (inf)|
|  -instruction-fetch-bandwidth=\<int\>   |  Number of instructions fetched per cycle (inf) |

### Comments:

*  Values of throughput and latency for the different types of nodes are specified in the following order: {SP (single-precision) fp addition, DP (double-precision) fp addition, SP fp multiplication, DP fp multiplication, SP fp fused multiply-add, DP fp fused multiply-add, SP fp division, DP fp division, SP fp shuffle, DP fp shuffle,  SP fp blend, DP fp blend, SP fp bool, DP fp bool, register, L1 load, L1 store, L2, L3, memory}.


* **Port binding**:


In addition to the parameters listed in Table 1, ERM requires the following paramters to be specified:

* **-constraint-agus=\<bool\>**.
* **-constraint-ports=\<bool\>**.
* **-constraint-ports-x86=\<bool\>**.



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/sb-port-binding.png"   width="95%" height="95%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>


* **-constraint-ports-ARM=\<bool\>**.
* **-x86-memory-model=\<bool\>**. Implement x86 memory model: writes are not reordered with other writes and writes are not reordered with earlier reads.
* **-arm-memory-model=\<bool\>**. Implement ARM memory model (constraints on loads and store ordeing).
* **-warm-cache=\<bool\>**. If true, analyze code with warm cache scenario. Requires the function to be executed twice (true).
* **-float-precision=\<bool\>**. false = single-precision floating point, true = double-precision.
* **-vector-code=\<bool\>**. Code contains vector code (false).
* **-max-vector-width=\uint\>**. Vector width. For example, if the code with AVX intrinsics, this value is 4 for double-precision, and 8 for single-precision. 
* **-march=\<string\>**. Use the default values for a given microarchitecture. Possible values: SB (Sandy Bridge) and ARM-CORTEX-A9. 
* **-config=\<string\>**. JSON configuration file ID. The value of this parameter is the value that appends to configX.json. 
* **-function=\<string\>**.   Name of the function to be analyzed (main).



The parameters that define the model of the microarchitecture can be specified via command line to the interpreter (type 'lli --help' to see all command line options) or via a JSON file. 

# Examples

### Sandy Bridge microarchitecture



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/Sandy-Bridge-frontend.png"   width="95%" height="95%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>

configSB.json, and specify `-uarch=SB` in the command line.

```
{"constraint-agus":"1", "constraint-ports-x86": "1", "execution-units-latency": "{3,3,5,5,0,0,22,45,1,1,1,1,1,1,0,4,4,12,30,100}", "reorder-buffer-size": "168", "load-buffer-size": "64", "store-buffer-size": "36", "cache-line-size": "64", "instruction-fetch-bandwidth": "4", "reservation-station-size": "54", "report-only-performance": "0", "x86-memory-model": "1", "register-file-size":"16", "l1-cache-size": "32768", "execution-units-parallel-issue": "{1,1,1,1,0,0,1,1,1,1,2,2,1,1,-1,2,1,1,1,1}", "execution-units-throughput": "{1,1,1,1,0,0,0.04545,0.0227,1,1,1,1,1,1,-1,16,8,32,32,8}", "line-fill-buffer-size": "10",  "memory-word-size": "8", "llc-cache-size": "20971520", "warm-cache": "1", "l2-cache-size": "262144", "spatial-prefetcher": "0", "mem-access-granularity": "{1,8,8,64,64,64}", "constraint-ports": "1", "vector-code": "0","max-vector-width":"4", "address-generation-units":"2"}
```




Equivalently, the parameters can be specified directly in the execution command:
```
Execution command

```




### ARM



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/arm-frontend-illustrated.png"   width="95%" height="95%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>


### POWER8



<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/power8-frontend-illustrated.png"   width="95%" height="95%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: High-level model of the processor core used by ERM.
</p>
</p>
