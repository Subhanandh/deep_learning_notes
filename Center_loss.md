# Center Loss

The term center loss first appeared in the research paper called *A Discriminative Feature Learning Approach for Deep Face Recognition.* The primary objective of this paper was to introduce a novel method to improve the discriminatory power of the deeply learned features. A new loss function called center loss was proposed in order to serve this purpose.

During the test time, unseen classes cannot be identified without using deeply learned features that are sufficient enough to discriminate the features based on class identity. Softmax loss function used in the CNN use separable features in the last layer to classify images during the course of learning. However, the function does not exploit the discriminative features to perform this task. Combining existing softmax loss with Center loss could make network utilize both kind of features to encourage better inter-class dispensation and intra-class compactness for face recognition.

Then existing loss functions such as Contrastive loss and triplet loss were not efficient as the image pairs and triplets used in respective networks grew drastically resulting in slower convergence and instability and required wise image pair or triplet selection strategy.

#### Loss function
The loss function ensures that the model learns a class center c for each class. c is a vector having same dimension of a feature.
$L_c = \frac{1}{2}\sum_{i=1}^{m}||x_i - c_{y_i}||_2^2$

where $c_{y_i}\in R^d$ - $y_i$th class center

* $c_{y_i}$ gets updated by averaging features of corresponding classes for every mini-batch using SGD. 
* A parameter $\alpha$ was introduced to control large perturbations in the updating of  center during training.

#### Joint Softmax and Center loss function
The joint supervision of softmax loss and center loss contribute enhance discriminative power. 
$L = L_s + L_c$
$L = -\sum_{i=1}^{m}\log\frac{e^{W_{y_i}^{T}x_i +b_{y_i}}}{\sum_{j=1}^{n}e^{W_{j}^{T}x_i +b_{j}}}  + \frac{\lambda}{2}\sum_{i=1}^{m}||x_i - c_{y_i}||_2^2$

where $\lambda$ is a hyper parameter introduced to balance the loss functions. Modifying the value of $\lambda$ yield different distributions and thus, a proper $\lambda$ is necessary to make significant enhancement in discriminating the classes.
|![](https://camo.githubusercontent.com/04a8ab0730c9c1fad719a07c060290d1c55aa6ff/687474703a2f2f3778736337382e636f6d312e7a302e676c622e636c6f7564646e2e636f6d2f63656e7465726c6f73735f6578616d706c652e6a7067)|
| :--: |
| *The above images show distributions of deeply learned features for MNIST dataset using last two hidden layers. x and y axes denote activations of 1st and 2nd neurons. The first figure uses only softmax loss where it makes classes separable. However, second one adds more intra-class minimization, moving data points closer to class centers and increases the inter-class dispensation too.* |
Softmax loss keeps the classes separated while the center loss pulls identical images to same class.

Using only the center loss would gradually degrade learned features and centers to zeros. Alternatively, using the softmax loss would cause large intra-class variations. But, combining them yields better results.

References:
<https://ydwen.github.io/papers/WenECCV16.pdf>
[GitHub - pangyupo/mxnet_center_loss: implement center loss operator for mxnet](https://github.com/pangyupo/mxnet_center_loss)