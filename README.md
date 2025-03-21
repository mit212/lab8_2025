# Lab 8: Computer Vision

2.12/2.120 Intro to Robotics  
Spring 2024[^1]

<details>
  <summary>Table of Contents</summary>

- [0 (Prelab) Software Set Up](#0-prelab-software-set-up)
  - [0.1 Python](#01-python)
  - [0.2 OpenCV](#02-opencv)
  - [0.3 Matplotlib](#03-matplotlib)
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
- [9 Feedback Form](#9-feedback-form)

</details>

In this lab, you will be trying out different functions from OpenCV to visualize the computer vision techniques introduced in lecture. The entire lab is designed to take less an hour. The rest of lab time is allotted to working on the final project.

## 0 (Prelab) Software Set Up

### 0.1 Python

You should already have Python (version 3.9+) installed from [Lab 4](https://github.com/mit212/lab4_2024?tab=readme-ov-file#01-python). You can check which version of Python you have by entering `python3 -V` or `python -V` in your terminal.

### 0.2 OpenCV

OpenCV is an open source computer vision library. To install OpenCV, enter `pip install opencv-python` in your terminal. 

### 0.3 Matplotlib

Matplotlib is a comprehensive library for creating static, animated, and interactive visualizations in Python. To install Matplotlib, enter `pip install matplotlib` in your terminal.

## 1 Hardware Set Up 

You will be using the built-in camera on your laptop. If your laptop does not have one, please ask the staff for an external camera.

Additionally, you should choose an object, preferably one of a solid color, to use for the entire lab. Throughout the lab, we will attempt to separate that object in the images taken by your camera from everything else.

## 2 Gamma Adjustment

Clone this repository.

In computer vision, one of the first things introduced is gamma adjustment or correction. To experiment with this concept, open and run `gammaAdj.py`.

When you run this file, three windows will appear. One will show your unprocessed raw camera footage, another will show a processed (gamma adjusted image), and the last will have a slider. Play around with the slider and see what you observe (make it completely black or white).

To stop running the script, go to the terminal in VSCode and enter `Ctrl+C`.

## 3 Otsu’s method of Thresholding

It is important to note that one of the prime goals of computer vision is to separate an image into *what you care about* and *what you don’t care about*. Otsu’s method of thresholding is a prime example of a simple method that algorithmically separates pixels into two separate classes. To use it, open and run `otsu.py`.

When you run this command, a single image will be taken and then processed using Otsu’s method of thresholding. A histogram will appear on your screen, as well as the unprocessed and processed image.

## 4 Canny Edge Detection

Though Otsu’s method is nice as it comes directly as an output of a relatively fast algorithm, it is apparent that more complex techniques need to be developed so that we can tackle this problem of separating *what you care about* and *what you don’t care about*.

To that end, Canny edge detection is an attempt at answering this problem when all you care about is **edges**. Hypothetically, as your object has edges, you would be able to separate it from its background if it is an edgeless background. To experiment with the concept further, open and run `canny.py`.

You’ll notice that if you increase the lower threshold above the upper threshold, the algorithm should automatically switch so that the lower threshold becomes the upper threshold for the algorithm.

## 5 Hough Transforms

Similarly to how Canny Edge detection would work great if your object is the only object with edges, Hough transforms are great if your object is either circular or a line.

### 5.1 Lines

Open and run `houghLines.py`. Unlike the circles algorithm, the lines algorithm is much less time intensive when it comes to false negatives so there is no image shrinking done for this one.

You can find more information on the commands used [here](https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_lines/hough_lines.html).

### 5.2 Circles

Open and run `houghCircles.py`. Due to how many false circles are detected by the algorithm, the frame is shrunk with respect to the original captured image. This is accounted for in the accumulator ratio slider bar. If you wish to play more with this, you can change the ratio of the shrunk image with respect to the ratio.

You can find more information on the commands used [here](https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_circle/hough_circle.html).

### 5.3 Sudoku Grid

Before running this on camera footage, you will be processing `sudoku.png`. Namely, edit the `TODO`s `process.py`. The goal for this script is to isolate the grid lines and only the grid lines of the sudoku grid. 

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

## 9 Feedback Form

Before you leave, please fill out https://tinyurl.com/212-feedback. 

| :white_check_mark: CHECKOFF 3 :white_check_mark:   |
|:---------------------------------------------------|
| Show the feedback form completion screen to a TA or LA. |

[^1]: Version 1 - 2016: Peter Yu, Ryan Fish and Kamal Youcef-Toumi  
  Version 2 - 2017: Luke Roberto, Yingnan Cui, Steven Yeung and Kamal Youcef-Toumi  
  Version 3 - 2019: Jerry Ng, Jacob Guggenheim  
  Version 4 - 2020: Jerry Ng, Rachel Hoffman, Steven Yeung, and Kamal Youcef-Toumi  
  Version 5 - 2020: Phillip Daniel  
  Version 6 - 2024: Jinger Chong