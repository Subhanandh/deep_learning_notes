# Convolution
### Introduction

Extracting features from the image input is the most indispensable part of a CNN. And, CNN achieves it by incorporating convolution operations across different layers. In the initial layers, filters are trained to discover the elementary structures such as edges and its orientations using back propagation and gradient descent. Advancing to the higher layers, the filters learn to detect high level features of the image.

Mathematically, it is an operation that takes a given filter and slides it across the input image pixel-wise while performing elementwise multiplications and adding up the values (for each slide) that defines the value of each pixel in the output feature map. 


### Parameters defining a convolutional layer
  * Kernel/filter size - Kernel convolving with the input image can vary in dimensions that define how much the receptive field of an output image pixel should be. More clearly, it defines how much information a pixel can aggregate and represent from previous layers. 3x3, 5x5 and 1x1 kernels are commonly used dimensions.
  * Input and output channels - Kernels work on the input channels and produce output data channels; determined by the number of kernels used for convolution at that layer. 
  * Stride - Stride defines how much step a kernel should slide across the input sample during convolution. 1 stride convolution is the most widely used technique. However, it could be more than 1 whenever down-sampling is required.
  * Padding - The input sample could be padded with additional data in the border in order to preserve the output spatial dimensions when the kernel size is greater than 1.

| ![Convolution example](https://github.com/vdumoulin/conv_arithmetic/blob/master/gif/same_padding_no_strides.gif?raw=true) |
|:--:|
| *The above image of 5x5 pixels is convolved with a 3x3 kernel of stride 1. Padding of 1 cell is added across image borders in order to preserve the dimensions of resultant activation map. Here, the receptive field of each cell in the activation map is 3x3 (9 cells).* |

### Dimensionality of Convolutions.
Convolution can be performed with kernels of different number of dimensions depending on the kind of data and application the user tends to solve.
  * 1D convolution contains a single dimensional kernel to perform convolution. Audio and signal processing neural networks require this type of convolution.
  * 2D convolution contains 2-dimensional kernel to perform convolution. Image processing neural networks use 2D convolution. In a CNN, 2D convolution is performed across 3D input data.
  * 3D convolution uses three-dimensional kernel to perform convolution. Neural networks related with video processing that have additional temporal dimension can benefit from 3D convolutions. 

### Other types of 2D Convolution used in CNN

* Dilated convolution - Here, the convolution is done across interleaving pixels of input image. The interleaving space is determined by dilation rate. This type of convolution increases the receptive field of the activation map proportional to dilation rate. It is much useful in real time image segmentation.
| ![Example of a dilated convolution](https://github.com/vdumoulin/conv_arithmetic/blob/master/gif/dilation.gif?raw=true) |
| :--: |
| *3x3 Dilated convolution of dilation rate 1 is performed across a 7x7 input image resulting in a 3x3 activation map. Thus, each pixel in the activation map has 5x5 receptive field.*  |
* Transposed convolution - This convolution scales up the given input pixels by reversing the spatial resolution of the image pixels. It is identical to deconvolution except for their differnce in mathematical operations. Transposed convolutions are applicable in Encoder and Decoder type of network architectures.
| ![Example of a transposed convolution](https://github.com/vdumoulin/conv_arithmetic/blob/master/gif/no_padding_no_strides_transposed.gif?raw=true) |
| :--: |
| *Transposed convolution across a 2x2 input image is performed to obtain a 4x4 cross output image using a 3x3 convolution kernel.*  |
* Separable convolution - Here, convolution is performed using multiple 1D kernels instead of single 2D or 3D kernels such that dot product of these former kernels would result in the latter kernel. The benefit of this approach is reduced parameters in the model. For example, a 3x3 kernel can be replaced with two 1X3 kernels that result in the same 3x3 kernel upon performing dot product. So, the model requires inly 6 parameters instead of 9. EffNet, a novel architecture targeting to run CNN on mobile and embedded hardware has implemented this approach.
* Depth-wise separable convolution - Two set of convolutions are performed separately across each spatial dimension followed by channel dimension. It forms the core idea behind the inception modules of Xception architecture. It helps in decoupling cross-channel and spatial information of feature maps. MobileNet, an existing CNN powering mobile applications use this convolution.
