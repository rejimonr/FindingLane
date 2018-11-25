# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied Gaussian smoothing by using the opencv function GaussianBlur. (the key here is ofcourse the kernel size. I settled on 11 after some experiementation.) 
The next step was to identify the edges using Canny algorithm. I used a low and high threshold of 60 and 120 respectively. 
The next step in the pipeline was to isolate an region of interest. I defined a polygon starting from the bottom of the image extending to about the middle of the image. (Again had to experiment using the test images to come to an number).
Hough algorithm was then used to pick up the lines from this image. The parameters were again tuned based on the testing the test images. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to do the following things
1. As a first step all the lines identified were categorized as left or right using the slope value. (Negative slope indicates left lane and positive slope indicated right lane. Also added some checks to discard flat horizontal lines).
2. Next step was to identify the most fitting line. After some research settled on using scipy.stats.linregress method to give the most fitting line. (Started of initially with simple average for slope and intercept but this gave a better fit. Reference: https://medium.com/computer-car/udacity-self-driving-car-nanodegree-project-1-finding-lane-lines-9cd6a846c58c). 
3. Extrapolated the lines to cover the full region of interest area in the image.

As a last step, combined the original image and the image with lane lines using opencv addWeighted function. 


![Test Image][./test_images/solidWhiteCurve.jpg] ![Output Image][./test_images_output/Added_Lane_solidWhiteCurve.jpg]


### 2. Identify potential shortcomings with your current pipeline


One of the main shortcomings is handling of curves. Right now the impact is reduced due to very careful management of the region of interest. However this may not be sustainable as more images are tested. This was evident when the pipeline was run on the videos. 

Another issue seen is the need for more perfection in identifying dotted lanes. The impact is more in videos where again parameters had to be tuned heavily to make the output decent. 


### 3. Suggest possible improvements to your pipeline

One area to do further research is around how to average and extraploate the lines more perfectly (including considering the curves). Couple of areas to study more will be around how to use the previous frame image and future frame image data in identifying the lanes in the current frame image better. Should also look into seeing if another order polynomial may be a better fit than line.
