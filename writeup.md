# **Finding Lane Lines on the Road** 
[//]: # (Image References)

[image0]: ./test_images/solidWhiteCurve.jpg
[image1]: ./test_images_gray/solidWhiteCurve.jpg
[image2]: ./test_images_blur/solidWhiteCurve.jpg
[image3]: ./test_images_canny/solidWhiteCurve.jpg
[image4]: ./test_images_region/solidWhiteCurve.jpg
[image5]: ./test_images_hough/solidWhiteCurve.jpg
[image6]: ./test_images_merged/solidWhiteCurve.jpg


In order to detect road lines in images and videos (sequence of images) one has to perform the following seven steps:

1. Read the images on which you want to detect road lines
![alt text][image0]
2. Convert RGB image to a grayscale image
![alt text][image1] 
3. Apply Gaussian smoothing to gray images
![alt text][image2] 
4. Perform Canny edge detection to smoothed gray images
![alt text][image3] 
5. Crop images to region of interest ROI and mask away the undesired portions of the image
![alt text][image4] 
6. Apply Hough transformation to images
![alt text][image5] 
7. Merge original image with hough lanes
![alt text][image6] 

For performing the processing you need the OpenCV computer vision and image processing library. The challenge here is to figure out the parameters of each function in order to produce the desired output. The Gaussian smoothing function takes a kernel size as an input. The bigger the kernel the blurrier is the image. The Canny edge function takes a high and low thresholds, which determine a minimum difference in intensity to establish an edge and to form a contiguous extension of an established edge, respectively. The Hough transform function takes as paramters: the resolution for line position and orientation, a minimum number of points to establish a line, the minimum length of a line, and the maximum gap between points allowed for a line. Please look at the test_images folders as I have saved the images after each step of my pipeline for each test image.

NOTE: The hough transform gives us small lines, we can extrapolate these small lines to make a left and right lanes spreading from the bottom to the top (marked with red in the figures). The first step in generating the red lines is to separate the small line X,Y coordinates into two groups/arrays i.e. one with a positive slope (the left line) and one with a negative slope (the right line). After that one can use the polyfit function to get the slope and the intercept of the left and the right lines (line equation y= slope * x + intercept). At the end based on the slope, intercept and the max coordinates X, Y of the image, one can calculate the end points of the lines and draw full lines with the cv2.line function.

This pipeline worked well enough, and it was possible to identify the lines in all test images. In order to identify lines in a video the above steps were included in a function for processing image. Then the same function was run to the sequence of the images generated in a video. You can find the generated videos in [https://github.com/frtunikj/sdc_finding_lane_lines/test_videos_output](https://github.com/frtunikj/sdc_finding_lane_lines/blob/master/test_videos_output). 

### Potential shortcomings of the current pipeline

The approach/pipeline described above works well with the test data provided. One potential shortcoming would be in case the camera angle is different with that of the region of interest ROI, since currently the ROI is hardcoded. In addition, if the lane markings in the figures/videos were with a bad quality i.e. the difference between the color of the lanes were too minimal, the approach would not be able to detect the lanes by using the canny transform. 

### Possible improvements of the current pipeline

A possible improvement would be to tweak the parameters for even better performance. Further, currently only straight lines were in the current scope of detection. Fitting curved line to canny edge lines would perform in curves. A lot of disturbing lines are produced in case there are people or other object e.g. cars in between the two lanes/in front of the car. To overcome this we would have to filter that out from the image before in canny edge detection is run.

### References

Further useful readings:

* https://docs.opencv.org/trunk/da/d22/tutorial_py_canny.html 
* https://docs.opencv.org/2.4/modules/imgproc/doc/filtering.html?highlight=gaussianblur#gaussianblur 
* https://www.udacity.com/course/introduction-to-computer-vision--ud810 (Lesson 8: 2B-L1 Hough transform: Lines) 
