# Vehicle Detection Project

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/01-view_image.png
[image2]: ./output_images/02-hog.png
[image3]: ./output_images/03-predict.png
[image4]: ./output_images/04-heatmap.png
[image5]: ./output_images/05-heatmap.png
[image6]: ./output_images/06-test_pipeline.png
[video1]: ./output_videos/project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  
### Please see the code of this project in `main.ipynb` 
---

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:  

![alt text][image1]

I then define a function to return HOG features and visualize HOG on example image.  

![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

To choose the best HOG parameters (also spatial, histogram parameters, etc), I defined a class `FeatureParameters` to hold all the parameters. There are lots of combinations that I did not cover all, I just tried several combinations manually and compare the model accuracy and fitting time.  
The final parameters I used are the following:   
* self.cspace = 'HLS'
* self.orient = 9
* self.pix_per_cell = 8
* self.cell_per_block = 2
* self.hog_channel = 'ALL'
* self.spatial_size = (16, 16)
* self.hist_bins = 32
* self.hist_range = (0, 256) 

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using the parameters above and finally got a 99.27 accuracy result. the model has been dumped with name `model.p`

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I firstly defined a `search_windows` function to pass and image and return the windows for car detections. then I loaded a test image and applied the classifier to the image and drew boxes. The scales and overlap parameters are chose by experimenting and comparing the final boxes. 

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

The following image is a result of this experimentation on a test image:  
![alt text][image3]

To optimize the performance, a heat map has been introduced here with a threshold, and then a HOG sub-sampling was implemented in `find_cars` function. The image below shows the results on the test images:  
![alt text][image5]

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output_videos/project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle. I constructed bounding boxes to cover the area of each blob detected.


I created a pipeline in `process_image` function to cover the whole image processing above, and then optimize it a little bit in `process_image_final` function to make the boxes more 'smooth' when display. 


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

There are several points which I would like to Improve/Try if time allows: 
1. When training the model, more combinations should be tried and choose the best one. 
2. Combine the pipeline together with previous 'Advanced lane finding'
3. Try other object detection approach like YOLO, integrate it into this project and compare with current solution.


