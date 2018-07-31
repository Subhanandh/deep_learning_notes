## Angular softmax

Angular softmax was first introduced in the paper "*SphereFace: Deep Hypersphere Embedding for Face Recognition*" which aimed at solving Open-set face recognition problem. Since, testing identities could not be found in training set, Open-set FR
is essentially a metric learning problem rather than a classification problem.

Generally, face images possess less inter-class similarity but, more intra-class dispensation. So, learning such discriminative features is hard.
Euclidean margin used by existing loss functions such as contrastive loss, triplet loss and center loss is not compatible with softmax loss durian joint supervision naturally. To support the aforementioned statement, the distribution of softmax loss exhibit angular distribution (distribution arrived from 2D output of the FC-1 layer of CNN).

A-softmax being proposed in this paper leverages this intrinsic angular distribution to discriminate the features in a hypersphere manifold using angular distance metric. In a hypersphere manifold, this angular metric could be represented by geodesic distance. Altogether features learned in the hypersphere is known as SphereFace. Thereby, this loss function provides a clear geometric interpretation.
A-softmax also introduces an angular margin m which can be arrived such that the maximum intra-class distance is lesser than the minimum inter-class distance.


| ![](<https://image.slidesharecdn.com/face-recognition-mailru-highload-171109071450/95/face-recognition-from-scratch-to-hatch-57-638.jpg?cb=1510211815>)|
| :--: |
| *This figure shows how angular discrimination is possible on a hemispherical manifold*|
#### Loss function:

$L_{ang} = \frac{1}{N}\sum_i {-\log{( \frac{e^{||x_i||\psi(\theta_{y_i,i})}}{e^{||x_i||\psi(\theta_{y_i,i})} + \sum_{j \neq i} e^{||x_i||\psi(\theta_{j,i})}} )}}$

where $\psi(\theta_{y_i,i}) = (-1)\cos(m\theta_{y_i,i})-2k$,
$\theta_{y_i,i} \in [\frac{k\pi}{m}, \frac{(k +1)\pi}{m}]$,
$k \in [0, m-1]$,
$m\geq1$ and m controls the size of angular margin this parameter makes network learn angular margin between different identities.
Here, $\theta$ is the angle between feature vector x and weight W and x, W are taken from the last FC layer of network before calculating the loss. In the loss function, $W_i = 1, b_i = 0$ is set so as to make the prediction depend on $\theta$.

Each class maintains its own decision boundary. And, decision boundaries are more stringent and separated with the angular margin. A-Softmax makes a CNN learn features with geometrically interpretable angular margin by supervision.

Losses such as contrastive and triplet loss uses Euclidean margin while A-Softmax works with the nature of angular distribution using angular margin. Moreover, the former kind of losses could lead to data expansion while constituting pairs and triplets. This problem is alleviated using A-Softmax loss function.
