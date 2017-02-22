#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./output_images/edges_solidYellowLeft.jpg "Grayscale"
[image2]: ./output_images/masked_solidYellowLeftjpg "Grayscale"
[image3]: ./output_images/lanes_solidYellowLeft.jpg "Grayscale"
[image4]: ./output_images/blended_solidYellowLeft.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:
- First, I converted the images to grayscale, then I applied gaussian smoothing to the grayscale image with kernel size=5. 
- Next, I applied the canny edge detection using a 1:2 low to high threshold ratio (75-150)
![alt text][image1] 
- Then, I defined the 4 vertices that would make up the region of interes where we will apply the hough transformation. The position of the vertices were defined as "fractions" of the total length and heigh of the initial image. This was done so that this region of interest remains consistent across different image/video sizes.
- Next I used those vertices to apply the mask over the output image from the canny edge detection algorithm
![alt text][image2]
- Then I applied the Hough transformation on the masked canny edge output image from the previous step which also uses the draw function to draw single lines over 
![alt text][image4]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. By observing the test images, one can deduce that the Hough line segments with negative slope most likely belong to the left lane, and the Hough line segments with a positive slope most likely belong to the right lane. Therefore, the new function loops through the line segments, calculates the slope of each line segment and stores them in separate numpy arrays depending on their slope. To avoid any segments that are also picked up by the Hough algorithm and are not lanes, I also had to implement a threshold on both positive and negative slope. This helped me increase the accuracy of the single lane lines that had to be drawn. To actually find the two single lines that represented the left lane and the right lane I used polyfit to fit a line through the segments for the left lane and another one for the segments of the right lane. Then I used the coefficients obtained from that function to calculate/extrapolate the points that would made up each line. 

![alt text][image3]



###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...