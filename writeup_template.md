# **Finding Lane Lines on the Road** 

---


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[c1-RGB]: ./examples/challenge-1.JPG "c1-HLS"
[c1-HLS]: ./examples/challenge-1HLS.jpeg "c1-HLS"
[c1-HSV]: ./examples/challenge-1inHSV.jpeg "c1-HSV"
[c1-YUV]: ./examples/challenge-1inYUV.jpeg "c1-YUV"
[c2-RGB]: ./examples/challenge-2.JPG "c2-RGB"
[c2-HLS]: ./examples/challenge-2inHLS.jpeg "c2-HLS"
[c2-HSV]: ./examples/challenge-2inHSV.jpeg "c2-HSV"
[c2-YUV]: ./examples/challenge-2inYUV.jpeg "c2-YUV"

---

### Introduction

In this project, I used Python and OpenCV to track lane markings of three different roads. It consists of three imprtant sections which are described below.

### Sections

### 1. Image transformation

This is probably the most important section of this project as this is where we setup a few transofrmations that will determine how robust the lane tracking shall be. Allow me to explain.

A common practice is to take the RGB image/video frame and convert it into a grayscale image to apply certain transformations. That's the path I chose too before encountering an issue with one of the road conditions under which it proved challenging to track the lanes. A section of the road was white and that threw the algorithm off which is doing the tracking based on color information. I tried changing the thresholds but good tracking was not achievied on that section of the road. However, the RGB color space is not the only space that's avilable for us in computer vision. These may not be necessary all the time but can sometime transform the image enough to extract a particular fetaure set. Let's see if those transofrmations helps us for this road condition.

The below two images show a captured frame form the 'challenge.mp4' video with the change in road color for a section of the bridge.

![alt text][c1-RGB]![alt text][c2-RGB]

<br/><br/>

The below two images show how those two images looks like in the YUV space. In the first of these two images, the yellow line(marked as pink here) is prominent but as the line approaches the white section of the road, it's not easily distinguishable. 

![alt text][c1-YUV]![alt text][c2-YUV]

The below two images show the images when transofmed into the HSV space.The yellow lane markings show similar pattern in this space but the white lane gets lost on the white section of the road(second image)

![alt text][c1-HSV]![alt text][c2-HSV]

The below images show the HSL transofrmations of our original images. We can see that the yellow lane now shows up as a white line with a bluish overlay. Though it also suffers from a lack of contrast as the lane approaches the white section, this image(& the transformation) provided better results without significant hurdles to overcome.

![alt text][c1-HLS]![alt text][c2-HLS]

I applied a couple of filters (exclude green & include blue) to extract as much as the lane markings while ignoring everything else and then convert to a grayscale image. The website http://colorizer.org/ was very handy when adjusting the channel values manually to see how they would affect the image & to also look at the corresponding values in each color space.


### 2. Edge Detection
With the right color space selected, I applied an edge detection filter (I used Canny for this project) to retrieve the edges. I also applied a gaussian blurring filter to smooth the images before passing through the Canny filter.

### 3. Hough Transform
I defined a region of interest for the edges image and passed that along to Hough transform. The lines returned from this transofrmation are used to draw lines on the image.

To extrapolate the lines all the way, I made some changes to the 'draw_lines' method. First, I classified the lines into two buckets, left lane and right lane. I then calculated the slopes of each of those lines. I filtered out outliers from the slopes as debris on the road can looks like edges and hence could be used to draw the lane markings skewing the lines. To avoid a lot of the jittery affect on the lane marking going from frame to frame, I calculated an average slope weighing in the length of the individual segments.

### Room for improvement

A few shortcomings were encountered and though some were fixed before the initial submission a few remain Below is the list, their status & how I plan to overcome them in the near future.

1. A line was drawn from the max Y point on a lane to the min Y point of a lane. This resulted in a lane that did not track the lane properly.<br/>
  Status: Fixed. I changed my approach from the above to finding the weighted average slope. This provided better tracking of the lane &  avoided drastic changes in the line draw from frame to frame.
  
 2. The region of interest is defined with fixed values.<br/>
  Status: Fixed. I used a fixed region of interest for all 3 example videos. It worked fine for the first two and did not work with the challenge video. I changed my fixed region to a trapezoid with a generous width on the x-axis based off a fixed percent of the image size. This should now work well with inputs of different resolutions.
  
  3. Incorrect lane tracking.<br/>
  Status: Pending. There is one spot (a frame or a few) in the 'SolidYellowLeft' file where my pipeline draws the right lane line that is a little off of the actual lane markings (Check 0:11 in the output video). I believe this is due to a horizontal white line on the yellow lane marker. I hoped to overcome that by excluding the outliers but it still remains. Any suggestions for improvement is appreciated.
  
  
  4. The Challenge.<br/>
  Status. Fixed. My original pipeline did not work well for this. Part of it was because of the fixed definition of my region of interest ignoring certain parts of the lane. Fixing item #2 on this list helped greatly towards solving this. However, there was a section of the road where the lines went haywire though they return to normal after passing that section. But, unfortunately, we would have had a catastrophic crash by then. Changing the color space with the correct filters applied provided better tracking over this patch of the road.

### Conclusion
My pieline tracks the lane marking for the provided test cases very well. It also handles smaller curvatures in the road & different road conditions.

There could be some more improvements made if I can use curves instead of lines to track the lane markings.
