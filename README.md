# **Finding Lane Lines on the Road**


**Finding Lane Lines on the Road**


[//]: # (Image References)

[image1]: ./MD_images/1_blur1.png "First Blur"
[image2]: ./MD_images/2_contrast.png "Contrast Increase"
[image3]: ./MD_images/3_yellow_white_mask.png "Boost Yellow and White with mask"
[image4]: ./MD_images/4_grayscale.png "Grayscale Blur"
[image5]: ./MD_images/5_blur2.png "Second Blur"
[image6]: ./MD_images/6_canny.png "Canny Edge Detection"
[image7]: ./MD_images/7_region_mask.png "Region Mask"
[image8]: ./MD_images/8_lines.png "All Lines and average Line"
[image9]: ./MD_images/9_result.png "Result"

---

### Reflection
Results:
[solidWhiteRight - Youtube Link](https://www.youtube.com/watch?v=ZBBuqd8Gxjk&list=PLT4_vNeVR74lCL9pbwh6b1csRqTj63MfU)
[solidYellowLeft - Youtube Link](https://www.youtube.com/watch?v=oE11-MPvHdY&index=2&list=PLT4_vNeVR74lCL9pbwh6b1csRqTj63MfU)
[challenge - Youtube Link](https://www.youtube.com/watch?v=SZEhOOdH60o&list=PLT4_vNeVR74lCL9pbwh6b1csRqTj63MfU&index=3)

### 1. Description of the pipeline.

My pipeline consisted of 9 steps.
* First Blur, this is especially important for the challenge video (the part with shadows).
    Also for getting ride of noise, before enhancing contrast
![image1]

* Minor Contrast increase for better detection if lighting is bad
![image2]

* Boost Yellow and White by converting to HSV and applying mask. Then combine with the incoming image
![image3]

* Grayscale the image (not necessarily necessary)
![image4]

* Second Blur, getting rid of noise (not necessarily necessary)
![image5]

* Canny Edge Detection
![image6]

* Apply region mask
![image7]

* First Determine all lines with Hough Transformation, then calculate slope, y0 and length of every line.
    * Then classify them into left and right, according to the slope (positive or negative).
    * At the same time check against min and max slope, if not in this range, ignore line.
    * After this, calculate average slope and y0 with weighting in the length, to make longer lines more dominant.
    * Then calculate the slope and y0 with moving average over variable number of frames (I chose 4 frames) to smooth.
    * Store this information (slope, yo) in array for next frame for calculating moving average or if no lines are detected.
    * Calculate the four points for the two lines (1 right, 1 left), format the output in the way, that different coloring is possible
![image8]

 * Draw the two lines on original image, choose red for not detected and green for detected line   
![image9]


### 2. Potential shortcomings with the current pipeline


Shortcomings:
    - the minimal and maximal slope of a line is set, so hard curves are not possible
    - this pipeline is pretty computationally intensive, it takes some time. So it has possibly not real-time capability
    - in the challenge video the shadow and the light colored bridge are very difficult, accuracy is not very high


### 3. Suggest possible improvements to the pipeline

Improvements:
    - instead of plotting a straight line, you can polyfit to the data points and make curves more accurate
    - Don't grayscale and blur a second time to save computing time
    - the challenge video was  not accurate
