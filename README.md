

---
###Beshari Project Write Up

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for colour selection of obstacles and rock samples.


![example_grid1](https://user-images.githubusercontent.com/6395647/31848397-ff5eaba2-b5ff-11e7-8a74-85fe79aaf319.jpg)
![example_rock1](https://user-images.githubusercontent.com/6395647/31848399-ff774252-b5ff-11e7-9e4c-6ff3231e4a57.jpg)

*Fig1: Test Images*


A new function was added to the notebook:

~~~python
def color_thresh_rock(img, rgb_thresh=(20, 0, 10)):
    # Create an array of zeros same xy size as img, but single channel
    color_select = np.zeros_like(img[:,:,0])
    # Require that each pixel be above all three threshold values in RGB
    # above_thresh will now contain a boolean array with "True"
    # where threshold was met
    above_thresh = (img[:,:,0] > rgb_thresh[0]) \
                & (img[:,:,1] > rgb_thresh[1]) \
                & (img[:,:,2] < rgb_thresh[2])
    # Index the array of zeros with the boolean array and set to 1
    color_select[above_thresh] = 1
    # Return the binary image
    return color_select
~~~
the function is used to identify rocks as shown below:
the obstacle thresholded image is produced by the ==numpy.invert()== method.

![rock](https://user-images.githubusercontent.com/6395647/31848404-0658ae58-b600-11e7-824a-99726c657a15.jpg)

*Fig2: a Rock image after applying the `color_thresh_rock` world-map*
#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a world-map.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

![general overview](https://user-images.githubusercontent.com/6395647/31848403-064b2a4e-b600-11e7-9257-7becd3699d98.png)

*Fig3: The process of processing the image to steering to the right direction*


and we got a movie result (Please see in the Output Folder named **test_mapping**


### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

####perception.py

the perception step has been populated and modified accordingly, the code is very close to the Jupyter Notebook part in the ==process_image(img)== function

the only differences are receiving the image from `Rover.img`

the image then is perspective-transformed using the ==perspect_transform(img, src, dst)== function

This Produced an image from an upward view with the origin point(0,0) in both (x,y) axis in the upper left corner. as shown in the 2nd picture of Fig3


the image is then thresholded, the light pixels of the image above a certain threshold is transformed to white and what is not is transformed black as shown 3rd image of Fig3

the image is transformed to Robot Local coordinates and the average distance and average angles of all pixels (in a 2D binary array) is calculated in polar coordinates 

The robot then is able to receive and make action accordingly.

Along the process, the terrain, obstacle and rock binary images are produced and become the input of the RGB channels of the robot class, then shown on the left of the screen,

The terrain, obstacle and rock binary images then are scaled down and transformed with the data of the Robot (Yaw and the rover's x,y coordinates) to world coordinates and compared with the original image

####the Decision Step

the Throttle quantity was changed to be explicitly numerical and higher than the default, the Robot therefore is able to navigate the environment faster. A name variable was added to the Robot class.

**the data pipeline** goes as: 

 Images were perspective-transformed first, then thresholded, thirdly, the pixels were rotated in perspective of the Robot, fourthly, angles and distances of the pixels are calculated in polar coordinates based on the terrain thresholded, warped picture, and finally the picture is mapped on the world map

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  


![output](https://user-images.githubusercontent.com/6395647/31848405-0947b398-b600-11e7-8249-a08c6fabc56b.jpeg)
The results were satisfactory: a lot could be done better, however, The robot (has a name in the code"RoboBesh") navigates autonomously with very little assistance manually and able to "see", map the world and locate rocks

Better Algorithm could be used in the decision and manoeuvring analysis. If I would pursue this project further, I would write an algorithm that splits the picture in two, left and right and have the robot crawl the walls.

I would also not have the robot to go to pre-visited places where they have been mapped already.

I would also improve the algorithm where the RoboBesh could see and go, when he is not moving physically (Stuck) to do a getting-out manoeuvre from where he is stuck.
**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Display Setting: 1280 X 1024 on fast fps




