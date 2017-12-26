# **Advanced Lane Finding Project** 

## Writeup 

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)


[radius_curvature]: ./output_images/radius_curvature.png "Radius Cuvature Formula"
 
## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. 

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the 4th code cell of the IPython notebook located in "./P4.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

Then I used the variables `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function (code cell #7). I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 



Original chess board              |  Undistorted chess board 
:-------------------------:|:-------------------------:
<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen1.png" width="350">  |  <img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen2.png" width="350">


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will show the distortion correction to one of the test images like this one:
<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen3.png" width="350">

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at cell codes #12 through #21 in `./P4.ipynb`).  Here's an example of my output of this step.  (note: this is actually from one of the test images)

<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen4.png" width="350">

After that, we apply a mask to the image (cell codes #22 through #24 in `./P4.ipynb`)., the result of this operation on the above image is shown below: 

<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen5.png" width="350">

Finally, we take the gradient in the x direction with a threshold. The resulting image is shown below:

<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen6.png" width="350">


#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()`, which appears in code cell #18 in the file `./P4.ipynb`.  The `warp()` function takes as inputs an image (`img`), the source (`src`) and destination (`dst`) points are taken from global variables previously calculated.  I chose the source and destination points by manual inspection. Here they are:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 200,720     | 280,720      | 
| 580,480     | 400,190      |
| 720,480    | 920, 190     |
| 1050,700      | 960, 720       |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen5.png" width="850">

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial, which appears in code cell #21 in the file `./P4.ipynb`. I tried other methods such as Isotonic Regression but its performance was worse than the 2nd order polynomial, that is why I used lane lines with a 2nd order polynomial.



#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in cell code 22# in the IPython file `./P4.ipynb`. I used the following formula that mentions in the videos. 

![alt text][radius_curvature]

This expression is relatively easy to calculate in the case of other methods such as Isotonic Regression is very difficult, this is a good point to choose 2nd order polynomial.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the cell code 23# through 26#  in `./P4.ipynb`.  Here is an example of my result on a test image:

<img src="https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_images/imagen6.png" width="750">


---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://github.com/ricardoues/CarND-Advanced-Lane-Lines/raw/master/output_videos/outputVideo.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The pipeline could fail when the conditions of brightness being different to the images considered. We can make the pipeline more robust considering images from other places and with different climate conditions.
