<style>
body {
text-align: justify}
</style>


# Comparison of modeled and measured performance

we compare the performance estimated by ERM with the actual performance
measured with hardware performance counters [11] when the code is executed on an Intel Xeon
E5-2680 (ERM models a [Sandy Bridge microarchitecture](uarch-configurations.md). The
measured data reported in the plots is the mean value of 20 repetitions. For small size kernels
we reduce the error induced by measuring overhead by executing the kernel several times until
the total measurement time is larger than 10^8 cycles. The difference between the modeled and
measured performance is reported as a fold change, that is, 1 + | log2(estimated/measured)|.


## Scalar code

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/fft-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/wht-i-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/wht-r-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mvm-war-m-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mmm-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mmm-block-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/kmeans-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/stencil-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

</p>



## Vector code



## Differences with measured performance


* **Branch prediction**. Our model considers a perfect branch predictor and therefore we
do not model wrong path fetches due to mispredictions. 


* **Cache structure**. We model fully-associative caches because their miss rate can be estimated
for any cache size with a single-pass analysis of the memory access trace. This is
particularly advantageous because stack reuse distance algorithms are very expensive for
large memory traces. Contrary to our model, modern microarchitectures implement set-associative
caches. Not modeling the right cache structure will have effect in applications that
exhibit a large number of conflict misses


* **Microop decoding and macroop fusion**. In CISC architectures like Intel x86, complex
instructions are decomposed into simpler RISC-like instructions called microops. Common instruction sequences, in turn, sometimes are
optimized and fused into macroops. The details of how microop decoding and macroop
fusion are implemented are not disclosed by processors vendors, and we can only model
them based on benchmarking and empirical data [82].


* **Prefetcher**. Modern microprocessors implement various kinds of prefetchers. ERM models a
generic prefetcher that can be configured with the following parameters: when to trigger
the prefetch (e.g., upon a certain number of accesses to consecutive cache blocks), what to
prefetch (e.g., the next cache line or a strided access), and where to prefetch (e.g., L1 data
or L2 caches). However, since we cannot know precisely which prefetchers are activated
when running our experiments, we do not consider them as a parameter in the model.

* **Analysis prior to code generation**. The code analyzed by ERM is 
prior to code generation and the effect of target-specific optimizations is not considered by
our model. This effect could be alleviated by translating ISA-specific dynamic instruction
traces (e.g., x86) into LLVM IR traces and using these in the analysis4. However, using
actual ISA instruction traces digresses from the goal of our proposed model which is based
on DAG analysis and does not consider low-level architectural and microarchitectural details.


* **Instructions not analyzed**. Finally, ERM disregards all computations related to loop index variables, address
calculations, or stack pointer arithmetic. The numerical kernels considered by ERM so far,
however, are dominated by floating-point computations, and we expect the effects of these
instructions to be negligible for most of the kernels.



## References

[1] S. Williams, A. Waterman and D. Patterson. "Roofline: an insightful visual performance model for multicore architectures". Communications of the ACM, 2009.
