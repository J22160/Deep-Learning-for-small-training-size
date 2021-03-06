# Comparison of CNN and Pre Trained Model.

It is well known that deep learning is data hungry and requires a lot of training data to generalise well. In this project, we will compare 2 types of deep learning models and see which one performs better when training size is very small.

## Data

The data consists of MRI scans of the brain. The task is to identify whether the MRI has brain tumour or not. It is a classification problem which is solved using deep learning. The images can be downloaded from [here](https://www.kaggle.com/navoneel/brain-mri-images-for-brain-tumor-detection). We have a total of only 260 images of which 155 images have brain tumor, named as 'YES', and 105 images do not have brain tumor which are named as 'NO'. 

![](https://user-images.githubusercontent.com/47391270/68539273-f4847f80-03a6-11ea-8f19-a806a8c2df7d.png 'YES') ![](https://user-images.githubusercontent.com/47391270/68539274-f8180680-03a6-11ea-9e5a-9a99c165e68d.png 'NO')

## Splitting the data

The data was spilt into Training and Validation Set. The training set has 220 images and validation set has 40 images. The ratio of 'YES' and 'NO' images is almost equal in both the sets (1.472 in training and 1.5 in validation).

## Models used

**1. CNN Model**

It is a sequential model having 2 Convolutional Blocks and a Fully Connected Layer.

![Screenshot from 2019-11-12 10-53-54](https://user-images.githubusercontent.com/47391270/68644412-ed8e7600-053a-11ea-9e92-db10ed2ddfaa.png)

Stochastic Gradient Descent with momentum is used as the optimizer with a very small learning rate and the model is trained for 50 epochs so as to avoid overfitting.

**2. Pre Trained Model**

I use VGG16 as the pre-trained model with weights trained on the ImageNet dataset. The fully connected layers at the top of VGG16 are omitted. Custom Fully connected layers are added after the convolutional layers of the base model. 

![vgg16](https://user-images.githubusercontent.com/47391270/68466520-95096100-023a-11ea-92af-408cc2bd4e85.png)

The training is done in 2 steps. In **Step 1**, all the convolutional layers of VGG16 are freezed and only the custom fully connected layers at the top are trained. A small learning rate is used to avoid making drastic updates to the weights. The model is trained for 25 epochs with a SGD optimizer with momentum.

![Screenshot from 2019-11-10 09-54-47](https://user-images.githubusercontent.com/47391270/68538816-63aaa580-03a0-11ea-9825-d5b1aac7165c.png)

In **Step 2**, all the convolutional layers except the last convolutional block of VGG16 are freezed. The last conv layer and the fully connected layers are trained with the same optimizer as in step 1. The model is again trained for 25 epochs. So the total number of epochs for the transfer learning model is 50 which is equal to the number of epochs in CNN model.

##### Why a 2-step process?
This is because our training images are very different from the ImageNet dataset on which VGG16 is trained. To make sure the our model learns the features present in the training images, the last convolutional block is unfreezed in step 2. Further this 2-step process gives a better accuracy than training step 1 for 50 epochs.

## Result
The code for training both the models is given in `Brain Tumor Detection.ipynb`. I ran the code for 10 different sets of validation data. All the sets were different from each other and union of all the validation sets gives the entire training set of 260 images. 

![Screenshot (10)](https://user-images.githubusercontent.com/47391270/68562835-5f01f200-0471-11ea-8045-85fe042a5844.png)

Accuracy and loss graphs for the models are present in `graphs.odt`.

![](https://user-images.githubusercontent.com/47391270/68563324-2fec8000-0473-11ea-8210-010eec426510.png)

## Conclusion
Clearly the Pre Trained Model outperforms the CNN Model in 9 out of the 10 sets and also has a higher mean accuracy. Fine tuning the pre trained model by using a 2 step process also contributes in a higher validation accuracy.

## References
- https://www.pyimagesearch.com/2019/06/03/fine-tuning-with-keras-and-deep-learning/
- https://www.kaggle.com/navoneel/brain-mri-images-for-brain-tumor-detection
