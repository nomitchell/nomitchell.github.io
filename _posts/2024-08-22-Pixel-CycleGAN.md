## Pixel Art CycleGAN

This post is a little late from my earlier deadline of Sunday, but I ran into some issues with deciding on a project for this post. 

Over the last few days, I have been training a CycleGAN model to convert images to a pixel art style. The goal of this project is to generate pixel art images for the training of a text-to-image model. I want to train such a model for a game I am working on, with generative models as the story generator (rimworld's story generator but more dynamic and open ended). More on this will come in a later post.

Feeling lazy, I used a premade implementation of CycleGAN at this repo -> https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix. Training was done with an aggregate of different datasets and images I collected, totalling about ~4000 images in both the A and B batches (8k total). For the pixel art style images, I used a preconverted subset of 2000 images from the diffusionDB dataset. These images were naively converted to pixel art so their quality is dubious and may have done more harm than good to the underlying model. I plan on trying to retain the model without these at some point, and with some higher quality data. I also collected images from this kaggle dataset -> https://www.kaggle.com/datasets/artvandaley/curated-pixel-art-512x512, google images (searching pixel art, beautiful pixel art, scenary pixel art, character pixel art, etc), and pixel art gallery sites. In the non-pixel image category, I took stuff from google images, diffusiondb, character sprites (non pixel), anime images (sfw danbooru, google), and probably some other things I can't remember. 

The model was trained over the course of 2-3 days, or roughly 70 epochs. I initially planned on training for 100 epochs, however, I started getting error messages relating to my graphics drivers popping up and had to call it short. I am happy with the quality of the generations, however, I regret not being more consistant in my pixel size for the training images. The model as it is now arbitrarily uses different pixel dimensions for the images, with some images being much more detailed than others after being converted. Regardless, I believe it will work for my purposes as an initial test of sort. 

Some example images 
1. 
![518](https://github.com/user-attachments/assets/32c294dc-72a8-450b-bd2d-d6409ed74ffd)
![3534image](https://github.com/user-attachments/assets/fd037d3f-196e-42ce-a613-86b0a19232d5)

2.
![3280](https://github.com/user-attachments/assets/65e28b8d-2648-4e23-890d-e9f45f2f0b5a)
![2536image](https://github.com/user-attachments/assets/1d4a5a28-ff3f-464f-84b9-4aaffef81ba5)

3.
![4043](https://github.com/user-attachments/assets/367f3eb9-fad9-4945-9f9d-0b078c7af328)
![3384image](https://github.com/user-attachments/assets/084d59a1-dc77-47c5-a2a1-653f03273a76)

4.
![4050](https://github.com/user-attachments/assets/a74b14d2-38c1-4a42-9f01-61ab82f0e891)
![3392image](https://github.com/user-attachments/assets/cfcc3b1a-625d-4454-a330-f9ecd3e8d684)

5.
![3179](https://github.com/user-attachments/assets/9499f0d1-f039-41a4-ac6c-c033746d35b3)
![2423image](https://github.com/user-attachments/assets/9e7bbdd1-dde7-40cd-8152-4cd0c3302790)

As is typical in ML blog posts, I must now explain what CycleGAN is and how it works. CycleGAN is an image-image translation method that lets you impart the style of one set of images to another set. The cool thing about CG is that it doesn't require paired images. As long as one set has the style (pixel) and the other set is the domain you want to apply it to, you might be good. 

CG is a GAN method, which stands for generative adversarial network. Typically, this architecture has both a generator model, and a discriminator model. The goal of the generator is to generate novel elements (images in our case) that are indistinguishable from the target dataset. On the other hand, the goal of the discriminator is to identify whether an image is real (from the dataset) or fake (generated). An adversarial loss function is used to measure how well these components are performing and to update their weights.

For CG, there are two generates and discriminators. One set is responsible for translating images from domain X to Y (example, normal to pixel), while the other is for Y to X. We will call the models that translate our images from one domain to the other functions G and F. This method relies on an assumption based on translation cycle consistancy. The example given in the original research paper is that when translating a sentence from English to French, then back to English again, you should retain the same sentence. Here, a cycle consistency loss function is used to measure how similar an image is to when it started after passed through functions G and F. For example, if we pass image x into function G, we will get translated image y (G(x) = y). Putting image y into function F should theoretically give us back image x. Cycle loss is the measure of how well the models are capable of this identity-like process. 

The cycle consistency loss component is important for the model to generate images that have meaningful correspondence to the original input image, otherwise the model might just convert every image to some pixelated blob (the discriminator would recognize it as valid pixel art and wouldn't do anything about it).

You can download my pretrained model here -> https://www.dropbox.com/scl/fi/fwekfij597nbbalkncvuy/latest_net_G_A.pth?rlkey=grdhdtv9dut8fhzri3c95fj1h&st=n3o6o807&dl=0

I am also posting the code I used for inference in another repo. I anticipate my next blog post will be this coming Sunday (or a few days past depending on results). I want to try exploring some of Sutton's new RL papers, and maybe try writing some implementations/conversions to deep rl. Moreover, I would like to do a post about what was originally going to be the topic of this post; giving a vision-langauge model access to a video/image generator model. I am pretty sure the results will be nonsense, but I'm also curious to see how it will help answer spatial questions, visualizations, and other related problems.  
