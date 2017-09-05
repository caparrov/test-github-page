<style>
body {
text-align: justify}
</style>


# Examples

Experimental setup. We model performance bounds for numerical kernels running on a
modern Intel Xeon; thus, we configure ERM to model this microarchitecture, according to the
parameters in Table 4.3. We emphasize that we don’t need access to the actual processor to
perform our analyses. In the generalized roofline plots shown, the original performance bounds
are shown as solid lines, our added issue, latency and stall bottlenecks as dashed lines, and overlap
bottlenecks as gray solid lines. Due to the usually high ( 0.95) measured overlaps, the latter
loose their curved shape.


AND SOME CONCLUSION



## Cold and warm cache: MVM

Figure 1 analyzes a double-precision floating-point matrix-vector multiplication (MVM) run
with both cold and warm cache. In the former, memory latency (and bandwidth, which is included
in all memory bottleneck lines) is the bottleneck as expected (the matrix has to be loaded and
is not reused). Further, long-latency operations like the memory accesses introduce non-compute cycles due to RS stalls. In the warm cache scenario, L1 becomes the limiting resource. The peak performance is not
reached because dependences again prevent the full issue bandwidth to be used and produce ROB
stalls and latency effects. It is also interesting to observe how bottlenecks change from one scenario to the other, even
if they are not the ones that limit performance. In the cold cache scenario, e.g., the computation
issue bottleneck suggests that even if the latency of floating-point operations and stall cycles are
removed, only half of the resources are being utilized; the reason is that the long latency memory
operations prevent the exploitation of available ILP from the application. This bottleneck is
considerable reduced in the warm cache scenario.


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-cold-2500-bottleneck-overlap-illustrator.png"   width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-warm-2500-bottleneck-overlap-illustrator.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 1: Generalized roofline plot for MVM of square matrices 2500 × 2500..</p>
</p>





## Bottlenecks for different sizes

Bottlenecks may also change with the input size. Figure 2 shows the generalized roofline plots for fast Fourier
Transforms (FFTs) of sizes 2^10 and 2^20. For the small size, L1 latency and bandwidth is the
limiting resource and the peak is not reached because of latency effects. For larger sizes, more
performance bounds appear because the application accesses more levels of the memory hierarchy,
and new execution stalls due to OoO buffers appear (most stalls are due to ROB). Actually, all
the possible bottleneck lines appear, which means that all buffers create execution stalls.

<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-fft-warm-1024-bottleneck-overlap-illustrator-memmodel.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-fft-warm-1048576-bottleneck-overlap-illustrator-memmodel.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: Generalized roofline plot for FFT of sizes (a) 1024 and (b) 1048576, warm cache.</p>
</p>





## Different implementations of the same application

We compare how bottlenecks change across different implementations of matrix-matrix multiplication
(MMM) of square matrices of size 500. Fig. 4.17 shows the generalized roofline plots for (a)
a triple loop implementation and (b) a six-fold loop version, which is known to have better locality
and, hence, better performance as shown in the figure. In both implementations, execution stall
cycles due to the ROB occupancy and latency cycles of floating-point computations are important
contributors to the execution time, and the associated bottleneck lines hit the performance point.
While in the blocked implementation only the L1 latency appears as a bottleneck, in the triple
loop implementation, the L2 latency limits performance as much as the L1 latency. In none of the
cases the memory-related (mem) bottlenecks show up because the three matrices fit within the
last-level cache. Note that although blocking is an optimization that targets improving locality,
it also reduces the floating point latency bottleneck.
The generalized roofline plots in Fig. 4.17 show that the notion of compute- and memorybound
change when more resources than just throughput and memory bandwidth are taken into
account. MMM is known to be compute-bound [23], and in fact it is with respect to the throughput
and memory bandwidth roofs. However, the generalized roofline plots show that latency cycles
associated with memory accesses contribute largely to the execution time of the kernel, as much
as the cycles spent on computation.


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mmm-block-warm-500-bottleneck-overlap-illustrator.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mmm-warm-500-bottleneck-overlap-illustrator.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: Generalized roofline plot for FFT of sizes (a) 1024 and (b) 1048576, warm cache.</p>
</p>




## SIMD applications
Figs. 4.20(a) and 4.20(b) show the result for a symmetric rank-4 update (syrk) operation for
small matrices of size 15 × 15 and 16 × 16, respectively. The main difference between the two
plots is that in the latter, the matrix size is multiple of the vector length, which results in higher
performance and operational intensity. The bottleneck for the kernel in Fig. 4.20(a) is clearly the
L1 store bandwidth (ERM reports a large number of register spills for this kernel), while in the
kernel in Fig. 4.20(b), it is the overlap between L1-ld and vector additions the main bottleneck.
In this case, all the throughput and bandwidth roofs exhibit high utilizations (issue and latency
bounds are close to the original throughput bounds) and there are no bottlenecks associated with
the reorder buffer and the store buffer, in contrast to the plot in Fig. 4.20(a).
Figs. 4.20(c) and 4.20(d) show the generalized roofline plots for a lower triangular linear system
solver (ltrsv) of a small size (matrix of size 16, fits within the L1 cache) and a large size (matrix
of size 112, does no fit in the L1 cache), respectively. For the small size, in contrast to most
generalized roofline plots analyzed so far, there is no tight bound that hits the performance of
the application. In general, we observe this behavior for small-size kernels, what confirms that
once the code has been effectively vectorized, the main challenge optimizing code that fits within
the L1 cache is to rearrange computations to achieve high overlaps between computation and
data accesses. In these cases, a more detailed overlap analysis, i.e., an analysis that considers not
only pairs of runtimes, but any combination of runtimes would yield tighter bounds. Although
we omit these bounds in the generalized roofline plot for ease of visualization, ERM reports
overlap percentages for every set of runtimes, including overlaps between runtime components
such as Tissue or Tlat. Although there is no tight bottleneck in the generalized roofline plot in
Fig. 4.20(c), the overlap between vector divisions and L1 accesses, and reservation station stalls,
are the main bottlenecks of this computation. For this kernel, shuffles also contribute largely to
performance. For the large size in Fig. 4.20(d), latency of L2 accesses and reservation station stalls
are the main bottlenecks. Finally, note that these kernels also perform blend vector operations;
however, since the fraction of blends with respect to the number of arithmetic computations W
is very small, the associated performance bound given by (4.29) is too high and does not appear
in plot.
Finally, Figs. 4.20(e) and 4.20(f) show the generalized roofline plots for matrix-matrix multiplications
with symmetric (symml) and upper/lower triangular matrices (lusmm), respectively.
In both cases, performance reaches approximately 50% of the peak performance of the platform
(higher performance than the previous SIMD applications analyzed). In both cases, it is hard to
pinpoint a single bottleneck because all resources contribute to performance. For the symmetric
matrix, for example, the overlap of vector additions with L1 load and L2 accesses is the main
bottleneck; however, ROB stalls and the latency of additions are also important limiters of performance.
For the generalized roofline plot in Fig. 4.20(f), the overlap of L1 loads with additions
and multiplications, and L1 load latency, and ROB stalls associated with L1 load accesses, are
the main bottlenecks.


<p align="center">
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/syrk-15-4-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-15-4-config-1.png
  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/syrk-16-4-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-16-4-config-1.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/ltrsv-16-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-16-config-1.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/ltrsv-112-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-112-config-1.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/sylmm-72-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-72-config-1.png"  width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/lusmm-86-emit-llvm-c-g-O3-fno-vectorize-fno-slp-vectorize-warm-git-243-86-config-1.png" width="80%" height="80%" style="border:0px;margin:10px" alt="Sublime's custom image"/>

<p style="width:image width px; font-size:90%; text-align:center;">
Figure 2: Generalized roofline plot for FFT of sizes (a) 1024 and (b) 1048576, warm cache.</p>
</p>



