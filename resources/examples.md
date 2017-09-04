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
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-cold-2500-bottleneck-overlap-illustrator.png"   width="36%" height="36%" alt="Sublime's custom image"/>
<img src="https://raw.githubusercontent.com/caparrov/test-github-page/master/resources/images/data-rooflinePlot-mvm-warm-2500-bottleneck-overlap-illustrator.png" width="62%" height="62%" alt="Sublime's custom image"/>
<p style="width:image width px; font-size:90%; text-align:center;">
Figure 1: Generalized roofline plot for MVM of square matrices 2500 × 2500..</p>
</p>





## Bottlenecks for different sizes

## Different implementations of the same application

## Stencil and k-means

## SIMD applications
