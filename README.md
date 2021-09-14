# Cycle-GAN
CycleGAN uses a cycle consistency loss to enable training without the need for paired data. In other words, it can translate from one domain to another without a one-to-one mapping between the source and target domain.  This opens up the possibility to do a lot of interesting tasks like photo-enhancement, image colorization, style transfer, etc. All you need is the source and the target dataset (which is simply a directory of images).


# How does it work?

A Tale of Two Generators
CycleGAN is a Generative Adversarial Network (GAN) that uses two generators and two discriminators. (Note: If you are not familiar with GANs, you may want to read up about them before continuing).
We call one generator G, and have it convert images from the X domain to the Y domain. The other generator is called F, and converts images from Y to X.

Both G and F are generators that take an image from one domain and translate it to another. G maps from X to Y, whereas F goes in the opposite direction, mapping Y to X.
Each generator has a corresponding discriminator, which attempts to tell apart its synthesized images from real ones.

One discriminator provides adversarial training for G, and the other does the same for F.

# The Objective Function

There are two components to the CycleGAN objective function, an adversarial loss and a cycle consistency loss. Both are essential to getting good results.

If you are familiar with GANs, the adversarial loss should come as no surprise. Both generators are attempting to “fool” their corresponding discriminator into being less able to distinguish their generated images from the real versions. We use the least squares loss (found by Mao et al to be more effective than the typical log likelihood loss) to capture this.

# Generator Architecture

Each CycleGAN generator has three sections: an encoder, a transformer, and a decoder. The input image is fed directly into the encoder, which shrinks the representation size while increasing the number of channels. The encoder is composed of three convolution layers. The resulting activation is then passed to the transformer, a series of six residual blocks. It is then expanded again by the decoder, which uses two transpose convolutions to enlarge the representation size, and one output layer to produce the final image in RGB.


# Discriminator Architecture

The discriminators are PatchGANs, fully convolutional neural networks that look at a “patch” of the input image, and output the probability of the patch being “real”. This is both more computationally efficient than trying to look at the entire input image, and is also more effective — it allows the discriminator to focus on more surface-level features, like texture, which is often the sort of thing being changed in an image translation task.

# Strengths and Limitations

Overall, the results produced by CycleGAN are very good — image quality approaches that of paired image-to-image translation on many tasks. This is impressive, because paired translation tasks are a form of fully supervised learning, and this is not. When the CycleGAN paper came out, it handily surpassed other unsupervised image translation techniques available at the time. In “real vs fake” experiments, humans were unable to distinguish the synthesized image from the real one about 25% of the time.
