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
[image2]: ./output_image/blur.png "Blur"
[image3]: ./output_image/canny.png "Canny"
[image4]: ./output_image/roi.png "Roi"
[image5]: ./output_image/hough.png "Hough"
[image6]: ./output_image/final_result.png "Final"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I use gaussian blur to smooth or decrease noise in image, Third, I use canny to edges detection on image, then create mask or roi(region of interest) in image, finally use hough transform to find line on image  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by create list that collect m and b of left line and right line. next I calculate slope and intersect and define value of end point from roi both top and bottom next calculate mean value of all m and b and make condition to draw left and right line from end point.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]

This is blur image.

![alt text][image2]

This is image after use canny.

![alt text][image3]

This is roi in image.

![alt text][image4]

This is line on image by hough transform.

![alt text][image5]

this is final result. 
![alt text][image6]  


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
