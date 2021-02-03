# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Detect lanes on the road that vary in color and orientation
* Detect lanes on the road that have shadows and varying road colors


[//]: # (Image References)
[image1]: ./test_images/solidWhiteRight.jpg "Example input image"
[image2]: ./test_images_with_lines/formatted-solidWhiteRight.jpg "Example input image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Here is an example input image. This image could be a frame from a video being streamed by a camera on a driving car.


![alt text][image1]


My pipeline consisted of 5 steps:

1. Convert input image to gray scale:
2. Apply gaussian blur with a kernel size of 5
3. Apply canny transform with low and high thresholds set to 1 and 110 respectively
4. Create a polygon mask around the area of focus
5. Apply hough transform with rho set to 1, theta set to 1 radian, threshold set to 15, min line length set to 1 and max line gap set to 100
6. Generated a weighted image which overlays lines outputted from hough transform over the original image

The final result looks like:
![alt text][image2]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

1. Calculate the slop of the vectors outputted by hough transform
2. If the slope is negative you know it's the left lane. If posititive it's the right lane
3. The most difficult part was creating a full line, instead of segments:
  - This was achieved by averaging out the slopes and hardcoding the y1 and y2
  for each lane vector. x1 and x2 were calculated using the slopes and intercepts.
  I do not believe this will work for curved lanes.

### 2. Identify potential shortcomings with your current pipeline

1. After testing with the challenge video which was comprised of shadows and a few frames where the road changed
  color when driving over a bridge, it is safe to say the current pipeline cannot handle shadows which add lines to the scene
  that are picked up by the hough transform algorithm.
2. Also if the road was curved, ex. going around a round about, the lane lines would cross over each other. The current solution
  is taking a linear approach, when lanes are not really linear. A curve patch made up of vectors needs to be determined in order
  to create a path of a lane that is not just straight.

### 3. Suggest possible improvements to your pipeline

1. Handling curved lanes
2. Being able to filter out shadows
3. Have the image mask be dynamic. If the car goes over a bump, the car should use a sensor (ex. gyro) to recalibrate,
  by adjusting the mask polygon to always stay focused on the road
4. Right now the solution only works for a front facing camera. It would be ideal to create an algorithm that takes
  the camera position, rotation and focal length as inputs. And then dynamically adjusts the mask polygon to detech lanes.
