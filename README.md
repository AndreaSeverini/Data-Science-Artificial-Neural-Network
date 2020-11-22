# **ARTIFICIAL NEURAL NETWORKS AND DEEP LEARNING**
## **Homework 1**

Classify images depicting groups of people based on the *number of masked people*. 

In the specific, the solution must discriminate between images depending on the following cases: 
  - 1) *All the people in the image are wearing a mask* (**CLASS 2**) 
  - 2) *No person in the image is wearing a mask* (**CLASS 0**)
  - 3) *Someone in the image is not wearing a mask*  (**CLASS 1**)

## **Workflow Explanation**

### **1 - Managing Dataset**
Analysing the dataset structure (MaskDataset) we have two folder, one for training and one for test and a JSON file in which we have the classes regarding only the training part.
For this reason, i decided to use the test part just for the CSV creation that has to be uploaded in Kaggle for this private Univercity competition.
Afterall we cannot use test images for training and usually is not provided.

I decided to use **ImageDataGenerator** to create a generator and a dataset object in order to use *flow_from_directory* method.
This require a directory that is just pre-organized in subfolders, one for each class and in which we obviously have the right images for the right class.

This required managing the MaskDataset provided and for this reason i created a custom code to create the folders and to upload images by class from JSON file provided.

ImageDataGenerator allows us to split directly training part to **training** and **validation** (**10%** in this case) and also to apply **Data Augmentation**.

### **2 - Model Creation Steps**

- In the first steps i created a classical CNN structure with some layers of Convolution+MaxPooling+ActivationFunction applyed in a iterarive way. Then i Flattened the output of my deep-network structure (features extractor) in order to apply some Dense layers up to the last one that have 3 neurons for the classification purpose.
Managing the classification part i undestand that too much dense layers with too much neurons was not the right solution.
Indeedt the classification is quite simple with only 3 classes and furthermore i had to deal with more parameters to be trained and an easy overfit with too small data. Clearly see overfit after some epochs.

- I added **eraly stopping** in *callbacks* to manage my training and to stop before overfit.
Managing with Convolutiona(Activation+Maxpool) i found that a good trade-off was about 5-6 repetition of these plus a Dense layers of 16(or 32) neurons plus the one for the classification with the softmax activation function.
I tryed to change the activation functions (Relu, Sigmoid and Tanh) with not a great interesting variations. Same between Adam or SGD as optimizer. Choosing Adam because of adjusting learning rate. Tryied to change Filter(Kernel) size from (3,3) to (5,5) but finally i decided for the first one. Choosing depth of Convolution about 10. 
I managed a validation accuracy in the neighborhood of **60%** and a loss of 0.7/0.8.

- Try to increase my accuracy reducing overfitting adding some **Data Augmentation** and reaching about **65%**, then try increasing Dense layers but with no much gain. Adding other Conv block was too much time demanding.

- Tryed to use **Transfer Learning** so that i removed the TOP from VGG16 freezing all weights and adding my Dense layers with my parameter tuned.
Reacing **70%** accuracy.

- Tryed to use **Fine Tuning** from the last layers of the VGG16 (*block 5 last conv*) plus DataAugmentation deals with a gain of **5%**

- Finally i tryed to start **Fine Tuning** from the *block 4* adding a more aggressive **Data Augmentation**, some **regularization** adding **Dropout** layers in between Dense Layers and **Weight Decay** to constraint the training to maintain a low weights value. In this case i understand than a very low learning rate (*1e-4*) was required to maintain the weights from VGG16 and not update them to quickly.
This final method allows me to use pre-trained model using low features extractor freezed and updating only high level features. This allows to reach a level about **87.7%** of accuracy. A clearly good improvement.

### **3 - Final Conclusion**

**Fine Tuning** and **Transfer Learning** is a good way to gain good accuracy without too much computational power and time consumption.
I can try to use different pre-trained model or with a more accurate Hyperparameter tuning and a model redefinition but i run out of time.
I really appreciated this competition and i think that it allows me to really undestrand some concepts we discussed during lessons.
Looking forward to the next Homework Competition.
