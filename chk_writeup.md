# Advanced Lane Finding

## Chundi Himakiran Kumar

---

***In this project we have attempted advanced lane finding that involves finding lanes in real world conditions such as
shadows , camera distortion and steep curves and changing road conditions. Apart from the usual libaries we have in particular tried to make us of SCIPY's savitzky-golay-filter and have also implemented code on the lines of the LANE class for ensuring speedier processing.***



---

<video width="320" height="240" controls>
  <source src="project_video_output.mp4" type="video/mp4">
</video> 


---

**Advanced Lane Finding Project**

***The goals / steps of this project are the following:***

1.Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
2.Apply a distortion correction to raw images.
3.Use color transforms, gradients, etc., to create a thresholded binary image.
4.Apply a perspective transform to rectify binary image ("birds-eye view").
5.Detect lane pixels and fit to find the lane boundary.
6.Determine the curvature of the lane and vehicle position with respect to center.
7.Warp the detected lane boundaries back onto the original image.
8.Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.
9.Finally to apply the pipeline on a video and output a video with lanes marked.

[//]: # (Image References)

[image1]: ./output_images/camera_cal/calibration2.jpg_CCoutput "Calibration"
[image2]: ./output_images/undistorted/test4.jpg "Undistorted"
[image3]: ./output_images/perspective_transformed_images/test3.jpg "Perspective transformed"
[image4]: ./output_images/threshold_output/straight_lines1_HSV_V.jpg "Threshold output"
[image5]: ./output_images/pipeline_output/test4_final_output.jpg "Pipeline Output"
[image6]: ./output_images/lane_detected/test2_lane_drawn.jpg "Lane detection output"
[image7]: ./output_images/final_output/test2.jpg "Final image output"
[video1]: ./project_video_output.mp4 "Final Video Output"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. This file is the writeup / README which includes statements and supporting figures / images that explain how each rubric item was addressed, and specifically where in the code each step was handled.

### Camera Calibration

#### 2. The camera calibration was done using the provided calibration images. Post this step which yielded us the camera matrix and distortion coefficent, all the test_images were undistorted and verified.

The code for this step is contained in the first four code cells of the IPython notebook located in "./P2.ipynb" 
 We used the cv2.findChessboardCorners function on the gray output of the test calibration images. Then the function cv2.drawChessboardCorners was used to draw the corners on the original image. This step give us the camera matrix and distortion coeffcient whcih we shall now use on all images to undistort them.

The following is an example of a calibartion image with the corners marked in 
[image1]: ./output_images/camera_cal/calibration2.jpg_CCoutput "Calibration"
and this image is an example of an undistorted image.
[image2]: ./output_images/undistorted/test4.jpg "Undistorted"

### Perspective Transformation
#### 3.  Post the undistorting of the images we first used a function do_perspective_transform( ) which was supplied with a collection of source coordinates and destination coordinates that were manually selected to fit all the test images adequately. The idea was this area which were going to perspective transform should give us the maximum points of interest of the lanes. This was a highly manual task and alot of trial and error went into it.

The following is an example of an original image and the image when transformed perspectively.

[image]: ./test_images/test3.jpg "original test image"
[image3]: ./output_images/perspective_transformed_images/test3.jpg "Perspective transformed"



### Gradients and Color thresholding
#### 4.  This was an exhaustive task. We here attempted all the gradient techniques we were taught and some we googled on the internet. To make this a easier experience, we created a function to visualize images and used it to visualize the output of each of the thresholding techniques. The following internet link helped us in this process. We implemented a First stage pipeline that accepts and image and gives us the thresholded lanes on a black and white image which we shall use as the input for the next step of sliding window lane detection.

[I'm an inline-style link](https://towardsdatascience.com/a-very-simple-demo-of-interactive-controls-on-jupyter-notebook-4429cf46aabd)

The above link helped us much in the tuning process to find the parameters for the various thresholds. The complete list of thresholding techniques and their output is available in the P2.IPYNB notebook.

After visually assessing the output and reading through the below link

[I'm an inline-style link](https://towardsdatascience.com/teaching-cars-to-see-advanced-lane-detection-using-computer-vision-87a01de0424f)

we decided on using the below code for our final thresholding technique

`combined[(img_HLS_L == 1) | (img_LAB_B == 1) | (img_sobelAbsMag == 1)]=1 `

A sample output is shown below
[image4]: ./output_images/threshold_output/straight_lines1_HSV_V.jpg "Threshold output"

### Detecting Lanes using Sliding Windows technique

#### 5. We wrote a total of four functions to achieve this task. Functions fit_polynomial( ) which makes use of find_lane_pixels( ) forms part of the sliding windows code which evaluates the lane parameters from plotting the histogram onwards. The second set of functions  search_around_poly( ) and fit_poly make up the code to do the same task for subsequent frames by only searching in the area where the earlier lanes were found.

#### 6. An important thing we did was to implement the logic of the Lane class as explained in the course by coding the functionality straight in to the above four functions. So we ended up doing the same thing without using the Lane Class. Whenever a frame is sent a check is carried out if it is an intial frame( that means the left_fit and right_fit are empty arrays fed to fit_polynomial( ) function) and then it is sent to either of the two groups of code. This has been explained adequately with comments in the P2.ipynb notebook.

A sample output at the end of lane detection is given below
[image6]: ./output_images/lane_detected/test2_lane_drawn.jpg "Lane detection output"

#### 7.  We have also tried to use the SCIPY's SAVITZKY-GOLAY-FILTER to help correct noise in the points but it had not any perceivable effect and hence we commented it out. Like wise we tried to implement a logic where the cells would shift left or right by a hyperparameter _margin in the absence of data points. But again it did not work out. so we commented it out.


#### 8. The function to calculate the radius of curvature and the postion of the vehicle has also been coded into the above four functions.

#### 9. Finally the main pipeline function was coded , img_processing_pipeline_final( ) that would accept an image and would use all the above functions to output a acolor image with the lanes drawn and the lane area colored and the text printed on top showing the radius of curvature and distance from the center.

 An example output of the final image processing pipeline is given below
 [image7]: ./output_images/final_output/test2.jpg "Final image output"
 
#### 10. Finally we were able to apply the pipeline to the given video and got a reasonably good output video which we have linked it below

[video1]: ./project_video_output.mp4 "Final Video Output"


---

### Discussion

#### 1. The thresholding both color and gradient was challenging and time consuming. Tweaking the parameters to satisfy all the test images was challenging.
#### 2. The sliding window code has implemented a lot of functionality including the logic behind the Lane Class as explained above. It can be simplified and broken up.
#### 3.  It is felt that filters like SCIPY's SAVITZKY-GOLAY-FILTER with adequate tweaking and with inbuilt safeguards might get us a better lane detction.