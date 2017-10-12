# **Finding Lane Lines on the Road** 

## Writeup
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

My pipeline consisted of 5 steps :  

- Put an iterator for each sample image
- For each image, I first converted it to grayscale
- Performed gaussian blur
- Did Canny Edge detection
- Assign values to vertices for Region of interest
- Apply Hough Transformation on the Region of interest
- Tweaked the values of kernel size, rho, theta, threshold, min_line_len, max_line_gap (Resultant lines were continous and long)
- Got an image with lanes drawn on it after calling Hough Transformation

	-  First with normal draw lines
	-  Then with improved draw lines (draw one single thick line from bottom till center after avg/ extrapolating to detect lane) 
		- First segragate left lines and right lines (according to slope)
		- Used numpy.polyplot to find value which returns slope and intercept after fitting multiple lines
		- Tried two other methods to achive avg/ extrapolation (explained in improvements)

- Merged that image with my original image to get final output
- Did the same with video (multiple images)
- Stored all the outputs in corresponding output folders

ALSO, implemented two

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Following are the shortcomings :  
- When lanes are curved, there is problem in detection
- If noisy data is present in the region of interest, final lane lines are changed
- All this is dependent on region of interest; slight changed would need new adjustments, dependent on the positioning of camera on the car


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to detect outliers in the segregated left lane lines and right lane lines, remove them and then average the lane lines and draw a final single lane line.

Worked on this - 

1. SOLUTION 1 : Used mean and standard deviation on array of segregated slopes to remove outliers and then find the mean after cleaning array of slopes. Stuck at how to find the corresponding value of intercept; in order to use both slope and intercept in calculating x1 and x1 for drawing a line line when we know y1 = bottom most point and y2 = height of centre of image (or region of interest height)

2. SOLUTION 2 : Find the upmost and bottom most x values (we already know the y values ) and draw a line from bottom to up / OR / from bottom to centre of region of interest
   (Working fine, but problem with example.mp4)
