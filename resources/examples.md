# Examples



## Cold and warm cache: MVM

Figure1  analyzes a double-precision floating-point matrix-vector multiplication (MVM) run
with both cold and warm cache. In the former, memory latency (and bandwidth, which is included
in all memory bottleneck lines) is the bottleneck as expected (the matrix has to be loaded and
is not reused). The peak performance is not reached because dependences prevent the full issue
bandwidth to be used and introduce non-compute cycles due to RS stalls and arithmetic latency.
In the warm cache scenario, L1 becomes the limiting resource. The peak performance is not
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

## Stencil and k-means

## SIMD applications
