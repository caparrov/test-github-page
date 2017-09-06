<style>
body {
text-align: justify}
</style>


# Examples

We show examples of generalized roofline plots for analyzing bottlenecks of numerical kernels running on a
modern Intel Xeon (thus, ERM is configured to [model a Sandy Bridge microarchitecture](uarch-configurations.md)). We emphasize that we don’t need access to the actual processor to
perform these analyses. In the generalized roofline plots shown, the original performance bounds
are shown as solid lines, the new issue, latency and stall bottlenecks as dashed lines, and overlap
bottlenecks as gray solid lines. 


## MVM: Cold and warm cache

Figure 1 analyzes a double-precision floating-point matrix-vector multiplication (MVM) run
with both cold and warm cache. In the former, memory latency (and bandwidth, which is included
in all memory bottleneck lines) is the bottleneck as expected (the matrix has to be loaded and
is not reused). Further, long-latency operations like the memory accesses introduce non-compute cycles due to RS stalls. In the warm cache scenario, the RS bottleneck is
considerable reduced, and the latency of L1 accesses becomes a limiting resource, together with the memory latency. It is also interesting to observe how bottlenecks change from one scenario to the other, even
if they are not the ones that limit performance. 


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-cold-2500-bottleneck-overlap-illustrator.png"   width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-warm-2500-bottleneck-overlap-illustrator.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 1: Generalized roofline plot for MVM of square matrices 2500 × 2500, for (a) cold and (b) warm cache scenarios.</p>
</p>





## FFT: Bottlenecks for different sizes

Bottlenecks may also change with the input size. Figure 2 shows the generalized roofline plots for fast Fourier
Transforms (FFTs) of sizes 2^10 and 2^20. For the small size, L1 latency and bandwidth are the
limiting resources and the peak is not reached because of latency effects. For larger sizes, more
performance bounds appear because the application accesses more levels of the memory hierarchy,
and new execution stalls due to OoO buffers appear).
<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-fft-warm-1024-bottleneck-overlap-illustrator-memmodel.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-fft-warm-1048576-bottleneck-overlap-illustrator-memmodel.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 3: Generalized roofline plot for FFT of sizes (a) 1024 and (b) 1048576, warm cache.</p>
</p>





## MMM : Different implementations of the same application

Figure 4 shows the generalized roofline plots for (a)
a triple loop implementation and (b) a six-fold loop version, which is known to have better locality
and, hence, better performance as shown in the figure. In both implementations, execution stall
cycles due to the ROB occupancy and latency cycles of floating-point computations are important
contributors to the execution time, and the associated bottleneck lines hit the performance point.
While in the blocked implementation only the L1 latency appears as a bottleneck, in the triple
loop implementation, the L2 latency limits performance as much as the L1 latency. In none of the
cases the memory-related (mem) bottlenecks show up because the three matrices fit within the
last-level cache. The generalized roofline plots show that although MMM is known to be compute-bound [1], latency cycles
associated with memory accesses contribute largely to the execution time of the kernel, as much
as the cycles spent on computation.


<p align="center">

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mmm-warm-500-bottleneck-overlap-illustrator.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mmm-block-warm-500-bottleneck-overlap-illustrator.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<p style="width:image width px; font-size:90%; text-align:center;">
Figure 4: Generalized roofline plot for (a) MMM and (b) MMM blocked, warm cache.</p>
</p>



## SIMD applications
Figures 5 shows the generalized roofline plots for a symmetric rank-4 update operation for
small matrices of size 15 × 15 and 16 × 16, respectively. The main difference between the two
plots is that in the latter, the matrix size is multiple of the vector length, which results in higher
performance and operational intensity. The bottleneck for the kernel in the first figure is clearly the
L1 store bandwidth (ERM reports a large number of register spills for this kernel), while in the
kernel in the second figure, the overlap between L1-ld and vector additions are the main bottleneck.
In this case, all the throughput and bandwidth roofs exhibit high utilizations (issue and latency
bounds are close to the original throughput bounds) and there are no bottlenecks associated with
the reorder buffer and the store buffer, in contrast to the first plot.
 
<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/syrk-15-4-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-15-4-config-1.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/syrk-16-4-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-16-4-config-1.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<p style="width:image width px; font-size:90%; text-align:center;">
Figure 5: Generalized roofline plot for a symmetric rank-4 update operation for matrices of size (a) 15 × 15 and (b) 16 × 16, warm cache.
</p>
</p>



Figure 6 show the generalized roofline plots for a lower triangular linear system
solver of a small size (matrix of size 16, fits within the L1 cache) and a large size (matrix
of size 112, does no fit in the L1 cache), respectively. For the small size there is no tight bound that hits the performance of
the application, which means that once the code has been effectively vectorized, the main challenge optimizing code that fits within
the L1 cache is to rearrange computations to achieve high overlaps between computation and
data accesses. For the larger size, latency of L2 accesses and reservation station stalls
are the main bottlenecks.

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/ltrsv-16-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-16-config-1.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/ltrsv-112-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-112-config-1.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<p style="width:image width px; font-size:90%; text-align:center;">
Figure 6: Generalized roofline plot for a
lower triangular linear system
solver of a matrix of size (a) 16 and (b) 112, warm cache.</p>
</p>



## References

[1] K. Czechowski, C. Battaglino, C. McClanahan, A. Chandramowlishwaran, and R. Vuduc,
“Balance principles for algorithm-architecture co-design”, in Proceedings of Hot topics in
parallelism (HotPar), pp. 9–9, 2011.
