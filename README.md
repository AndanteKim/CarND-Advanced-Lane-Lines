## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, my goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project. Please, check out the [writeup](https://github.com/AndanteKim/CarND-Advanced-Lane-Lines/blob/master/writeup.md) for this project and use it as a starting point for creating my own writeup.  

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames. The images in `output_images` are results of images collections for each step. Besides, I saved all test images each step in separate directories of `output_images` for the sake of a user's convenience. If you want to check more test images in detail, you can see all of them in `output_images` directory. Essentially, if you want to see mere results, you could check final testing images in `pipeline_final_result` of `output_images`. With respect to a result of video, I created the video named `project_video_output.mp4` through my pipeline and finished quality-check whether it works well or not. If you are interested in this project specifically, please read [writeup](https://github.com/AndanteKim/CarND-Advanced-Lane-Lines/blob/master/writeup.md).

Currently, I attempt to improve my pipeline to challenge The `challenge_video.mp4` video is an extra (and optional) challenge.


How to run the program
---

Prerequisite is the following:

- Make sure there are necessary libraries such as matplotlib, cv2, numpy and so on. You could check all necessary libraries through requirements.txt. Before running jupyter notebook, you need to install necessary libraries properly. In my case, I write a command: pip install -r requirements.txt in my local machine. For Anaconda users, you could use this command: conda install --file requirements.txt.

* After finishing the prerequisite above, command: jupyter notebook Advanced_finding_Lanes.ipynb. Then, run cells(shift+enter key) from top to bottom. Otherwise, there might be a syntax or semantic error. 
