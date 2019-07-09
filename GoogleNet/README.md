# GoogleNet

## Paper

[Going Deeper with Convolutions](http://arxiv.org/abs/1409.4842)

## Introduction

In this paper, authors focus on an efficient deep neural network architecture for computer vision, codenamed Inception, which derives its name from the famous “we need to go deeper” internet meme. Creating deeper models often poses following challenges:

* Bigger the model, more prone it is to overfitting. This is particularly noticeable when the training data is small.
* Increasing the number of parameters means you need to increase your existing computational resources.

This paper proposes a new idea of creating deep architectures. This approach lets you maintain the “computational budget”, while increasing the depth and width of the network.

## Architecture

### Inception Module

Inception module is an innovative architecture which uses all types of convolutions (3 × 3, 5 × 5) and lets the network decide for itself the optimal configuration! Authors do this by doing each convolution in parallel on the same input, with same padding i.e. spatial dimensions of output is same as the input, and concatenating the feature maps from each convolutions into one big feature map. This big feature map is fed as input to the next inception module.

In theory you can have as many filter sizes as possible, but the Inception Architecture is restricted to filter sizes 1 × 1, 3 × 3 and 5 × 5. **The small filters help capture the local details and features whereas spread out features of higher abstraction are captured by the larger filters.**

![image](https://mohitjainweb.files.wordpress.com/2018/06/inception-module-naive1.png)

One big problem with the above naive form of the inception module is that the large convolution are computationally expensive.

Consider a 5x5 convolution with 32 filters on an input of 28x28x192. In this case there would be total **(5^2)(192)(32)(28^2) = 120,422,400 operations**, which is a lot!

Authors choose to handle this using **Dimensionality Reduction**. This involves convolutions with 1x1 filters before convolutions with bigger filters.

Now, 28x28x192 --(1x1 with 16 filters)-> 28x28x16 --(5x5 with 32)-> 28x28x32

**Total computations: (1^2)(192)(16)(28^2) = 2,408,448 + (5^2)(16)(32)(28^2) = 10,035,200 = 12,443,648 operations**. This is a reduction by 10 times!

![image](https://mohitjainweb.files.wordpress.com/2018/06/inception-module-with-dimensionality-reduction.png?w=1024)

### GoogleNet Architecture

![image](https://mohitjainweb.files.wordpress.com/2018/06/googlenet-architecture-table.png?w=1024)

All the convolutions, including those inside the Inception modules, use rectified linear activation. The size of the receptive field in this network is 224×224 taking RGB color channels with mean subtraction. “#3×3 reduce” and “#5×5 reduce” stands for the number of 1×1 filters in the reduction layer used before the 3×3 and 5×5 convolutions. One can see the number of 1×1 filters in the projection layer after the built-in max-pooling in the pool proj column. All these reduction/projection layers use rectified linear activation as well

The network is 22 layers deep when counting only layers with parameters (or 27 layers if we also count pooling). The overall number of layers (independent building blocks) used for the construction of the network is about 100.

Another interesting addition to the architecture is to change the second last fully-connected layer with an average pooling layer. This layer spatially averages the feature map, converting 7 × 7 × 1024 input to 1 × 1 × 1024. Doing not only reduces the computation and the number of parameters, by a factor of 49, of the network but also improves the accuracy of the model, improving top-1 accuracy by 0.6%. 

To address the vanishing gradient problem, special extra structures are added to the network (these are removed during testing). These are auxiliary classifiers attached to intermediate layers which serve two purposes:

* Doing this makes the layers in the middle of the network more discriminative and thus make them able to extract better features.
* All the losses from each classifier gets added up, taking contribution from the auxiliary classifier lower than the main one, during training. The gradient from the main classifier which would have otherwise become very small, and thus slowing training, by time it reached the lower initial layers, receives gradient from the auxiliary classifiers and thus the net gradient becomes big enough to allow training to progress.

The exact structure of the extra network on the side, including the auxiliary classifier, is as follows:

* An average pooling layer with 5 × 5 filter size and stride 3, resulting in an 4 × 4 × 512 output for the (4a), and 4 × 4 × 528 for the (4d) stage.
A 1 × 1 convolution with 128 filters for dimension reduction and rectified linear activation.
A fully connected layer with 1024 units and rectified linear activation.
A dropout layer with 70% ratio of dropped outputs.
A linear layer with softmax loss as the classifier (predicting the same 1000 classes as the main classifier, but removed at inference time).

## GoogleNet Training