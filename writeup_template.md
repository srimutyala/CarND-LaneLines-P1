# **Finding Lane Lines on the Road** 

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
