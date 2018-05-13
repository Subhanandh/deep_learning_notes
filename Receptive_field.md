# Receptive field

Receptive field is the patch of input pixels that define a pixel unit of activation map after convolving with a filter. And, a filter looks for a certain feature in that particular patch of pixels. So, receptive field is also known as Field of view of an output pixel.

Larger the receptive field, higher would be the amount of input pixels being be looked into. Receptive field can be increased in several ways. 
 * First option is to convolve the input with high dimensional filters.
 * Second option is to convolve the input with a stack of low dimensional filters.

Consider an input image of 12x12 spatial dimension. With a 7x7 filter of stride = 1, the output would be 6x6.  On the other hand, the same output dimension can be achieved by three 3x3 convolutions. The number of parameters in the first approach is 49 (7x7), higher than the parameters using second approach which totals around 27 (three times (3x3)). Moreover, the second approach leaves way for introducing additional non-linear layers in the network leading to increase in accuracy of predictions by improving representational strength of the model.

| ![Example](http://www.mdpi.com/remotesensing/remotesensing-09-00480/article_deploy/html/images/remotesensing-09-00480-g004.png =480x220) |
| :--: |
| *The above example uses a filter of size 3x3. A pixel of layer 2 has receptive field of 9 pixels (3x3). And, each pixel of layer 3 would have a higher receptive field, looking into 25 pixels of initial data.* |
Stacked up layers increase receptive field linearly. However, there are other ways to increase receptive field multiplicatively:
 * Subsampling methods such as pooling and striding with higher values. 
 * Using dilated convolutions with higher dilation rates.

### Effective receptive field 
Effective receptive field which reveals that not every pixel contributes equally to the output activation map and only a fraction of pixels contributes higher than the rest. Pixel at the center and pixels proximate to the center contribute exponentially higher than the pixels at the border. Thus, magnitude of pixel values and their gradients closer to the center would be higher as information is passed through these pixels are also high. The receptive field of final output looks like a gaussian distribution due to this innate nature. Using dilated convolutions in the network would produce rectangular effective receptive filed unlike circular ones.

| ![ECR](http://blog.christianperone.com/wp-content/uploads/2017/11/slice-post-receptive-field.png) |
| :--: |
| The above matrix represents the number of times an input pixel is involved in a convolution process. Pixels closer to the center are used relatively higher than border pixels.  |

Effective receptive field size increases after training process is finished. Better initialization schemes could increase receptive field size during initial stages of training. The receptive field is also highly dynamic while training the network as large gradient values passed through back propagation.

Biologically, fovea of human eye also has strong central vision and decayed peripheral vision. And, CNN uses the same concept.
