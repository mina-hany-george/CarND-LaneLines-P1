# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:

1 - Read the image pixels.

![alt_text][/writeup_images/solidWhiteRight.jpg]

2 - Convert the read image from RGB image to grayscale using cv2.cvtColor

![alt_text][/writeup_images/solidWhiteCurve_modified.jpg]

3 - Apply Canny edge detection with kernel (5x5) to get the gradient image.

![alt_text][/writeup_images/solidWhiteCurve_modified.jpg]

4 - Create a mask image with a polygon of 4 vertices to zero out region out of interest so that only the region of interest appears in the masked image

![alt_text][/writeup_images/solidWhiteCurve_modified.jpg]

5 - Apply Hough transform on the masked image to find the lines detected in the image and draw those lines on the image.

![alt_text][/writeup_images/solidWhiteCurve_modified.jpg]

6 - Combine the lines drawn on top of the input (RGB) image using cv2.addWeighted to get the final image.

![alt_text][/writeup_images/solidWhiteCurve_modified.jpg]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function and extrapolate the line by calculating the slope and intercept of the lines obtained from hough transformation.


### 2. Identify potential shortcomings with your current pipeline

In a single frame of the video, there may be no frames that qualify to be a detected line so there is no lines to draw on the image.

### 3. Suggest possible improvements to your pipeline

In the above explained issue, If this frame happened in the middle of the video, I keep the previous slope and intercept and assume the line will be of the same slope and intercept, which is really logical as the probability of lane suddenly changing the slope in few frames is very low.

But if this line was at the beginning of the video, I do not draw any lines and wait until we have an average "trusted" slope and intercept for the data we captured from hough transformation of previous frames in the video.
