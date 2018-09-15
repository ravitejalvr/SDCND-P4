# Project-4 Advanced Lane Line Detection:

This Project consists of detecting and labeling lane lines real-time.

The project is split into the following steps:

* Calculate the camera calibration matrix and distortion coefficients using chessboard images.
* Using the above coefficients, undistort test images
* Create a binary image using color scales and sobel gradients with clear lane line detection.
* Apply perspective Transform for obtaining a top view (bird-eye view) of images.
* Find Lane pixels using sliding windows techniques. Apply different color to these lane pixels for detection
* Find Lane line curvature and offset from center of the vehicle.
* Blend the unwarped images using the reverse perspective transform.
* Display output and save project video.

The Final Video is given below:

[![IMAGE ALT TEXT](http://img.youtube.com/vi/k3ullb36rHM/0.jpg)](https://youtu.be/k3ullb36rHM)  

### Camera Calibration and Undistorting

This step is for converting the distorted images of camera into undistorted original images. For this, we first calibrate the camera matrix using chessboard images and use the calculated matrices for obtaining undistorted imageds.

The undistortion requires object points and images. Image points are taken from detected corners whereas object points are selected as 3x3 grid. These values are then passed to cv2.calibrateCamera() for obtaining calibration matrix. Then, the parameters obtained from the function were fed into cv2.undistort() function for undistorting images. The values of camera calibration were stored in a pickle file to prevent processing of them everytime.

This image is before it is undistorted

<a href="url"><img src="https://image.ibb.co/jiRYWk/before_persp.png" align="center" height="200" width="250" ></a>

After undistorting, the output is:

<a href="url"><img src="https://image.ibb.co/kO82P5/Undistorted_Image.png" align="center" height="200" width="250" ></a>

Notice that the difference is less and is observed only at left and right edges.

#### Image Binarization

This part deals with converting image into a binary image with lane lines detected. This part is critical as all other steps that follow are highly dependent on this step. For binarization, sobel gradients along with yellow and white line detection were used together.

Sobel gradient in x and y direction is found and the magnitude of it along with angle is calculated and suitable limits for threshold were applied for obtaining good binarized image which captures lane lines exclusively as much as possible

The binarized image is given below:

<a href="url"><img src="https://image.ibb.co/kBd2P5/binarize.png" align="center" height="200" width="250" ></a>

#### Perpective

This part of the project deals with transforming the image to bird-eye viee using perspective transform. The cv2 functions for this are:

cv2.getPerspectiveTransform and cv2.warpPerspective.

The perspective image is:

<a href="url"><img src="https://image.ibb.co/mt0hP5/after_persp.png" align="center" height="200" width="250" ></a>

#### Fitting lane lines with polynomial

This is done by identifying pixels for which polynomial is to be fit. Then, the pixels were fit with second degree polynnomial. In-case of video, the fit is approximated by previous fits.

For identifying pixels, histogram is used.

The output of this step is:

<a href="url"><img src="https://image.ibb.co/bMc7rk/sliding.png" align="center" height="200" width="200" ></a>


#### Lane curvature and offset from center

For this computation, it is assumed the frame in the mid of the image is the mid of vehicle and the deviation of it from detected lane center is offset from center. In a similar way, from the coefficients of the polynomial which was fit, the lane curvature was found.

The output after all these steps is:

<a href="url"><img src="https://image.ibb.co/cw7kcQ/output_7.png" align="center" height="200" width="200" ></a>

### Conclusion
The Project is challenging compared to previous projects due to the number of steps involved and also the calibration of the parameter by hand. There are a various number of parameters which needed hand calibration. The values are hardcoded and the pipeline relies extensively on the tuning of these parameters. 

A Deep Learning based method, which can directly extract the features and select the drivable path, would be a better solution to this issue of manually hard-coding the values.

