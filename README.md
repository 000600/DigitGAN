# DigitGAN
## The Model
This GAN generates images of handwritten numbers (numbers zero through nine) based on an MNIST dataset containing images of numbers written by people. The model is located in the **DigitGAN.py** file and contains two neural networks: the generator and the discriminator. 

The generator creates fake images of handwritten numbers by taking an input of a point in latent space (in this case a vector of 100 elements consisting of Gaussian random numbers) and outputting a 28 x 28 grayscale image. The generator uses standard Leaky ReLU activation functions and an Adam optimizer with a learning rate of 0.0002 and a momentum of 0.5. The network has an architecture consisting of:
- 1 Input layer (with 6272 neurons, an input dimension of 100, and a Leaky ReLU activation function)
- 1 Reshape layer (which reshapes the output into a 7 x 7 image)
- 2 Conv2D Transpose layers (each with a Leaky ReLU activation function)
    - These layers upscale the image outputted by the Reshape layer to 28 x 28 images
- 1 Output Conv2D layer (with a sigmoid activation function to normalize values)

The discriminator is a convolutional neural network that classifies whether or not an image is "real" (created by a person) or "fake" (created by a computer). The discriminator is trained on the MNIST dataset and images created by the generator. It predicts a value close to 0 if an image is thought to be "fake" and a 1 if the image is considered to be "real." Since the network predicts binary categorical values (because it is classifying an image as either "real" or "fake"), it uses a binary cross entropy loss function and has 1 output neuron. The discriminator has an Adam optimizer with a standard learning rate of 0.0002 and a momentum of 0.5 It has an architecture consisting of:
- 1 Input Conv2D layer (with an input shape of [28 x 28 x 1], 64 filters, and a Leaky ReLU activation function)
- 2 Dropout layers (one after each convolutional layer, each with a dropout rate of 0.4)
- 1 Conv2D layer (with 64 filters and a Leaky ReLU activation function)
- 1 Flatten Layer
- 1 Output layer (with 1 neuron and a sigmoid activation function for binary classification)

These models are combined to form the complete GAN, which has a standard Adam optimizer (with a learning rate of 0.0002 and a momentum of 0.5) and a binary cross entropy loss function. Within the larger model, the weights of the discriminator are static and are only updated when the discriminator itself (rather than the entire composite model) is trained. In total, the GAN has an architecture consisting of:
- The generator model
- The discriminator model

## Metrics
Since it is difficult to use a loss function to determine the quality of the images generated by the GAN, every five epochs of training the weights of the generator are saved in a .h5 file and the generator generates multiple images. These images can be graded by someone on how realistic they seem to determine the status of the training process; if the digits seem like they could be handwritten, the training process can be stopped. Conversely, if the digits don't resemble numbers, the training process should continue or be restarted for better results. In both instances, a file  with the weights of the generator is saved in a .h5 file with the name **epochX.h5** where **X** is the number of epochs that the model had trained for when the file was saved. The file saves in the same path the file was run from. 

In this repository, an example .h5 file is included called **epoch100.h5**. I found the generator could generate relatively human-like numbers in multiple instances with the weights stored in this file. All .h5 files that are saved during the training process (including the example provided in the repository) can be loaded into the model to generate images using the **display_images.py** file by changing the file name passed into the *load_model* function on line 56. When run, this file outputs three plots: the first contains 25 images generated by the GAN, the second contains one enlarged image generated by the GAN, and the last contains 25 images from the training set (for reference). Additionally, one can restart the training process using specific weights stored in a .h5 file using the **load_GAN.py** file by changing the file name passed into the *load_model* function on line 108.

In addition to the saving of weights and the display of the GAN's generations, every five epochs the discriminator's accuracy on fake and real images is displayed. If the discriminator's accuracy in classifying fake images is relatively high, the GAN's generations are easy to classify as "fake" and it is not generating realistic images — an indicator that training needs to continue.

## Files
There are four files included in this repository, each of which with separate functions. The purpose of the files has mostly been described above, but to clarify here are their uses:
- **DigitGAN.py** is the file for the complete GAN. Running this file will train the GAN from scratch, saving generator weights and displaying images created by the model every five epochs.
- **load_GAN.py** enables the user to start training the GAN with a specific set of weights, which can be loaded into the generator model on line 108.
- **display_images.py** simply allows the user to view images generated by the GAN by inputting the file with the weights of the generator on line 56. Running the file will result in the display of three plots: a plot of 25 images generated by the GAN, a plot of one enlarged image generated by the GAN, and a plot of 25 "real" images from the dataset (for reference)
- **epoch100.h5** is an example .h5 file that stores weights. When loaded into the **display_images.py** file, one can see the GAN's ability to generate images of handwritten digits after 100 epochs.

## The Dataset
The dataset is an MNIST dataset included within the Keras library. The dataset contains approximately 70,000 (60,000 images in the train set and 10,000 images in the test set) 28 x 28 pixel images of human handwriting of numbers 0 - 9 and is included within the **DigitGAN.py** and **load_GAN.py** files.

## Libraries
This GAN was created with the help of the Tensorflow library.
- Tensorflow's Website: https://www.tensorflow.org/
- Tensorflow Installation Instructions: https://www.tensorflow.org/install
