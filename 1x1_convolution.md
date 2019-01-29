# 1x1 Convolution
### Introduction

1x1 Convolution was first mentioned in a research paper called Network in Network. This paper sites that 1x1 convolution is a way of achieving cross channel learning. And, it became popular with its usage in Inception module of GoogLeNet architecture. Lot of novel architectures could be found having adopted this operation within their networks. 

### Why do we need 1x1 convolution

* It acts as a feature mapper or merger rather than a conventional feature extractor. It maps the values across different input channels to different set of output channels while keeping the spatial dimensions such as width and height of the data untouched during convolution process. For example, consider an input tensor of size 10x10x32 and apply convolution using 16 kernels of size 1x1. It introduces 16x1x1x32 parameters in the layer. Aggregating the results across input channels produces an output tensor of size 10x10x16.

* The most significant advantage of 1x1 convolution is the reduction of the dimension of input channel. Otherwise, it could also mean reducing correlation across channels in the input. For example: An input of size WxHxC convolved with N kernels of size 1x1xC would result to WxHxN where N < C.

* By reducing the depth dimensionality, the number of channel parameters gets minimized. Hence, the computation cost of the output layer gets decreased.

### Example
| ![1x1 convolution example](https://raw.githubusercontent.com/iamaaditya/iamaaditya.github.io/master/images/conv_arithmetic/full_padding_no_strides_transposed_small.gif?raw=true) |
| :--: |
| *In the above convolution layer, a 1x1 filter is used to slide across a 7x7 input image with stride of 1 yielding an activation map of same spatial dimension.* |

However, the third dimension across channels are in turn reduced. The above example does not have a third dimension. So, the output map also has 1 channel dimension.

* It could be used to reduce the channel dimensions before performing spatial convolution with 3x3 or higher kernel sizes so as to reduce the computational complexity. GoogleNet architecture use this approach within their inception modules.

* It could be combined with stride value more than 1 to achieve both spatial and channel dimension reduction.
