#**Finding Lane Lines on the Road** 

[//]: # (Image References)

[gray]: ./md/gray.jpg "gray"
[dark_gray]: ./md/dark_gray.jpg "dark_gray"
[masked_gray]: ./md/masked_gray.jpg "masked_gray"
[edges]: ./md/edges.jpg "edges"
[hough]: ./md/hough.jpg "hough"

---

### Reflection

###1. Pipeline description

The pipleline consisted of 5 steps. 

First, a copy of the image is converted to grayscale. 
![alt text][gray]
It is then darkened to decrease contrast of unwanted areas. 
![alt_text][dark_gray]
Another copy of the image is converted to HSV, from which two masks where retrieved for the yellow and white lane lines.
A bitwise OR operation is performed on both masks, and the result is used in another bitwise OR operation with the darkened grayscale image.
![alt_text][masked_gray]
Canny edge detection is performed on the resultant image.
![alt_text][edges]
Hough transform is used to extract lines from the canny edges.

In order to draw a single line on the left and right lanes, the draw_lines() function was edited so as to filter lines based on slope and image quadrant, ie. discarding lines with slopes outside a predefined range and classifying left and right lane associations. End points of the filtered lines are then inserted into a RANSAC regressor to find the best fit pair of lines to represent the two lanes.
![alt_text][hough]



###2. Current pipeline's potential shortcomings

This simplified lane detection algorithm has several scenarios where it could easily be declared incapable. The algorithm is robust enough to handle shadows and road colour variations, however, in a scenario where a car comes too close to the ego vehicle, an area that has no lane lines, an area that has pedestrian markings (zebra strips), or even during low light conditions the algorithm would easily become confused.

Another shortcoming could be the algorithm's ability to detect curved roads. Currently, only straight roads are accurately detected. Curved roads are only seen as straight segments.


###3. Possible improvements

A possible improvement would be to use a state estimation method to predict the position of the lane lines, and update the local estimate with the observed readings. This would greatly imporve performance but would require a great amount of effort to reach the certainty model for the observations.

Another potential improvement could be to fit the detected lines into a second degree curve to be able to detect curved roads more accurately.
