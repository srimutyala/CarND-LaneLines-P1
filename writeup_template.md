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

My pipeline is made up of 3 main sections. The first section is a simple conversion to grayscale and an explicit call to a blurring function. The second section is involves a call to an edge detection function(canny in this case) to retrieve the edges of the image/frame. A fixed polygon(quad) is defined to mark the region of interest which I transform using Hough. As part of applying the Hough transform, I modified the 'draw_lines' helper function to accomodate the extrapolation of lane lines from individual segments to a single continbous line by first splitting the lines into left or right lane buckets & finding the average slope of each lines by wieghing the length of each segment.


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

A few shortcomings were encountered and some were fixed before the initial submission. Below is the list, their status & how I plan to overcome them in the near future.

1. A line was drawn from the max Y point on a lane to the min Y point of a lane. This resulted in a lane that did not track the lane properly.
  Status: Fixed. I changed my approach from the above to finding the weighted average slope. This provided better tracking of the lane & drastic changes of line drawing from frame to frame.
  
 2. The region of interest is defined with fixed values.
  Status: Pending. I will change this to a trapezoid with a generous  width on the x-axis based off a fixed percent of the image size. That should scale well for images of different resolutions.
  
  3. Lane line intersect.
  Status: Pending. there is one spot(a frame or a few) in the 'SolidYellowLeft' file as the road curves where my pieline draws the lines such that they intersect.
  Status: Pending. I could draw shorter lines such that the lines does not extend and intersect. But, that's not necessarily fixinf the problem rather than masking it. I believe this has to do with the parameters for the Hough transform which I haven't completely understood yet. I am going to read about the Hough transform and figure out a way to avoid that intersection.
  
  4. The Challenge.
  Status. Pending. My current pipeline does not work for this. Part of it is because of the fixed definition of my region of interest ignoring certain parts of the lane. Fixing item #2 on this list should help towards solving this. I am thinking of using a curve rather than 
One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
