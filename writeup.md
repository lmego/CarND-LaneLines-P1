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
[image2]: ./output_images/masked_solidYellowLeft.jpg "Grayscale"
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

- Then I applied the Hough transformation function on the masked canny edge output image from the previous step. This function calls the draw function which draws single lines over each lane line. 

![alt text][image4]

When observing the Hough line segment outputs, one can conclude that the segments with negative slopes most likely belong to the left lane, and the segments with a positive slopes most likely belong to the right lane. 

Therefore, in order to draw a single line per lane, I modified the draw_lines() function to do the following:

- Loop through the line segments that were identified by the Hough algorithm and calculate each segment's slope 
- Store each Hough line segment in one of two separate numpy arrays based on the segment's slope (positive slope in left lane array, negative slope in right lane array). 
- Apply Polyfit algorithm to identify the coefficients that make up the two lines that best fit all segments for each lane
- In addition, to avoid any segments that are also picked up by the Hough algorithm and are NOT lanes, I applied a slope threshold on both positive and negative slopes before applying polyfit. This weed out certain line segments with almost flat slopes which would interfere and skew the line fitting in the previous step.

![alt text][image3]


###2. Identify potential shortcomings with your current pipeline

I see a few shortcommings with my current pipeline:

- Currently, the parameters are tuned and fixed in the code manually. I believe that as seen in the challenge video, certain frames/images may have different levels of brightness and shadows. These  bright colors, or rapid changes in brightness in the asphalt can cause extreme gradiants which are can be within the thresholds specified for the canny algorithm. This would create fake strong edges that can impact the ability of the code to properly identify the lanes. This can be seen from the "challenge" video "extra.mp4" 

- Again, given that the area of interest (mask) is also fixed, lane detection may stop working if there is a car close to the front of our car as the car edges will be detected and may be considered lanes by our algorithm. Also, we this pipeline will run into issue if the car encounters sharp turns/angles.


###3. Suggest possible improvements to your pipeline

- A possible improvement for the lane detection issue on would be to try different color spaces. We could also normalize the color to reduce the effects of the different light intensity on the asphalt.
