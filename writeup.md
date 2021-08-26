## Writeup

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./examples/undistort_car.png "Road Transformed"
[image3]: ./output_images/binary_images/test1.jpg "Binary Example"
[image4]: ./examples/warped_curved_lines.png "Warp Example"
[image5]: ./output_images/binary_images/Windows_Search/result/warp_test1.jpg "Fit Visual"
[image6]: ./output_images/pipeline_final_result.jpg "Output"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./Advanced_finding_Lanes.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

To check another results of test images, please check in [collection of undistorted chessboard images](https://github.com/AndanteKim/CarND-Advanced-Lane-Lines/blob/master/output_images/Undistorted_images)

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color(HLS color) and gradient thresholds(x sobel operator, direction thresholds) to generate a binary image.  Here's an example of my output for this step.

![alt text][image3]

To check other test images, please check in [collection of binary images](https://github.com/AndanteKim/CarND-Advanced-Lane-Lines/blob/master/output_images/binary_images)

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()`, which appears in the 7th code cell of the IPython notebook.  The `warp()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src_coordinates = np.float32(
        [[255, height-20], # Bottom left 
         [585, 460], # Top left
         [730, 460], # Top right
         [1125, height-20]]) # Bottom right
dst_coordinates = np.float32(
        [[240, height], # Bottom left 
         [240, 0], # Top left
         [1080, 0], # Top right
         [1080, height]]) # Bottom right
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 255, 700      | 240, 720        | 
| 585, 460      | 240, 0      |
| 1127, 460     | 1080, 0      |
| 695, 700      | 1080, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in in the 10th code cell of the `Advanced_finding_Lanes.ipynb`.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the 11th code cell of the `Advanced_finding_Lanes.ipynb` in the function `draw_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  

&emsp;For the first time, I got some technical error about matplotlib problem("error: /tmp/build/80754af9/opencv_1512491964794/work/modules/highgui/src/window.cpp:611: error: (-2) The function is not implemented. Rebuild the library with Windows, GTK+ 2.x or Carbon support. If you are on Ubuntu or Debian, install libgtk2.0-dev and pkg-config, then re-run cmake or configure script in function cvShowImage"). This is solved by plt.imshow instead of cv2.imshow. It really hindered me from working on the project and took a long time to figure out and fix this error. 

&emsp;Second, I was struggling to save correct filtered and binary images in step 2 and 3 correctly. When I searched the reason to fix it, the cause results from swapped red and blue channels. To fix this, we need to use numpy.flip(image, axis=-1) for a color image and binary_image * 255  for a binary image.

&emsp;Third, I currently struggle to work on a challenge and harder challenge videos after I finish "project_output" video. Those challenges are really hard and overwhelming for me to solve. I attempted several ideas to apply on pipelines for challenge videos. However, I failed to detect lanes on those challenges. It seems I need to approach those challenges more prudently and profoundly. My pipeline works in the requirement of project video, but it is stuck in challenge videos.

### What could you do to make it more robust?
&emsp;My approaching method to complete project is finishing each step thoroughly and testing all tests images. After testing, I saved all the images in output directory and checked their quality of images whether my results of images are satisfied with the requirements perfectly. If there is a syntax error, then I attempt to fix the error. In other of cases such as not good results of images, I see all the lines of code and figure out why it happens. Then, I attempt to fix the lines of code that I think it results in creating wrong images.<br></br>

&emsp; Especially, I had really hard time to figure out step 3, 4, 5, 6 and spent a lot of time to debug and complete those steps meticulously for a long time even though I saw lecture videos and review quizzes. Between step 3~6, even though I finish previous step perfectly, any problems such as bad frames will appear in the next step. For example, if there exists such as a bad frame, I analyzed and fixed my codes in the current step(i.e. step 6). If it does not work at all, then I went back to the previous step to check what my logic is wrong in the lines of code. While I completed the project video, there is no significant problem to combine all the steps as my pipeline except trivial syntax errors and working on between step 3 and 6. To prove my statement, the output of project video proves display detecting lanes well.

### Where will your pipeline likely fail?  
&emsp;I think my pipeline fails when I run challenge videos. I keep figuring out where problems occur radically. The probem is when there are specific points, not being able to detect lanes, np.polyfit makes none. At my perspective, the problem might result from the shade points. For example, it will happen when a car passed around shaded area such as a tunnel or shaded road. To fix this, I need to improve the algorithm I keep improving my algorithms between step 3~6, but I cannot find clear solution until now even though I ask a mentor how to solve this. To solve this, I need to study and search for related materials deeply.