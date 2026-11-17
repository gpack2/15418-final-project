# 418 Final Project Proposal - CUDA Path Tracer

## Title:

A Path Tracing Renderer using CUDA - Gabe Pacella (gpacell2)

## URL:

https://gpack2.github.io/15418-final-project/

## Summary:

I am going to implement a GPU-accelerated path tracer using CUDA and C++, running on the NVIDIA GPUs in the GHC computers. The implementation will parallelize ray generation, intersection testing using bounding volume hierarchy (BVH) traversal, and light transport across thousands of rays.

## Background:

In the real world, objects are illuminated by direct and indirect light sources. A lamp illuminates a table by shining directly onto it, but that table also illuminates other objects indirectly by reflecting the lamp’s light onto them. So, in order to model this light behavior, we represent light using rays and represent the properties of light by following the paths of these light rays in a technique called path tracing.

Path tracing is a way to efficiently model the behavior of light by shooting a ray from the camera to every pixel in the image. When a ray intersects an object, it can bounce in a new direction depending on the object’s material. This process continues until a ray reaches it’s maximum number of bounces or hits a light source, and then the resulting light contributions are used to calculate the pixel’s color. 

Path tracing is computationally expensive because each ray may require multiple object intersection tests and bounces, and high resolution image can have millions of rays. However, all of these ray operations are entirely independent, which means we can process them in parallel on the GPU. Using CUDA, we can parallelize ray generation, intersection testing, and light transport across all GPU threads.

Determining object intersections (intersection testing) is the most costly computation for each ray, so to accelerate this, I plan to use a bounding volume hierarchy (BVH). A BVH organizes objects into a tree of bounding boxes, with the objects themselves as the tree nodes. Using this data structure, we can skip groups of objects that certainly cannot be intersected by the current ray. Each GPU thread will traverse the BVH for it’s own ray, allowing many rays to be processed in parallel.

## The Challenge:

The main challenge is efficiently parallelizing path tracing on the GPU. Although rays can be processed independently, they all can take different paths and require different amounts of bounces, leading to thread divergence and load imbalance. Also, the BVH tree can introduce irregular and non-homogenous memory accesses, since rays might traverse different parts of the tree. I hope to learn how to perform GPU and CUDA optimizations involving thread-blocks and memory accesses to increase rendering performance.

## **Resources:**

I plan to use the NVIDIA GPUs on the GHC machines and implement the path tracer primarily from scratch using C++ and CUDA. I will use [Ray Tracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html) as a reference for the basic path tracing implementation and [NVIDIA's Thinking Parallel](https://developer.nvidia.com/blog/thinking-parallel-part-ii-tree-traversal-gpu/) for GPU BVH traversal techniques. I will also use CUDA profiling tools to identify potential optimizations. If time permits, I would like use my AMD GPU to explore porting the implementation to HIP/ROCm.

## Goals and Deliverables:

Plan to Achieve (100%):

- Implement a fully-functional CUDA path tracer supporting ray generation, intersection testing, multiple ray bounces, and basic materials
- Implement BVH traversal for intersection testing
- Optimize GPU performance with CUDA thread-block sizes, memory organization, and other optimizations
- Evaluate and measure rendering performance across different scenes, image resolutions, samples-per-pixel, scene complexities, and ray bounces

Hope to Achieve (125%):

- Add complex materials, like reflections and refractions, and support more detailed scenes
- Port the CUDA path tracer to HIP using AMD’s ROCm platform to run on my AMD gpu and explore NVIDIA vs AMD architecture, performance, and programming experience
- A live demo that displays rendered scenes with adjustable parameters

Minimum to Achieve (75%):

- Implement a fully-functional CUDA path tracer with basic materials and multiple ray bounces, but without BVH traversal and additional rendering features.
- Profile and optimize memory organization and block-size
- Evaluate and measure rendering performance across different scenes, image resolutions, samples-per-pixel, scene complexities, and ray bounces

## Platform Choice:

I plan to use C++ and CUDA on the NVIDIA GPUs in the GHC machines. Path tracing is well suited for GPU parallelization in CUDA because thousands of rays can be computed independently across thousands of threads. If time permits, I hope to port the implementation to HIP and run it on an AMD GPU to explore differences between the two architectures.

## Schedule:

**Week 1 (11/16–11/22): Basic Path Tracer Implementation**

- Implement camera, ray generation, and ray-object intersection routines in C++
- Implement basic CUDA kernels to parallelize ray generation and intersection testing
- Add multiple ray bounces, basic materials, and image output

**Week 2 (11/23–11/29): BVH Acceleration**

- Implement BVH construction and GPU-based traversal
- Compare rendering performance with and without BVH acceleration
- Debug and verify rendering correctness

**Week 3 (11/30–12/6): Optimization and Evaluation**

- Profile CUDA kernels to identify performance bottlenecks
- Experiment with thread-block sizes, memory organization, and other optimizations
- Benchmark different scene complexities, resolutions, sample counts, and bounce depths

**Week 4 (12/7–12/10): Final Evaluation and Presentation**

- Complete remaining optimizations and performance measurements
- Generate example scenes and graphs showing performance results
- Prepare the final report, poster, and optional live demo for the parallelism competition