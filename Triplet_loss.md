## Triplet Loss


##### FaceNet

Triplet loss was first introduced in the paper called *FaceNet: A Unified Embedding for Face Recognition and Clustering*. The problem this paper tried to solve was to perform face verification with a variable number of classes. And, the number of classes are not fixed and limited unlike existing supervised learning methods. Each time the model looks at a face, it has to verify if it is identical to any of the existing classes. Here, the loss function was defined to train face embeddings rather than bottleneck layers (used in previous face recognition models). It was called Triplet loss.

This loss function encourages all faces of one identity to be projected onto a single point in the embedding space. FaceNet uses Euclidean space for this purpose and L2 distance corresponds to the similarity of images.

A margin is enforced between different identities in the embedding space thereby making identical images come into the same manifold.


| ![Triplet distance learning](https://cdn-images-1.medium.com/max/1600/0*_WNBFcRVEOz6QM7R.) |
| :--: |
| *This figure shows the change in proximity of anchor towards Positive, moving away from the Negative .* |

* Triplet loss works on the embedding produced by L2 loss function on the results obtained from CNN.
* A triplet consists of an anchor, a positive having same identity of a given anchor and a negative having a different identity. And, triplet loss minimizes the distance between the anchor and positive while increasing the anchor to negative distance.
* Loss function
$Loss = \sum_{i=1}^{N} [||f(x_i^a) - f(x_i^p)||_2^2 - ||f(x_i^a) - f(x_i^n)||_2^2 + \alpha ]_+$
$\forall(f(x_i^a),f(x_i^p),f(x_i^n)) \in \tau$ and a, p, n denotes anchor, positive and negative.
Here, the loss function imposes a margin of $\alpha$ that defines the minimum distance a negative should maintain from positive. The network is trained to minimize this loss function.

| ![](https://omoindrot.github.io/assets/triplet_loss/online_triplet_loss.png)|
| :--: |
| *Structure of a FaceNet Model using online triplet mining.* |
* Triplet selection plays the vital role in ensuring fast convergence. Triplets can be generated either online or offline and the former was found to be efficient. Triplets for which the negative being closer to anchor than the positive are called hard triplets and, the triplets with positive closer to anchor than negative but, distance of negatives still being within margin are semi-hard triplets. Using online method to train the dataset, a pair of anchor-positive and semi-hard or hard negatives are used in each min-batch. It leads to faster convergence. Initial trainings are made using semi-hard negatives.

##### Applications of triplet loss:
* Face recognition
  * Face verification (*is this the same person*) - using thresholds to verify person. 
  * Face recognition (*Who is the person*) - using kNN classification
  * Face clustering - using existing clustering methods.
* Fashion similarity - Based on the item a customer is viewing or adding to the wishlist, the website can recommend similar clothes based on the appearence such as color and shape.

References: 
[[1503.03832] FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/abs/1503.03832)
[Triplet Loss and Online Triplet Mining in TensorFlow | Olivier Moindrot blog](https://omoindrot.github.io/triplet-loss)
[Home | Silversparro](https://www.silversparro.com/single-post/2018/01/26/Using-Deep-Learning-for-fashion-similarity-and-face-recognition)
