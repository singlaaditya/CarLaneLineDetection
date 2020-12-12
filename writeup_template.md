# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**
Please find the code in WORKING.IPYNB
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./lane_detection.png "FINAL IMAGE"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps. Following is the sequence in which the pipeline functions:
1) Convert colored image to grayscale
2) Perform a gaussian blur on the grayscale with a kernel value of 3 
3) Perform a canny edge detection on gaussian blurred image and get a canny image. I used the low_threshold and high_threshold as 50 and 150 respectively.
4) Then I cut the canny image for a particular region of interest. Basically I blackened the rest of the part apart from my region of interest. The region of interest that I used is a quadrilateral ((int((x_dim)*5/11),int((y_dim)*3/5)),(int((x_dim)/6),int(y_dim) - 1),(int((x_dim)*7/8),int(y_dim) - 1),(int((x_dim)*6/11),int((y_dim)*3/5)))
Here x_dim and y_dim refers to the breadth and length of image.
5) Then I performed a hough transform on the previous image. Through this I got a list of lines. I used rho = 1, theta = 1 deg, threshold = 30, min_line_len = 1, max_line_gap = 10
6) Then I performed interpolation of these lines to get two lines corresponding to left lane line and right lane line. I used averaging of all the slopes and intercepts of the left lines and right lines to get two mean lines. The left lines were selected based on the fact that left lane lines have a negative slope and the right lines were selected based on the fact that the right lane lines have a positive slope. The averaging was done in a simple manner by taking a mean of the slopes and intercepts. Thereafter I used two points with y coordinates as y_dim * 6/10 and y_dim - 1. I used these two y coordinates to get corresponding x coordinates. 
7) Thereafter I created a blank numpy array similar to the original image and drew the two lines on this blank array.
8) Then I added the previous image and the original image in a weighted manner. 


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

My pipeline does not work on challenge.mp4.
Possible reason why my pipeline does not work on challenge.mp4 because the camera seems to be placed on dashboard and not in front of the car as observed in the other videos. I can see the bonnet of the car.


### 3. Suggest possible improvements to your pipeline

To make the pipeline work I should add a choice to the user that whether the camera is placed on the top of the dashboard or in front of the car. Based on this camera position variable I need to select the ROI.
