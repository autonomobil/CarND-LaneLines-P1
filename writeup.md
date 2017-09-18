# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**


[//]: # (Image References)

[image1]: ./MD_images/1_blur1.jpg "First Blur"
[image2]: ./MD_images/2_contrast.jpg "Contrast Increase"
[image3]: ./MD_images/3_yellow_white_mask.jpg "Boost Yellow and White with mask"
[image4]: ./MD_images/4_grayscale.jpg "Grayscale Blur"
[image5]: ./MD_images/5_blur2.jpg "Second Blur"
[image6]: ./MD_images/6_canny.jpg "Canny Edge Detection"
[image7]: ./MD_images/7_region_mask.jpg "Region Mask"
[image8]: ./MD_images/8_lines.jpg "All Lines and average Line"
[image9]: ./MD_images/9_result.jpg "Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 9 steps. 
* First Blur, this is especially important for the challenge video (the part with shadows).
    Also for getting ride of noise, before enhancing contrast
![1][image1]
* Minor Contrast increase for better detection if lighting is bad
![2][image2]
* Boost Yellow and White by converting to HSV and applying mask. Then combine with the incoming image
![3][image3]
* Grayscale the image (not necessarily necessary)
![4][image4]
* Second Blur, getting rid of noise (not necessarily necessary)
![5][image5]
* Canny Edge Detection
![6][image6]
* Apply region mask
![7][image7]
* Determine all lines with Hough Transformation, then calculate slope, y0 and length of every line.
    Then classify them into left and right, according to the slope (positive or negative).
    At the same time check against min and max slope, if not in this range, ignore.
    After this, calculate average slope and y0 with weighting in the length, to make longer lines more dominant.
    Then calculate the slope and y0 with moving average over variable number of frames (I chose 5 frames) to smooth.
    Store this information (slope, yo) in array for next frame for calculating moving average or if no lines are detected.
    Calculate the four points for the two lines (1 right, 1 left), format the output in the way, that different coloring is possible
![8][image8]
 * Draw the two lines on original image, choose red for not detected and green for detected line   
![9][image9]


### 2. Identify potential shortcomings with your current pipeline


Shortcomings:
    - the minimal and maximal slope of a line is set, so hard curves are not possible 
    - this pipeline is pretty computationally intensive, it takes some time. So it has possibly not real-time capability
    - in the challenge video the shadow and the light colored bridge are very difficult, accuracy is not very high


### 3. Suggest possible improvements to your pipeline

Improvements:
    - instead of plotting a straight line, you can polyfit to the data points and make curves more accurate
    
