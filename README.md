#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image_step1]: ./examples/pipeline_001.png "Step1"
[image_step2]: ./examples/pipeline_002.png "Step2"
[image_step3]: ./examples/pipeline_003.png "Step3"
[image_step4]: ./examples/pipeline_004.png "Step4"
[image_step5]: ./examples/pipeline_005.png "Step5"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Here are the overall steps of the pipeline:

######Step1: Get yellow lane marking (get_lane_markings() method) -- I used HSV values to find lane markings (white and yellow) to overcome the challenge with shadows.
![alt text][image_step1]

######Step2: Comine yellow lane from Step1 with original image
![alt text][image_step2]

######Step3: Get masked hough lines (get_lanes() method) -- I used "temporal smoothing" technique by tracking the previous lines information to overcome noisy data issue.
![alt text][image_step3]

######Step4: Draw lanes
![alt text][image_step4]

######Step5: Produce the final out by combining the line images from Step4 with the original image
![alt text][image_step5]

The "temporal smoothing" technique mentioned in the above Step 2 consists of 8 procedures:
1. Have a global queue that tracks the latest hough lines.
2. If previous line slopes exists, calculate the shortest distance between the previous left & right line segments and current hough lines, and discard the hough lines if the dinstance is beyond the acceptable range. With the clean hough lines as well as past hough lines stored in the global queue, calculate the current line slope and y-intercept for left and right line through line fitting.
3. Calculate the current line slope and y-intercept for left and right line through line fitting or averaging (I used averaging).
4. Dampen the above line slopes and y-intercepts like this: cur_left_m = cur_left_m * 0.2 + prev_left_m * (1 - 0.8).
5. Extrapolate the cur_left_m, cur_left_b, cur_right_m and cur_right_b to create final right and left lines.
6. Store the current slope and y-intercept as previous slope and y-intercept for both left and right line.
7. Store the current clean hough lines to the global queue, and pop the old hough lines if the queue is full.
8. Draw the final right and left lines to the frame image.



<!-- My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...
If you'd like to include images to show how the pipeline works, here is how to include an image: 
![alt text][image1]
 -->

###2. Identify potential shortcomings with your current pipeline

* My algorithm cannot handle curved lines
* My algorithm will most likely fail when it fails to detect edges due to shadow or too much light.
* My algorithm will also likely fail in bad weather such as rain or snow.
* My algorithm will also probably fail when a truck is blocking the front view.


###3. Suggest possible improvements to your pipeline

* To make the lane findings algorithm better/robust, I willl need to use the perspective transformation so that I can work on a rectangular space instead of a triange with vanishing point.
* I will need to enhance the algorithms so that it is not limited to two lanes but multiple lanes.
* Instead of purely relying on the Canny edge algorithm, the algorithm can be further enhanced by applying color thresholds to be more robust when the frame is impacted by shadow.


The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you can install the starter kit or follow the install instructions below to get started on this project. ##

**Step 1:** Getting setup with Python

To do this project, you will need Python 3 along with the numpy, matplotlib, and OpenCV libraries, as well as Jupyter Notebook installed. 

We recommend downloading and installing the Anaconda Python 3 distribution from Continuum Analytics because it comes prepackaged with many of the Python dependencies you will need for this and future projects, makes it easy to install OpenCV, and includes Jupyter Notebook.  Beyond that, it is one of the most common Python distributions used in data analytics and machine learning, so a great choice if you're getting started in the field.

Choose the appropriate Python 3 Anaconda install package for your operating system <A HREF="https://www.continuum.io/downloads" target="_blank">here</A>.   Download and install the package.

If you already have Anaconda for Python 2 installed, you can create a separate environment for Python 3 and all the appropriate dependencies with the following command:

`>  conda create --name=yourNewEnvironment python=3 anaconda`

`>  source activate yourNewEnvironment`

**Step 2:** Installing OpenCV

Once you have Anaconda installed, first double check you are in your Python 3 environment:

`>python`    
`Python 3.5.2 |Anaconda 4.1.1 (x86_64)| (default, Jul  2 2016, 17:52:12)`  
`[GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin`  
`Type "help", "copyright", "credits" or "license" for more information.`  
`>>>`   
(Ctrl-d to exit Python)

run the following commands at the terminal prompt to get OpenCV:

`> pip install pillow`  
`> conda install -c menpo opencv3=3.1.0`

then to test if OpenCV is installed correctly:

`> python`  
`>>> import cv2`  
`>>>`  (i.e. did not get an ImportError)

(Ctrl-d to exit Python)

**Step 3:** Installing moviepy  

We recommend the "moviepy" package for processing video in this project (though you're welcome to use other packages if you prefer).  

To install moviepy run:

`>pip install moviepy`  

and check that the install worked:

`>python`  
`>>>import moviepy`  
`>>>`  (i.e. did not get an ImportError)

(Ctrl-d to exit Python)

**Step 4:** Opening the code in a Jupyter Notebook

You will complete this project in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, run the following command at the terminal prompt (be sure you're in your Python 3 environment!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.  

**Step 5:** Complete the project and submit both the Ipython notebook and the project writeup

