## Style Transfer

Neural style transfer is an optimization technique used to take two images—a content image and a style reference image (such as an artwork by a famous painter) and blend them together so the output image looks like the content image, but “painted” in the style of the style reference image.
This is implemented by optimizing the output image to match the content statistics of the content image and the style statistics of the style reference image. These statistics are extracted from the images using a convolutional network.

Ex: 

<img src='https://149695847.v2.pressablecdn.com/wp-content/uploads/2020/06/1_kOQOZxBDNw4lI757soTEyQ.png' width=800/>

## Understanding how a Neural Style Transfer works

If we want to code a Neural Style Transfer, the first thing we must do is to perfectly understand how convolutional neural networks work. In my case, I already talked about them in this post, although today it will be more specific.

In order for a Neural Style Transfer network to work, we must achieve at least two things:

* Convey the style as much as possible.
* Assure that the resulting image look as close to the original image as possible.

We could consider a third objective: making the resulting image as internally coherent as possible. This is something that Keras’s implementation includes but that, in my case, I am not going to dive into.

To do this, a neural style transfer network has the following:

* A convolutional neural network already trained (such as VGG19 or VGG16). Three images will be passed to this network: the base image, the style image, and the combination image. The latter could be both a noise image and the base image, although generally the base image is passed in order to make the resulting image look similar and to speed up the process.
* The combined image is optimized and steadily changes in such a way that it takes the styles of the style image while maintaining the content of the base image. To do this, we will optimize the image using two different losses (style and content).

<img src='https://miro.medium.com/max/1924/0*F4F_8DzBkBFh3XWi' width=800/>


## How to transfer the style of an image

In convolutional neural networks, the deeper we go into the network, the more complex shapes the network distinguishes. This is something that can be clearly seen in the ConvNet Playground application, which allows you to see the layer channels at different “depths” of the network.

Therefore, if we want to transfer the style of an image, we will have to make the values of the features of the deep layers of our network look like those of the network of the style image.

But how can we calculate the loss function of this process in order to adjust it? For this, we use the so-called Gram Matrix.

## The Gram Matrix

Suppose we want to calculate the style loss on a layer. To do this, the first thing we must do is flatten our layer. This is a good thing, as the Gram Matrix calculation will not change based on the size of the layer.

<img src='https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_468/https://anderfernandez.com/wp-content/uploads/2020/10/Gram-Matrix.png' >

So, suppose we do a flatten on a layer of 3 filters. Well, the Gram Matrix shows the similarity between the filters and is obtained by calculating the dot product between the vectors:
When we are calculating dot products, we are taking the distance into account: the smaller the dot product, the closer the two vectors are and vice versa. 

## Content and Style images

<img src='https://github.com/EssamMohamedAbo-ElMkarem/Neural-Style-Transfer/blob/main/imgs/content.jpg' width=700 />

<img src='https://github.com/EssamMohamedAbo-ElMkarem/Neural-Style-Transfer/blob/main/imgs/style.jpg' width=700 />

## Output of 200 Iterations

<img src='https://github.com/EssamMohamedAbo-ElMkarem/Neural-Style-Transfer/blob/main/imgs/200.png' width=700 />
