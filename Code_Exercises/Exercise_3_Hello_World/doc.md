# SYCL Academy

### Exercise 3: Hello World

---

In this first exercise you will learn:
* How to create and submit a command group.
* How to define a SYCL kernel function.
* How to stream output from a SYCL kernel function.

---

Once you have a queue you can now submit work to be executed on your chosen
device and this is done via command groups, which are made up of commands and
data dependencies.

1.) Define a command group

Define a lambda to represent your command group and pass it to the submit member
function of the queue.

Note that submitting a command group without any commands will result in an
error.

2.) Define a SYCL kernel function

Define a SYCL kernel function via the `single_task` command within the command
group, which takes only a function object which itself doesn't take any
parameters.

Remember to declare a class for your kernel name in the global namespace.

3.) Stream “Hello World!” to stdout from the SYCL kernel function

Create a `stream` object within the command group scope as follows. The two
parameters to the constructor of the `stream` class are the total buffer size
and the statement size respectively.

Then use the stream you constructed within the SYCL kernel function to print
“Hello world!” using the `<<` operator.


4.) Try another command

Instead of `single_task` try another command for defining a SYCL kernel function
(see [SYCL 1.2.1 specification][sycl-specification], sec 4.8.5).

Remember the function object for the `parallel_for` which takes a `range` can be
an `id` or an `item` and the function object for the `parallel_for` which takes
an `nd_range` must be an `nd_item`.

5.) Try a different dimensionality

Instead of a 1-dimensional range for your SYCL kernel function, try a 2 or
3-dimensional range.

#### Build And Execution Hints

For For DPC++ (using the Intel DevCloud):

```
dpcpp -o sycl-ex-3 -I../External/Catch2/single_include ../Code_Exercises/Exercise_3_Hello_World/source.cpp
```

In Intel DevCloud, to run computational applications, you will submit jobs to a queue for execution on compute nodes,
especially some features like longer walltime and multi-node computation is only abvailable through the job queue.
Please refer to the [example][devcloud-job-submission].

So wrap the binary into a script `job_submission` and run:
```
qsub job_submission
```

For ComputeCpp:
```
cmake -DSYCL_ACADEMY_USE_COMPUTECPP=ON -DSYCL_ACADEMY_INSTALL_ROOT=/insert/path/to/computecpp ..
make Exercise_3_source
./Code_Exercises/Exercise_3_Hello_World/Exercise_3_source
```

For hipSYCL:
```
# Add -DHIPSYCL_GPU_ARCH=<arch> to the cmake arguments when compiling for GPUs.
# <arch> is e.g. sm_60 for NVIDIA Pascal GPUs, gfx900 for AMD Vega 56/64, and gfx906 for Radeon VII.
cmake -DSYCL_ACADEMY_USE_HIPSYCL=ON -DSYCL_ACADEMY_INSTALL_ROOT=/insert/path/to/hipsycl -DHIPSYCL_PLATFORM=<cpu|cuda|rocm> ..
make Exercise_3_source
./Code_Exercises/Exercise_3_Hello_World/Exercise_3_source
```
alternatively, without cmake:
```
cd Code_Exercises/Exercise_3_Hello_World
HIPSYCL_PLATFORM=<cpu|cuda|rocm> HIPSYCL_GPU_ARCH=<arch-when-compiling-for-gpu> /path/to/hipsycl/bin/syclcc -o sycl-ex-3 -I../../External/Catch2/single_include source.cpp
./sycl-ex-3
```

*Note:* Printing from kernels is still experimental on ROCm, so you might get an empty output when using the hipSYCL ROCm backend. In this case, try using the CPU backend instead.

[sycl-specification]: https://www.khronos.org/registry/SYCL/specs/sycl-1.2.1.pdf
[devcloud-job-submission]: https://devcloud.intel.com/oneapi/learn/advanced-queue/basic-job-submission#command-file-job-script-