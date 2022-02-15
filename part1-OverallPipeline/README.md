** Lane Detection and Tracking with Machine Learning Short Report **

In this project, my goal was to write a software pipeline to identify the lane boundaries in a video. While doing the project main steps of the code are image preproccessing, lane detection and lane tracking. They can be further divided as follows:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and try different methods to fit to find the lane boundaries.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

![image1]: ./outputs/undis_calibration1.jpg "Undistorted"
![image7]: ./outputs/orihinal_calibration1.jpg "Original Calibration Image"
![image2]: ./outputs/undist_test3.jpg "Distortion-corrected Road Image"
![image3]: ./outputs/binary_combo_test3.jpg "Binary Example"
![image8]: ./test_images/straight_lines2.jpg "Original RGB Image"
![image4]: ./outputs/warped_straight_lines2.jpg "Warp Example for Binary Image"
![image5]: ./outputs/fit_test3.jpg "Fit Visual"
![image6]: ./outputs/output_test2.jpg "Output"
![video1]: ./video1.mp4 "Video"

---


Mainly I have used the standart camera calibration code that is easily accessible in the community. To begin with calibration, preparing "object points", which is in a form of (x, y, z) coordinates of the chessboard corners in the real-world. Assuming the given chessboard is fixed on the z=0 plane, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. Just by using `objpoints` and `imgpoints`, we managed to calibrate camera by the `cv2.calibrateCamera()` function.  I applied the output opf the method to undistort the test image using the `cv2.undistort()` function and obtained realted images.

### Pipeline

After camera calibration a set of functions implemented for image preprocesing part and detection place part of the project. Some of the functions may be idle or some might be implemented more than once in order to handle different cases. At the last stage, code successfully runs and creates desired outputs. Please pardon the un-refactored code. Since the camera is calibrated, we can use the camera matrix and distortion coefficients to undistort the camera frames.
Since we will calculate the curvature of the road this step is really important. You can see the an example of undistorted test images in the related folder. In the 5th block in the notebook the function named "sobel_combined", I used grayscale image and saturation channel of hls image to calculate gradients and combined their results together to get more accurate results. I used this combination to create a single binary image which will be used to find lane lines and calculate road curvature. The function called unwarp takes an image as an input and create a bird view o the same image by using perspective transform. My source and destination points to apply perspective transform.

The fuction named "find_lane_pixels" identifies the pixels of lane lines from binary image and return their coordinates in image seperately for left and right lane line.Those points are used as unlabeled potential lane pixels. Then using several methods to group those lane pixel points as lane pixel lines. AS mentioned eralier kNN-liek algorithm, linear regression type curve fitters are used such as "fit_polynomial" fuction to fit a 2nd order polynomial which will be used later to calculate the curvature of of the lane lines. Results for the polynomial fit to lane lines can be seen in the related output folder.

In the 10th block of the my notebook there is a function called "curvature_radius" that is used to calculate the radius of the curvature. Radius of the curvature calculated from the output of points-to-line and using a quantitive ratio between real distance between two points in image and distance as pixels in image for that two points. An example of a output image can be seen  from outputs dir.

Here's a [link to my video result](./video1_output.mp4)