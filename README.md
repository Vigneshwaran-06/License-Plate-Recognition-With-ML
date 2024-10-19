# License-Plate-Recognition-With-ML
# install virtualenv if you don’t have the package already
pip install virtualenv
mkdir license-plate-recognition
cd license-plate-recognition
virtualenv lpr
source lpr/bin/activate

A folder with the name lpr should now be in your project directory.

Now, let’s install our first package scikit-image. It’s a Python package for image processing. To install it, run.

pip install scikit-image

Some key dependencies of the package are scipy (for some complex scientific calculations), numpy (for n-dimensional arrays manipulations) and matplotlib (for plotting graphs and displaying images). Another important package is Pillow — a python imaging library.

License Plate Detection (Plate localization)

This is the first stage and at the end of this stage, we should be able to identify the license plate’s position on the car. In order to do this, we need to read the image and convert it to grayscale. In a grayscale image, each pixel is between 0 & 255. We now need to convert it to a binary image in which a pixel is either complete black or white.
------------------------------------------------------------------------------------------------FIRSTSTAGE.PY----------------------------------------------------------------------------------------------------
We need to identify all the connected regions in the image, using the concept of connected component analysis (CCA). Other approaches like edge detection and morphological processing can also be explored. CCA basically helps us group and label connected regions on the foreground. A pixel is deemed to be connected to another if they both have the same value and are adjacent to each other.

------------------------------------------------------------------------------------------------SECONDSTAGE.PY-------------------------------------------------------------------------------------------------
We had to import the previous file so that we can access the values we have there. The measure.label method was used to map all the connected regions in the binary image and label them. Calling the regionprops method on the labelled image will return a list of all the regions as well as their properties like area, bounding box, label etc. We used the patches.Rectangle method to draw a rectangle over all the mapped regions.

From the resulting image, we can see that other regions that do not contain the license plate are also mapped. In order to eliminate these, we will use some characteristics of a typical license plate to remove them.

1)They are rectangular in shape.
2)The width is more than the height.
3)The proportion of the width of the license plate region to the full image ranges between 15% and 40%.
4)The proportion of the height of the license plate region to the full image is between 8% & 20%.
5)Don’t hesitate to tweak these characteristics 
if it doesn’t work for the shape of your license plate (the probability of that is low though).

------------------------------------------------------------------------------------------------THIRDSTAGE.PY-------------------------------------------------------------------------------------------------
The plate_like_objects is a list of all the regions on the car that look like a license plate. From the image I used, three regions were identified as candidates for a license plate. To save time, I hardcoded the index 2 because that’s the one with the license plate. The final code I’ll be sharing will contain a license plate validation technique to eradicate other regions that do not actually contain the license plate.

Then a CCA was done on the license plate and each character was resized to 20px by 20px. This was done because of the next stage that has to do with recognition of the character.

------------------------------------------------------------------------------------------------FOURTHSTAGE.PY-------------------------------------------------------------------------------------------------

In order to keep track of the order of the characters, the column_list variable was introduced to take note of the starting x-axis of each region. This can then be sorted to know the correct order of the characters.

Character Recognition

This is going to be the last stage, it’s at this stage we introduce the concept of machine learning. Machine learning can simply be defined as the branch of AI that deals with data and processes it to discover pattern that can be used for future predictions. The major categories of machine learning are supervised learning, unsupervised learning and reinforcement learning. Supervised learning makes use of a known dataset (called the training dataset) to make predictions. We’ll be taking the path of supervised learning because we already have an idea of how As, Bs and all the letters look like. Supervised learning can be divided into two categories; classification and regression. Character recognition belongs to the classification category.

All we need now is to get a training data set, choose a supervised learning classifier, train a model, test the model and see how accurate it is, then use the model for prediction.

Let’s start with training our model. I have two different datasets, one is 10px by 20px and the other is 20px by 20px. We’ll make use of the 20px by 20px because we’ve already resized each character to that size. Each letter except O and I (a typical Nigerian license plate doesn’t have these letters because of their resemblance with 0 and 1) has 10 different images.

There are several classifiers we can use with each of them having its advantages and disadvantages. We’ll use SVC (support vector classifiers) for this task. I chose to use SVC because it gave me the best performance. However, this does not necessarily mean that SVC is the best classifier.

We’ll have to install the scikit-learn package for this stage.

pip install scikit-learn
------------------------------------------------------------------------------------------------FIFTHSTAGE.PY-------------------------------------------------------------------------------------------------
In the gist above, each character in the training dataset was used to train the svc model. A 4-fold cross validation was also done to determine how accurate the model is and then the model was persisted to a file so that predictions can be done without training a model anymore.

Now that we have a trained model, we can attempt to predict the characters that we earlier segmented.

------------------------------------------------------------------------------------------------SIXTHSTAGE.PY-------------------------------------------------------------------------------------------------





