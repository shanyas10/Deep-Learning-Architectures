# VGGNet

## Paper

[“VGG” network paper: Very Deep Convolutional Networks for Large-Scale Image Recognition](https://arxiv.org/abs/1409.1556)

## Introduction
VGGNet is invented by VGG (Visual Geometry Group) from University of Oxford, Though VGGNet is the 1st runner-up, not the winner of the ILSVRC (ImageNet Large Scale Visual Recognition Competition) 2014 in the classification task, which has significantly improvement over ZFNet (The winner in 2013) [2] and AlexNet (The winner in 2012) [3]. There are various configurations of VGG mentioned in the paper. However, here we'll be discussing VGG 16 (Configuration D)

## Architecture
* Total no. of layers: 16 (13 conv layer + 3 FC layers)
* Input to the model is a fixed size 224×224 RGB image
* Preprocessing is subtracting the training set RGB value mean from each pixel
	### Convolutional layers
	* Stride fixed to 1 pixel
	* Padding is 1 pixel for 3×3
	* Spatial pooling layers
	* Spatial pooling is done using max pooling layers
		* Window size is 2×2
		* Stride fixed to 2
	* Convnets used 5 max pooling layers

	### Fully Connected layers
	* FC 1 and 2 have 4096 channels
	* FC 3(last layer) has 1000 channels (one for each class)
* Hidden layers are followed by Relu activation except the last layer which is softmax

## Training
* Loss function : Multinomial Logistic Regression
* Learning algorithm is mini-batch stochastic gradient descent(SGD) based on back-propogation with momentum
	* Batch size was 256
	* Momentum was 0.9
* Regularisation
	* L2 Weight decay (penalty multiplier was 0.0005)
	* Dropout for first two FC layers (set to 0.5)
* Learning rate
	* Intial: 0.01
	* Decreased by a factor of 10 when the validation set accuracy stopped improving. 
	* The learning rate was decreased 3 times, and the learning was stopped after 370K iterations (74 epochs).
* In spite of the larger number of parameters and the greater depth of VGG, the nets required less epochs to converge (compared to other nets like AlexNet) due to 
	(a) implicit regularisation imposed by greater depth and smaller conv. filter sizes; 
	(b) pre-initialisation of certain layers.
* Initialisation
	* For training deeper architectures(like VGG 16), they initialised the first four convolutional layers and the last three fully connected layers with the layers of net A (the intermediate layers were initialised randomly). 
	* For random initialisation (where applicable), they sampled the weights from a normal distribution with the zero mean and 10−2 variance. 
	* The biases were initialised with zero.
* Training Image Size
	* S is the smallest size of isotropically-rescaled image
	*	Two approaches were taken for setting S
		* Fix S, known as single scale training. Here S = 256 and S = 384
		* Vary S, known as multi-scale training
	* S from range [Smin, Smax] where Smin = 256, Smax = 512
	* Used scale jittering as one data augmentation technique during training
