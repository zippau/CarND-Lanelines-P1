#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of multiple steps. Each step was to convert the image into something usable by the next function. In general, the pipeline takes in images, uses conversions to read the image, identify key features, estimates the lane lines and then returns image or video with those lane lines projected on it. More specifically the steps included,
1.	Importing the image
2.	Making the image grayscale, to enhance the contrast
3.	Undertaking Gaussian Bluring of the image, to smooth the image and reduce noice (i.e. helps with the next function)
4.	Uses a canny function, to identify edges
5.	Creating an image mask, so that we only focus on the area we're interested in
6.	Use the hough transformation, to turn edge points into estimated lines
7.	Averaging the lines and extrapolating them on the image
8.	Plotting the lines on the image and returning them in image or video format

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
-	For each line, estimated a slope
-	Moved the line into two arrays, one for positive slopes and one for negative slopes. This represents left and right lanes
-	Excluded lines with unlikely slopes (to better estimate the image)
-	Estimate average position of the line section
-	Calculate the line intercept
-	Plot the line using the average position and line intercept data
-	Looped this function for each image in the folder or video frames

This is how my pipeline works.

[image2]: ./initial.jpg "Initial Image"
[image3]: ./gray.jpg "Grayscale"
[image4]: ./gaussian.jpg "Gaussian Blur"
[image5]: ./canny.jpg "Canny"
[image6]: ./canny-masked.jpg "Canny - Masked"
[image7]: ./hough.jpg "Hough Transformation"
[image8]: ./Houghlines-Weighted.jpg "Final Weighted Image"


###2. Identify potential shortcomings with your current pipeline

At the moment there are a few shortcomings with the proposed pipeline, these include:
-	Canny function is set up to be sensitive to the test images, this would be effected by colour changes in the images (i.e. clouds, shade, time of day)
-	Image masking works for flat images, it would not work well if the car was going up or down hill - which will change image perspective
-	Hough lines works with this distance between dashed lane lines, however if this was different, it would not work.
-	The extrapolation function only works with straight lines, not with curves

###3. Suggest possible improvements to your pipeline

Possible improvement to the pipeline could include:
-	Additional function to change masked image to be a constant brightness (make dull images bright, or concreate roads darker)
-	Average line funciton should look for best fit, straight or curved lines
-	Could create mask that creates the 'top of the mask' when the two lane lines are a certain pixel distance apart, as opposed to a fixed mask shape
