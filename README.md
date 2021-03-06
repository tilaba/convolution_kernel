#### About the Project

During the training of Convolutional Neural Networks (CNNs), the convolutional layer is the most time consuming layer. So, we wanted to accelerate the forward pass convolution operation on GPUs which would obviously reduce the time taken in the convolutional layer.

Researchers are actively working on different ways to reduce the time complexity of different convolution methods including Winograd algorithm, FFT based convolution etc.,

Based on the literature survey, we found that very few researchers are working on accelerating the general matrix multiplication(GEMM) based convolution by the usage of efficient memory access patterns. On noticing it, we planned to implement and verify any one of their techniques.

Our implementation of the convolution kernel is based on the algorithms mentioned in the conference paper titled ["Optimizing Memory Efficiency for Convolution Kernels on Kepler GPUs"](https://arxiv.org/abs/1705.10591) which was accepted at DAC'17.

Our implementation is benchmarked against the single-precision general matrix multiplication(SGEMM) based convolution kernel  available in NVIDIA's cuDNN library with the help of `nvprof`.

Special thanks to [Peter Goldsborogh](https://github.com/goldsborough) for his [blogpost](http://www.goldsborough.me/cuda/ml/cudnn/c++/2017/10/01/14-37-23-convolutions_with_cudnn/) and [gist](https://gist.github.com/goldsborough/865e6717e64fbae75cdaf6c9914a130d) which explained the usage of convolution algorithm routine available in the cuDNN library. Without his work, It would have been a tough time for us battling with the cuDNN developer guide to benchmark our kernel.

#### Benchmarking Environment

> OS : Ubuntu 16.04.3 LTS

> GPU : GeForce GTX 650 Ti BOOST

> CUDA Driver Version : 9.0

> CUDA Runtime Version : 8.0

> CUDA Capability Version : 3.0

> cuDNN Major Version : 7


#### Benchmarking Results
For the purpose of benchmarking, We are naming `our implementation of the memory-efficient kernel` as `Kernel A` and `the SGEMM based convolution kernel of cuDNN` as `Kernel B`.

Here are some of the results from the benchmarking process,

For a stride value of `1`, a filter dimension of `3*3` and number of channels to be `1`,


| Kernel | Image Dimension | Avg. Time|
| :--- | :---: | ---: |
| Kernel A | 2048*2048 | **8.2038 milli.secs** |
| Kernel B | 2048*2048 | 15.149 milli.secs |
| Kernel A | 1024*1024 | **2.0776 milli.secs** |
| Kernel B | 1024*1024 | 3.7918 milli.secs |
| Kernel A | 512*512 | **531.65 micro.secs** |
| Kernel B | 512*512 | 955.65 micro.secs |


From the above table, it can be clearly seen that `Kernel A` outperforms `Kernel B` by a ~50% reduction in the time taken for computation.
