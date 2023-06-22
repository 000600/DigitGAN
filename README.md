# DigitGAN
## The Model
This GAN generates images of handwritten numbers (numbers zero through nine) based on an MNIST dataset containing images of numberes written by people. The model contains two neural networks: the generator and the discriminator. 

The generator creates fake images of handwritten numbers by taking an input of a point in latent space (in this case a vector of 100 elements consisting of Gaussian random numbers) and outputting a 28 x 28 grayscale image. The generator uses standard LeakyReLU activation functions and an Adam optimizer with a learning rate of 0.0002 and a momentum of 0.5. The network has an architecture consisting of:

The discriminator is a convolutional neural network that classifies whether or not an image is "real" (created by a person) or "fake" (created by a computer). The discriminator is trained on the MNIST dataset and images created by the generator. It predicts a value close to 0 if an image is thought to be "fake" and a 1 if the image is considered to be "real." Since network predicts binary categorical values (because it is classifying an image as either "real" or "fake"), it uses a binary crossentropy loss function and has 1 output neuron. The discriminator has an Adam optimizer with a standard learning rate of 0.0002 and a momentum of 0.5 It has an architecture consisting of:

