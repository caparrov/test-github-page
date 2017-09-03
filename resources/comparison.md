<style>
body {
text-align: justify}
</style>


# Comparison of modeled and measured performance

The figures below show the comparison of the performance estimated by ERM with the actual measured performance when the code is executed on an Intel Xeon
E5-2680 (ERM models a [Sandy Bridge microarchitecture](uarch-configurations.md)) for some examples of both scalar and vector numerical kernels. The
measured data is obtained with hardware performance counters [1], and the data reported in the plots is the mean value of 20 repetitions. For small size kernels
we reduce the error induced by measuring overhead by executing the kernel several times. All these kernels
operate on double-precision data, and all the results shown correspond to a warm cache execution.

## Scalar numerical kernels

For the FFT, MVM and MMM triple loop, ERM accurately estimates performance and performance trends. In the case of FFT, for example,
estimated performance is on average (the mean across all sizes) 1.22x the measured performance
of the icc-compiled code, and 1.31x the performance of the clang-compiled code. For the other
computations, however, the difference is more significant. In the case of WHT, for example,
average estimated performance is 2.58x and 4x the measured performance for the iterative and
recursive implementations, respectively.
[Diferences](#differences-with-measured-performance)


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/fft-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/wht-i-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/wht-r-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mvm-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mmm-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/mmm-block-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/kmeans-warm-illustrator.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/stencil-warm-thesis.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
</p>

#### Comparison with microarchitectural simulator MARSS

Across the small-size kernels, performance estimated is on average (mean value of the fold
change across all kernels) 1.64x the original performance, and for the large benchmarks, 1.95x.
For large sizes, differences in performance tend to be larger due to the complexity of accurately
modeling resources like cache structure, memory bandwidth (is affected by many factors in practice
like page table loads or write-allocate traffic), and prefetcher.


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/livermore-loops-kernels-small-perf-comparison.png"  width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/livermore-loops-kernels-large-perf-comparison.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
</p>


## Vector code (with AVX vector intrinsics)


In this
case, the plots show the normalized performance (ERM performance vs. measured performance)
in log scale to properly visualize the fold changes in performance. On average, the modeled
performance is 1.37x in comparison with the code compiled with icc, and 1.69x with respect to
the performance of the code compiled with clang.


For vector code, the figures shows the normalized performance.  The difference between the modeled and
measured performance is reported as a fold change.




<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dsyrk-15-171-13-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dsyrk-16-160-16-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dltrsv-15-171-13-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>


<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dltrsv-16-160-16-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dlusmm-16-100-7-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dlusmm-16-96-8-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dsylmm-16-100-7-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/dsylmm-16-96-8-normalized.pdf.png"   width="45%" height="45%" alt="Sublime's custom image" style="border:0px;margin:10px"/>
</p>

## Differences with measured performance


* **Branch prediction**. Our model considers a perfect branch predictor and therefore we
do not model wrong path fetches due to mispredictions. 


* **Cache structure**. We model fully-associative caches because their miss rate can be estimated
for any cache size with a single-pass analysis of the memory access trace. Not modeling the right cache structure will have effect in applications that
exhibit a large number of conflict misses


* **Microop decoding and macroop fusion**. The details of how microop decoding and macroop
fusion are implemented are not disclosed by processors vendors, and we can only model
them based on benchmarking and empirical data [2].


* **Prefetcher**. Modern microprocessors implement various kinds of prefetchers. ERM
models a
generic prefetcher that can be configured with the following parameters: when to trigger
the prefetch (e.g., upon a certain number of accesses to consecutive cache blocks), what to
prefetch (e.g., the next cache line or a strided access), and where to prefetch (e.g., L1 data
or L2 caches). However, since we cannot know precisely which prefetchers are activated
when running our experiments, we do not consider them as a parameter in the model.

* **Analysis prior to code generation**. ERM analyzed the instruction trace
prior to code generation and the effect of target-specific optimizations is not considered by
our model. 

* **Instructions not analyzed**. ERM disregards all computations related to loop index variables, address
calculations, or stack pointer arithmetic. The numerical kernels considered by ERM so far,
however, are dominated by floating-point computations, and we expect the effects of these
instructions to be negligible for most of the kernels.



## References

[1] Intel Performance Counter Monitor. <http://software.intel.com/en-us/articles/intel-performance-counter-monitor/>.

[2] Livermore

[3] LGen

[] Agnes tables
