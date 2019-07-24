Vector Addition ~ Asymmetrical Multiple Compute Units (C)
=========================================================

This example demonstrates how multiple compute units can do parallel processing where the compute units are asymmetrical i.e. connected to different memory banks.

For kernels to execute concurrently, command queue is initialized with out of order execution mode enabled as shown below.

```c++
q = cl::CommandQueue(context, device,  CL_QUEUE_PROFILING_ENABLE | CL_QUEUE_OUT_OF_ORDER_EXEC_MODE_ENABLE, &err);
```

Kernel input and output ports can be connected to different memory banks for improved bandwidth. `--sp` option is used to specify such connections in `xocc` link stage.

```
LDCLFLAGS += --sp vadd_1.in1:DDR[0] --sp vadd_1.in2:DDR[0] --sp vadd_1.out_r:DDR[0] 
             --sp vadd_2.in1:DDR[1] --sp vadd_2.in2:DDR[1] --sp vadd_2.out_r:DDR[1]
```

SDAccel allows for different kernels to be associated with different CUs by name in xocc linking stage using `--nk` option.

```
    --nk vadd:4:vadd_1.vadd_2.vadd_3.vadd_4
```