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

<img src="https://image.ibb.co/kO82P5/Undistorted_Image.png" alt="Undistorted" style="width: 50px; height: 50px;"/>
<img src="https://image.ibb.co/mt0hP5/after_persp.png" alt="Undistorted" style="width: 50px; height: 50px;"/>
<img src="https://image.ibb.co/jiRYWk/before_persp.png" alt="Undistorted" style="width: 50px; height: 50px;"/>
<img src="https://image.ibb.co/kBd2P5/binarize.png" alt="Undistorted" style="width: 50px; height: 50px;"/>
<img src="https://image.ibb.co/cw7kcQ/output_7.png)" alt="Undistorted" style="width: 50px; height: 50px;"/>
<img src="https://image.ibb.co/bMc7rk/sliding.png)" alt="Undistorted" style="width: 50px; height: 50px;"/>

### Camera Calibration and Undistorting

This step is for converting the distorted images of camera into undistorted original images. For this, we first calibrate the camera matrix using chessboard images and use the calculated matrices for obtaining undistorted imageds.

The undistortion requires object points and images. Image points are taken from detected corners whereas object points are selected as 3x3 grid. These values are then passed to cv2.calibrateCamera() for obtaining calibration matrix. Then, the parameters obtained from the function were fed into cv2.undistort() function for undistorting images. The values of camera calibration were stored in a pickle file to prevent processing of them everytime.

This image is before it is undistorted
![img_normal](https://image.ibb.co/jiRYWk/before_persp.png){:height="36px" width="36px"}.

After undistorting, the output is:
![img_normal](https://image.ibb.co/kO82P5/Undistorted_Image.png)

Notice that the difference is less and is observed only at left and right edges.

#### Image Binarization

This part deals with converting image into a binary image with lane lines detected. This part is critical as all other steps that follow are highly dependent on this step. For binarization, sobel gradients along with yellow and white line detection were used together.

Sobel gradient in x and y direction is found and the magnitude of it along with angle is calculated and suitable limits for threshold were applied for obtaining good binarized image which captures lane lines exclusively as much as possible

The binarized image is given below:

![img_normal](https://image.ibb.co/kBd2P5/binarize.png)

#### 3. Perpective transform

The code for my perspective transform includes a function called `perpective()`, which appears in "Perpective View" cell of the notebook.  The `perpective()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the following source and destination points:

```python
src = np.float32([[w, h-15], [0, h-15], [546, 460], [732, 460]])
dst = np.float32([[w, h], [0, h], [0, 0], [w, 0]])
```


I verified that my perspective transform was working as expected by visualizing a test image and its warped counterpart.

![alt text][image4]

#### 4. Fitting lane lines with polynomial

When fitting polynomials to lane lines, there are 2 cases:
- New image - I use the `sliding_windows()` function to find the peak locations of the histogram of the binary image, and then slide two windows towards the upper side of the image, deciding which pixels belong to the lane line.
- In case of video processing, we can limit our search in the neighbourhood of previously detected lane lines. I do this using the method `approx_by_previous_fits()`

![alt text][image5]

#### 5. Radius of curvature and offset from center calculation

The offset from the lane center can be computed assuming that the camera is fixed and mounted in the midpoint of the car roof. In this case, we can approximate the car's deviation from the lane center as the distance between the center of the image and the midpoint at the bottom of the image of the two lane-lines detected. This is done using the method `compute_offset_from_center()`.  
The radius of curvature can be computed using the coefficients of 2nd degree polynomial fitted to the lane lines. I did this in the method `curvature_meter()`.

#### 6. Final Output

The whole pipeline produces the following image:
Other images can be found in the `output_images` directory.  

![alt text][image6]


### Discussion
One of the main issues that I see with the approaches in this project is the selection of parameters by hand for these specific road images. This clearly makes this approach hard to generalize. The pipeline is very fragile to image changes such as variations in brightness, contrast etc. as the thresholding parameters have been hardcoded and the whole pipeline depends heavily on the binarized image.  
