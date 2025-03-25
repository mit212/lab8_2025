# Lab 8: Computer Vision

2.12/2.120 Intro to Robotics  
Spring 2025[^1]

<details>
  <summary>Table of Contents</summary>

- [0 (Prelab) Software Set Up](#0-prelab-software-set-up)
  - [0.1 Python](#01-python)
  - [0.2 OpenCV](#02-opencv)
  - [0.3 Matplotlib](#03-matplotlib)
  - [0.4 AprilTag Library](#04-apriltag-library-optional)
- [1 Hardware Set Up](#1-hardware-set-up)
- [2 Gamma Adjustment](#2-gamma-adjustment)
- [3 Otsu’s method of Thresholding](#3-otsus-method-of-thresholding)
- [4 Canny Edge Detection](#4-canny-edge-detection)
- [5 Hough Transforms](#5-hough-transforms)
  - [5.1 Lines](#51-lines)
  - [5.2 Circles](#52-circles)
  - [5.3 Sudoku Grid](#53-sudoku-grid)
- [6 HSV vs RGB](#6-hsv-vs-rgb)
- [7 Morphological Operations](#7-morphological-operations)
- [8 Optical Flow](#8-optical-flow)

</details>

In this lab, you will be trying out different functions from OpenCV to visualize the computer vision techniques introduced in lecture. The entire lab is designed to take less than an hour. Before the lab begins, we will do a quick demo of AprilTag detection and pose estimation along with the associated camera calibration procedure. The rest of lab time is allotted to working on the final project.

## 0 (Prelab) Software Set Up

### 0.1 Python

You should already have Python (version 3.9+) installed from [Lab 4](https://github.com/mit212/lab4_2025?tab=readme-ov-file#01-python). You can check which version of Python you have by entering `python3 -V` or `python -V` in your terminal.

### 0.2 OpenCV

OpenCV is an open source computer vision library. To install OpenCV, enter `pip install opencv-python` in your terminal. 

### 0.3 Matplotlib

Matplotlib is a comprehensive library for creating static, animated, and interactive visualizations in Python. To install Matplotlib, enter `pip install matplotlib` in your terminal.

### 0.4 AprilTag Library [Optional]

Python bindings for the Apriltags3 library developed by AprilRobotics, used in the AprilTags demo code. To install, enter `pip install pupil-apriltags` in your terminal.

## 1 Hardware Set Up 

You will be using the built-in camera on your laptop. If your laptop does not have one, please ask the staff for an external camera.

Additionally, you should choose an object, preferably one of a solid color, to use for the entire lab. Throughout the lab, we will attempt to separate that object in the images taken by your camera from everything else.

## 2 Gamma Adjustment

Clone this repository or download/extract the ZIP file to your computer. You can use any code editor that supports Python. We recommend using IDLE, Python's built-in editor that should have been installed alongside Python, to edit and run each file.

In computer vision, one of the first things introduced is gamma adjustment or correction. To experiment with this concept, open and run `gammaAdj.py`.

When you run this file, three windows will appear. One will show your unprocessed raw camera footage, another will show a processed (gamma adjusted image), and the last will have a slider. Play around with the slider and see what you observe (make it completely black or white).

To stop running the script, click on the `IDLE Shell 3.x.x` window and press `Ctrl+C`, then close the shell window.

## 3 Otsu’s Method for Automatic Image Thresholding

It is important to note that one of the prime goals of computer vision is to separate an image into *what you care about* and *what you don’t care about*. Otsu’s method of thresholding is a good example of a simple method that algorithmically separates pixels into two separate classes. To use it, open and run `otsu.py`.

When you run this command, a single image will be taken and then processed using Otsu’s method of thresholding. A histogram will appear on your screen, as well as the unprocessed and processed image.

You should remember from lecture how Otsu's method works. Hint: the teal line on the histogram seperates the pixels into two classes. All pixels from one class are set to black, and all pixels from the other class are set to white. The threshold seperating the pixels is set to minimize the intra-class variance, defined as a weighted sum of variances of the two classes.

## 4 Canny Edge Detection

Though Otsu’s method is nice due to the algorithm being relatively straightforward and fast, it is apparent that more complex techniques may be needed, especially to reduce background noise and handle cases where the object is a similar color to the background. This will help us further tackle the problem of separating *what you care about* from *what you don’t care about*.

To that end, Canny edge detection is an attempt at answering this problem when all you care about are **edges**. Hypothetically, since your object has edges, you would be able to separate it from its background if no background edges intersect with the object, or the background is "edgeless" (e.g. a solid colored wall). To experiment with the concept further, open and run `canny.py`. You may need to play with the sliders a bit before the edges start showing up. The sliders affect the minimum and maximum intensity gradient necessary for an edge to be counted, as described [here](https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html#:~:text=Hysteresis%20Thresholding).

You’ll notice that if you increase the lower threshold above the upper threshold, the algorithm should automatically switch so that the lower threshold becomes the upper threshold for the algorithm.

## 5 Hough Transforms

Similarly to how Canny edge detection would work great if your object is the only object with edges, Hough transforms are great if your object is either circular or a line.

### 5.1 Lines

Open and run `houghLines.py`. Note that a Canny edge detector is used to pre-process the image, to make it easier for the Hough transform to "agree" on lines. Play around with the sliders to see how changing the thresholds affects which lines are detected in the image. Unlike the circles algorithm, the lines algorithm is much less time intensive when it comes to false negatives (e.g. a line exists but is not detected), so there is no image shrinking done for this one.

You can find more information on the commands used [here](https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_lines/hough_lines.html).

### 5.2 Circles

Open and run `houghCircles.py`. Due to how many false circles are detected by the algorithm, the frame is shrunk with respect to the original captured image. This is accounted for in the accumulator ratio slider bar, which controls the ratio of image resolution to accumulator resolution. For example, if `Accumulator Ratio = 1`, the accumulator will have the same resolution as the input image. If `Accumulator Ratio = 2`, the accumulator will have half as big width and height. You can also change the degree the image is shrunk in line 44 of the code.

You can find more information on the commands used [here](https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_circle/hough_circle.html).

### 5.3 Sudoku Grid

Before running this on camera footage, you will be processing `sudoku.png`. Namely, edit the `TODO's` in `process.py`. The goal for this script is to isolate the grid lines (and **only** the grid lines) of the sudoku grid. If you see `TypeError: 'NoneType' object is not iterable`, no lines were detected in the Hough lines transform using your parameters.

Don't just blindly change the thresholds until something changes. Look at the code and understand how it works first. What intermediate image processing step do you need to confirm is working properly before the Hough lines transform has a chance of detecting any lines in the image?

| :white_check_mark: CHECKOFF 1 :white_check_mark:   |
|:---------------------------------------------------|
| Show the detected grid output to a TA or LA. |

## 6 HSV vs RGB

If you were paying very close attention, you may have noticed that ALL the algorithms before this section are conducted after converting your raw image into grayscale. With that in mind, we will now experiment with colors. For this section, in addition to the object you’ve been using, we ask that you find a few different colored objects to experiment with the robustness of the two methods for separation presented here. To begin the script, open and run `colorThresh.py `.

After capturing the objects of your choosing, you’ll want to note down the HSV values for the next part of the lab.

## 7 Morphological Operations

After using the color thresholding, you may have noticed that there are some features that remain in the image aside from your object that you want to detect. With that in mind, morphological operations are an excellent way of getting rid of extra features you don’t want and enlarging objects you do want. To experiment and find which operations work best, open and run `morphOps.py`. Use the HSV values from the [previous section](#6-hsv-vs-rgb)

Something else interesting about morphological operations is the kernel operator that is used. If you open the script, you’ll find a section of code that allows you to write in your own kernel (a vertical line is given as an example in the comments). Rerun the script with a kernel of your own design.

| :white_check_mark: CHECKOFF 2 :white_check_mark:   |
|:---------------------------------------------------|
| Show your custom kernel in action to a TA or LA. |

## 8 Optical Flow

Optical flow has several uses. The one we’ll present here is tracking an "object" as it moves through space. Hypothetically, if you were able to place trackers based on the output of your morphological operations presented before, you’d be able to use optical flow to track your object in space. In this case, we will be placing trackers on detected corners. To run the optical flow script, open and run `opticalFlow.py`.

To tune the features tracked by the script, you can edit the variable `feature_params` and decrease the `qualityLevel` and increase the `maxCorners`. Tune these features so that when running this script, a corner is detected on your object.

| :white_check_mark: CHECKOFF 3 :white_check_mark:   |
|:---------------------------------------------------|
| Show the optical flow script in action. |

[^1]: Version 1 - 2016: Peter Yu, Ryan Fish and Kamal Youcef-Toumi  
  Version 2 - 2017: Luke Roberto, Yingnan Cui, Steven Yeung and Kamal Youcef-Toumi  
  Version 3 - 2019: Jerry Ng, Jacob Guggenheim  
  Version 4 - 2020: Jerry Ng, Rachel Hoffman, Steven Yeung, and Kamal Youcef-Toumi  
  Version 5 - 2020: Phillip Daniel  
  Version 6 - 2024: Jinger Chong  
  Version 7 - 2025: Roberto Bolli Jr., Kaleb Blake
