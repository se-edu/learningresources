# An Introduction to GPGPU

###### Author: [Pierce Anderson Fu](https://github.com/PierceAndy)

- [GPGPU](#-1-gpgpu)
    - [What is GPGPU?](#-11-what-is-gpgpu)
    - [Why bother with parallel processing?](#-12-why-bother-with-parallel-processing)
    - [Aren't multicore CPUs enough?](#-13-arent-multicore-cpus-enough)
    - [What are the challenges with GPGPU?](#-14-what-are-the-challenges-with-gpgpu)
    - [Implementations](#-15-implementations)
- [Further Readings](#-2-further-readings)
- [References](#-3-references)

## § 1. GPGPU

### § 1.1 What is GPGPU?
GPGPU stands for General-purpose computing on graphics processing units. It is the use of a graphics processing unit (GPU), which typically handles computation only for computer graphics, to perform computation in applications traditionally handled by the central processing unit (CPU).<sup>[[1]](#footnote1)</sup>

Simply put, it's a kind of parallel processing where we're trying to exploit the data-parallel hardware on GPUs to improve the throughput of our computers.

### § 1.2 Why bother with parallel processing?
Moore's law is the observation made by Gordon Moore that the density of transistors in an integrated circuit board doubles approximately every two years. It has long been co-opted by the semiconductor industry as a target, and consumers have taken this growth for granted.

Because it suggests exponential growth, it is unsustainable and it cannot be expected to continue indefinitely. In the words of Moore himself, "It can't continue forever.".<sup>[[2]](#footnote2)</sup> There are hard physical limits to this scaling such as heat dissipation rate<sup>[[3]](#footnote3)</sup> and size of microprocessor features.<sup>[[4]](#footnote4)</sup>

As software engineers, this means that free and regular performance gains can no longer be expected.<sup>[[5]](#footnote5)</sup> To fully exploit CPU throughput gains, we need to code differently.

### § 1.3 Aren't multicore CPUs enough?
Between CPUs and GPUs, there are differences in **scale** and **architecture**.
- In terms of **scale**, CPUs only have several cores while GPUs house up to thousands of cores.
- In terms of **architecture**, CPUs are designed to handle sequential processing and branches effectively, while GPUs excel at performing simpler computations on large amounts of data.

This means that CPUs and GPUs excel at different tasks. You'll typically want to utilize GPGPU on tasks that are data parallel and compute intensive (e.g. graphics, matrix operations).

> ##### Definitions:
>
> *Data parallelism* refers to how a processor executes the same operation on different data elements simultaneously.
>
> *Compute intensive* refers to how the algorithm will have to process lots of data elements.

### § 1.4 What are the challenges with GPGPU?
Not all problems are inherently parallelizable.

The SIMT (Single Instruction, Multiple Threads) architecture of GPUs means that they don't handle branches and inter-thread communication well.

### § 1.5 Implementations

- CUDA: [Official website](http://www.nvidia.com/object/cuda_home_new.html)
- OpenCL: [Official website](https://www.khronos.org/opencl/)

## § 2. Further Readings
- [How concurrency is the next big change in software development since OO](http://www.gotw.ca/publications/concurrency-ddj.htm)
- [Official CUDA C programming guide: What GPUs excel at processing, and why](http://docs.nvidia.com/cuda/cuda-c-programming-guide/#from-graphics-processing-to-general-purpose-parallel-computing)
- [Official CUDA C programming guide: Architecture of NVIDIA GPUs](http://docs.nvidia.com/cuda/cuda-c-programming-guide/#simt-architecture)
- [Lightning talk slides: An Introduction to GPGPU](https://github.com/nus-oss/lightningtalks/issues/10)

## § 3. References

<a name="footnote1">[1]</a>: https://en.wikipedia.org/wiki/General-purpose_computing_on_graphics_processing_units<br />
<a name="footnote2">[2]</a>: http://www.techworld.com/news/operating-systems/moores-law-is-dead-says-gordon-moore-3576581/<br />
<a name="footnote3">[3]</a>: http://theory.physics.lehigh.edu/rotkin/newdata/mypreprs/spie-09b.pdf<br />
<a name="footnote4">[4]</a>: https://arstechnica.com/gadgets/2016/07/itrs-roadmap-2021-moores-law/<br />
<a name="footnote5">[5]</a>: http://www.gotw.ca/publications/concurrency-ddj.htm<br />
