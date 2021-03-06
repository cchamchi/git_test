

# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.png "Visualization"
[image2]: ./examples/germansign.png "germansign"
[image3]: ./examples/top5.png "Top5"
[image4]: ./examples/conv1.png "conv1"
[image5]: ./examples/conv2.png "conv2"
## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32,32,3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing the data distribution based on different signs.

![alt text][image1]

The numbers are not same for each sign. The smallest amount is 180, however, the largest is around 2000. This will be a factor resulting in different accuracy for each sign.



### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because grayscale image may performed better than 3-channel RGB image. However, I commented out my grayscale because it does not increase validation accuracy a lot, Instead, I performed only normalization and used 3-channel RGB image.



#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

The LeNet Convolutional network was adapted to the traffic sign recognition problem by changing the output classes to 43 and adding dropout layers in the fully connected layers in order to prevent overfitting. The input image depth was 3(RGB data were used. Becasue input depth was 3times, Layer 1 convolution filter was increased upto 12. The resulting model architecture is the following:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x12 	|
| RELU					|	Activation function							|
| Max pooling	      	| 2x2 stride,  outputs 14x14x12				    |
| Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x16	|
| RELU					|	Activation function                        | 
|Max pooling|2x2 stride,  outputs 5x5x16	|     									
|Flatten|Input 5x5x16, output 400|
| Fully connected		| Input 400, output 120 |
| RELU					|	Activation function                        | 
|Dropout|Dropout function |
|Fully connected|Input 120, output 84|
| RELU					|	Activation function                        | 
|Dropout|Dropout function |
|Fully connected|Input 84, output 43|
| Softmax				| Final Activation Function      									|

 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used the Adam Optimizer. Learning rate is 0.001, batch size is 128, number of epochs is 30, Dropout is 0.5 for full connection layers.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.998
* validation set accuracy of 0.961
* test set accuracy of 0.951

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
* What were some problems with the initial architecture?
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

I used the LeNet architect at first without dropout and grayscale. But the validation accuracy is not good. It was around 0.85. So, I changed preprocessing with commenting out grayscale and used 3channel RGB data. The accuracy was increased to 0.92, but fluctuated. I adapted dropout with keep_prob=0.5. Dropout actually prevented  overfitting, it randomly drops the unit in my fully connection layers. This gives me a much better accurracy, that is ~0.96. The reason why keep_prob = 0.5 is that half units drops yield better validation accuracy. With keep_prob = 0.4, the accurary increased slowly when epoches interates, it need more epoches. With keep_prob = 0.6, the accurary increased faster when epoches interates, but finally accuracy is same as keep_prob = 0.5. So keep_prob = 0.5 is chosen for the final value.

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on the web:

![alt text][image2]


#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| speed limit(20km/h)      		| speed limit(20km/h)  | 
| speed limit(20km/h)      			| speed limit(20km/h)  |
| speed limit(50km/h) 					| speed limit(50km/h) |
| No passing	      		| No passing|
| No entry		| No entry    |
| Road work |Road work |
| Pedestrians | Pedestrians|
| keep right | keep right|




The model was able to correctly guess 8 of the 8 traffic signs, which gives an accuracy of 100%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

 The top eight soft max probabilities were
![alt text][image3]


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?
This is the " NO passing" sign visualization. For conv layer 1, all the line were shown clearly. 
![alt text][image4]

For conv layer 2, details can be seen. some details are very important and interesting, e.g.feature map10,11. I am guessing it means the two cars of no passing sign

![alt text][image5]
