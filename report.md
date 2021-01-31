# **Finding Lane Lines on the Road**

Identifying lanes on the road is a common task performed by all human drivers to ensure their vehicles are within lane constraints when driving, so as to make sure traffic is smooth and minimize chances of collisions with other cars due to lane misalignment.

Similarly, it is a critical task for an autonomous vehicle to perform. It turns out that recognizing lane markings on roads is possible using well known computer vision techniques. Some of those techniques will be covered below.

The goal of this project, the first Term 1 of the [Udacity Self Driving Car Engineer Nanodegree](https://www.udacity.com/course/self-driving-car-engineer-nanodegree--nd013), is to create a pipeline that finds lane lines on the road.

[//]: # (Image References)

[image1]: ./output_image/gray.png "Grayscale"
[image2]: ./output_image/blur.png "Blur"
[image3]: ./output_image/canny.png "Canny"
[image4]: ./output_image/roi.png "Roi"
[image5]: ./output_image/hough_1.png "Hough"
[image6]: ./output_image/final_result.png "Final"
[image7]: ./test_images/solidWhiteRight.jpg "origin"
[image8]: ./output_image/hough.png "Hough_I"



# Setup

Udacity provided sample images of solidWhiteRight.jpg. Below is the provided images.

![alt text][image7]


# The Pipeline

In this part, we will cover in detail the different steps needed to create our pipeline, which will enable us to identify and classify lane lines. The pipeline itself will look as follows:
* Convert image to grayscale for easier manipulation
* Apply Gaussian Blur to smoothen edges
* Apply Canny Edge Detection on smoothed gray image
* Trace Region Of Interest and discard all other lines identified by our previous step that are outside this region
* Perform a Hough Transform to find lanes within our region of interest and trace them in red
* Separate left and right lanes
* Interpolate line slope to create two continue left and right lines with improve draw_lines() function

The input to each step is the output of the previous step (e.g. we apply Hough Transform to region segmented image).



## Convert To Grayscale

We are interested in detecting white or yellow lines on images, which show a particularly high contrast when the image is in grayscale. Remember that the road is black, so anything that is much brighter on the road will come out with a high contrast in a grayscale image.

The conversion from RGB to a different space helps in reducing noise from the original three color channels. This is a necessary pre-processing steps before we can run more powerful algorithms to isolate lines.

![alt text][image1]

## Gaussian Blur

[Gaussian blur](https://en.wikipedia.org/wiki/Gaussian_blur) (also referred to as Gaussian smoothing) is a pre-processing technique used to smoothen the edges of an image to reduce noise. We counter-intuitively take this step to reduce the number of lines we detect, as we only want to focus on the most significant lines (the lane ones), not those on every object. We must be careful as to not blur the images too much otherwise it will become hard to make up a line.

The OpenCV implementation of Gaussian Blur takes a integer kernel parameter which indicates the intensity of the smoothing. For our task we choose a value of _3_.

The images below show what a typical Gaussian blur does to an image.

![alt text][image2]


## Canny Edge Detection

Now that we have sufficiently pre-processed the image, we can apply a [Canny Edge Detector](https://en.wikipedia.org/wiki/Canny_edge_detector), whose role it is to identify lines in an image and discard all other data. The resulting image ends up being _wiry_, which enables us to focus on lane detection even more, since we are concerned with lines.

The OpenCV implementation requires passing in two parameters in addition to our blurred image, a low and high threshold which determine whether to include a given edge or not. A threshold captures the intensity of change of a given point (you can think of it as a gradient). Any point beyond the high threshold will be included in our resulting image, while points between the threshold values will only be included if they are next to edges beyond our high threshold. Edges that are below our low threshold are discarded. Recommended low:high threshold ratios are 1:3 or 1:2. We use values _50_ and _100_ respectively for low and high thresholds.

I show the canny images below:

![alt text][image3]

## Region Of Interest

Our next step is to determine a region of interest and discard any lines outside of this polygon. One crucial assumption in this task is that the camera remains in the sample place across all these image, and lanes are flat, therefore we can identify the critical region we are interested in.

Looking at the above images, we "guess" what that region may be by following the contours of the lanes the car is in and define a polygon which will act as our region of interest below.

I put the segmented images below:
![alt text][image4]


## Hough Transform

The next step is to apply the [Hough Transform](https://en.wikipedia.org/wiki/Hough_transform) technique to extract lines and color them. The goal of Hough Transform is to find lines by identifiying all points that lie on them. This is done by converting our current system denoted by axis (x,y) to a _parametric_ one where axes are (m, b). In this plane:
 * lines are represented as points
 * points are presented as lines (since they can be on many lines in traditional coordinate system)
 * intersecting lines means the same point is on multiple lines

Therefore, in such plane, we can more easily identify lines that go via the same point. We however need to move from the current system to a _Hough Space_ which uses _polar coordinates_ one as our original expression is not differentiable when m=0 (i.e. vertical lines). In polar coordinates, a given line will now be expressed as (ρ, θ), where line L is reachable by going a distance ρ at angle θ from the origin, thus meeting the perpendicular L; that is ρ = x cos θ + y sin θ.
All straight lines going through a given point will correspond to a sinusoidal curve in the (ρ, θ) plane. Therefore, a set of points on the same straight line in Cartesian space will yield sinusoids that cross at the point (ρ, θ). This naturally means that the problem of detecting points on a line in cartesian space is reduced to finding intersecting sinusoids in Hough space.

More information about the implementation of Hough Transform in OpenCV can be found [here](http://docs.opencv.org/trunk/d6/d10/tutorial_py_houghlines.html)

The Hough transform returns lines, and the below images show what they look like:

![alt text][image5]

## Improve the draw_lines() function

In this stage I will modify draw_lines() function to create continue line on the left and right lanes, I modified the draw_lines() function by create list that collect m and b of left line and right line. next I calculate slope and intersect and define value of end point from roi both top and bottom next calculate mean value of all m and b and make condition to draw left and right line from end point.

The images below show final result after modify draw_lines() function.

![alt text][image8]



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
