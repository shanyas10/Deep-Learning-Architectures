# ZFNet

## Paper
[Visualizing and Understanding Convolutional Networks](https://arxiv.org/abs/1311.2901)

## Interesting Points

* Visualization of Convolutional Layers
* Ablation study to identify performance contribution from different model layers (e.g. cnn vs fc layer - which contributes more)
* Sensitivity analysis through occluding portions of an image
* Results surpassed AlexNet as visualizations helped find better hyperparameters

## Implementation

### Deconvolution

Deconvolution, also known as Transposed Convolution goes back from the output dimension to the input dimension. This deconvolution is used during backpropogation of error through the convolutional layer.

Visualizations produced are reconstructed patterns from an input image coming from the validation set that cause high activations in a given feature map. To produce input images that result in maximum activations, a separate Deconvnet is attached to the output of each layer whose features we want to visualize. Each Deconvnet itself is a neural network whose input is activations of a particular layer, and the output is an image depicting pixels that are responsible for maximum activation of a convolutional kernel of our interest for a given convolutional layer. In the paper, Deconvnet is described as a sequence of transposed convolutions, de-pooling and a special modified ReLU (sequence is the reverse of the steps taken to produce the layer's output). The output of the Deconvnet has the same dimensions as the input image in the convnet we are visualizing.

De-pooling operation (de-pool) is expanding dimensionality of data by remembering the pooled positions in the forward pass of the network we want to visualize. 
![image](https://www.yjpark.me/assets/expressions/Unpooling.png)
Modified ReLU (relu*) only passes forward positive activation, this is equivalent to backpropagating only positive gradients, which is different from backpropagating through a regular ReLU. This is why the authors call it modified.

### Visualizations

![image](https://pechyonkin.me/images/201901-zfnet/layer12.png)
![image](https://pechyonkin.me/images/201901-zfnet/layer3.png)

Through the visualization of the features learned by the convolution kernels of each layer, it is found that the features learned by the neural network have a hierarchical structure.
* The second layer is to learn edge and corner detectors.
* The third layer learnt some texture features.
* The fourth layer learnt some invariant features for the specified category of images, such as dog faces and bird legs.
* The fifth layer obtains more prominent features of the target and acquires position change information.
* The low-level features are more stable after less epoch training, and the higher the number of layers, the more epoch is needed for training. Therefore, enough epoch processes are needed to ensure smooth model convergence.

### Architecture

![image](http://www.programmersought.com/images/309/4f9d8825a2f2587f4d5c775ec8841d0d.png)
By visualizing the characteristics of the first and second layers of AlexNet, the authors found that the features of the larger stride and convolution kernel extraction are not ideal, so the authors changed the first layer from the convolution kernel 11*11 to 7*7, the stride is reduced from 4 to 2. The experiment showed that this helped improve the classification performance.

### Occlusion Analysis

The occlusion experiment shows that occluding the key area of ​​the image is has a great influence on the classification performance, indicating that the model clearly locates the object in the scene during the classification process.

![image](http://www.programmersought.com/images/477/a44f1185d8b6c2e188977cb284217a15.png)
