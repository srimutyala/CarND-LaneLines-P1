# **Finding Lane Lines on the Road** 

---


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[c1-RGB]: ./examples/challenge-1.jpeg "c1-HLS"
[c1-HLS]: ./examples/challenge-1HLS.jpeg "c1-HLS"
[c1-HSV]: ./examples/challenge-1inHSV.jpeg "c1-HSV"
[c1-YUV]: ./examples/challenge-1inYUV.jpeg "c1-YUV"
[c2-RBG]: ./examples/challenge-1inHLS.jpeg "c2-RGB"
[c2-HLS]: ./examples/challenge-2inHLS.jpeg "c2-HLS"
[c2-HSV]: ./examples/challenge-1inHSV.jpeg "c2-HSV"
[c2-YUV]: ./examples/challenge-1inYUV.jpeg "c2-YUV"

---

### Introduction

In this project, I used Python and OpenCV to track lane markings of three different roads. It consists of three imprtant sections which are described below.

### Sections

1. Image transformation

This is probably the most important section of this project as this is where we setup a few transofrmations that will determine how robust the lane tracking shall be. Let me explain.

A common practice is to take the RGB image/video frame and convert that into a grayscale image to apply certain transformations. That's the path I chose too before encountering an issue with one of the road conditions under which it proved challenging to track the lanes. A section of the road was white and that threw the algorithm off which is doing the tracking based on color information. I tried changing the thresholds but good tracking was not achivied on that section of the road. However, the RGB color space is not the only space that's avilable for us in computer vision. These may not be necessary all the time but can transform the image enough to extract a particular fetaure set. Let's see if those transofrmations helps us for this road condition.

![alt text][c1-RGB]![alt text][c2-RGB]
![alt text][c1-YUV]![alt text][c2-YUV]
![alt text][c1-HLS]![alt text][c2-HLS]
![alt text][c1-HSV]![alt text][c2-HSV]




### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline is made up of 3 main sections. The first section is a conversion to the HSL  grayscale and an explicit call to a blurring function. The second section is involves a call to an edge detection function(canny in this case) to retrieve the edges of the image/frame. A fixed polygon(quad) is defined to mark the region of interest which I transform using Hough. As part of applying the Hough transform, I modified the 'draw_lines' helper function to accomodate the extrapolation of lane lines from individual segments to a single continbous line by first splitting the lines into left or right lane buckets & finding the average slope of each lines by wieghing the length of each segment.


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

A few shortcomings were encountered and some were fixed before the initial submission. Below is the list, their status & how I plan to overcome them in the near future.

1. A line was drawn from the max Y point on a lane to the min Y point of a lane. This resulted in a lane that did not track the lane properly.
  Status: Fixed. I changed my approach from the above to finding the weighted average slope. This provided better tracking of the lane & drastic changes of line drawing from frame to frame.
  
 2. The region of interest is defined with fixed values.
  Status: Fixed. I used a fixed region of interest for all 3 example videos. It worked fine for the first two and did not work with the challenge video. I changed my fixed region into a trapezoid with a generous width on the x-axis based off a fixed percent of the image size. This should now work well with inputs of different resolutions.
  
  3. Occasional incorrect lane tracking.
  Status: Pending. There are a couple of spots(a frame or a few) in the 'SolidYellowLeft' file where my pipeline draws lines that are a little off of the actual lane markings. This is due to a horizontal white line on the yellow lane marker at 0:11 sec on the video. The fix is to look at the slopes on all the lines on each side and remove the outliers.
  
  4. The Challenge.
  Status. Pending. My current pipeline does not work well for this. Part of it was because of the fixed definition of my region of interest ignoring certain parts of the lane. Fixing item #2 on this list helped greatly towards solving this. However, there is a section of the road where the lines go haywire though they return to normal after passing that section.But, unfortunately, we would have had a catastrophic crash by then. I think I understand where ths pipeline is being thrown off and I will look into addressing that.


### 3. Suggest possible improvements to your pipeline

The improvements have been included in the shortcomings section.
