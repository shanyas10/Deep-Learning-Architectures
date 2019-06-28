# AlexNet

## Paper

 [ImageNet Classification with Deep Convolutional Neural Networks]( https://www.google.co.nz/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjvzL_N8MTVAhUFpZQKHcpODPsQFggoMAA&url=https%3A%2F%2Fpapers.nips.cc%2Fpaper%2F4824-imagenet-classification-with-deep-convolutional-neural-networks&usg=AFQjCNGDNgq60s_SjQnOE1S-bk3xAGeLjg )


## Introduction

Alex Krizhevsky, Geoffrey Hinton and Ilya Sutskever created a neural network architecture called ‘AlexNet’ and won Image Classification Challenge (ILSVRC) in 2012. They trained their network on 1.2 million high-resolution images into **1000 different classes** with 60 million parameters and 650,000 neurons. 

## Architecture

It contains **5 convolutional layers** and **3 fully connected layers**. Relu is applied after very convolutional and fully connected layer. **Dropout** is applied before the first and the second fully connected year. **The image size in the following architecutre chart should be 227 * 227 (instead of 224 * 224, pointed out by Andrei Karpathy in his famous CS231n Course)**

![AlexNet Architecture](https://engmrk.com/wp-content/uploads/2018/10/AlexNet_Original_Image.jpg)

### Convolutional Layers
#### Layer 1:
The input for AlexNet is a **227x227x3 RGB image** which passes through the first convolutional layer with **96 feature maps** or filters having **size 11×11** and a **stride of 4**. 
Then the AlexNet applies **maximum pooling layer** or sub-sampling layer with a **filter size 3×3** and a **stride of two**.

#### Layer 2:
Next, there is a second convolutional layer with **256 feature maps** having size 5×5 and a stride of 1. Then there is again a **maximum pooling layer** with **filter size 3×3** and a **stride of 2**. 

#### Layer 3,4,5:
The third, fourth and fifth layers are convolutional layers with **filter size 3×3** and a **stride of one**. The first **two used 384 feature maps** where the **third used 256 filters**.
The three convolutional layers are followed by a **maximum pooling layer** with **filter size 3×3**, a **stride of 2** and have **256 feature maps**.

### Fully Connected Layers
#### Layer 6, 7, 8:
Fully connected layers with **4096 units**

## Highlights:

* They used **Relu instead of Tanh to add non-linearity**. It accelerated the speed by 6 times at the same accuracy.
* To prevent overfitting, they used **Dropout with a rate of 0.5**. However, it almost doubled the training time.
* To reduce the size of network, **Overlap pooling** was used. It reduced the top-1 and top-5 error rates by 0.4% and 0.3%, repectively.
* **Local Response Normalization** was used to aid generalization. It reduced the top-1 and top-5 error rates by 1.4% and 1.2%,
respectively. (However, I have implemented the network using Batch Normalization)
* They spread the network across **two GPUs**. (I am, however, implementing a sequential network). This scheme reduces the top-1
and top-5 error rates by 1.7% and 1.2%, respectively, as compared with a net with half as many kernels in each convolutional layer trained on one GPU. The two-GPU net takes slightly less time to train than the one-GPU net2.
* For data augmentation, they perform **PCA on the set of RGB pixel values** throughout the ImageNet training set

