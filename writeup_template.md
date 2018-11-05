## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./output_images/distortion.jpg "Undistorted"
[image2]: ./test_images/test2.jpg "Road Transformed"
[image3]: ./output_images/binary.jpg "Binary Example"
[image4]: ./output_images/War.jpg "Warp Example"
[image5]: ./output_images/polyfit.jpg "Fit Visual"
[image6]: ./output_images/radCur.jpg "Output"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I strated by operation on 20 distorted chess board images  to get correct and accurate  mtx, dist parameters 
then I used them later on my images to undistort 

this is done by camera_calibration function in my code
![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
the screenshot contains one of the chessboards images before and after distortion correction 


![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image  
- I converted the image to another color space to get rid of brightness and shadows effect and used gradient to detect vertical edges
this is done by color_gradient function in my code

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

I defined two numpy arrays one of the source and one for desitination 
- I get the points for source array by choosing the beginings of lane lines and ends in the photo something like applying region mask before
- then I get the destination points to make the points form rectangle 
- then I applied perspective transform by using a funvtion from opencv library
 This section is done by two seprate cells in the middle of the code to define the source and the destination arrays 

```python
src = np.float32(
    [[230,703],
    [588,454],
    [691,454],
    [1056,690]])
dst = np.float32(
    [[230,703],
    [230,0],
    [1056,0],
    [1056,690]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 230,703    | 230,703      | 
| 588,454     | 230,0    |
| 691,454   |1056,0     |
| 1056,690     |1056,690      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
1) first get the histogram to find the peaks of ehite pixels to use it as starting point 
2) I made windows starting from this position and updated them cope with the position of the pixels 
3) from this I get the right and left lanes pixels into seprate arrays 
4) then applied a function to fit in  with the pixels position 

this is done by the function fit_polynomial in the code 

note : i haven't colered the lanes in the image below as i didn't need this in my project




![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

-first I used a mathematical formula provieded in lesson for calculating the radius of curvature for the nearest pixel in the left 
lane as it represent the nearest to the car
- then I transformed the radius from pixel space to real space to get it in meters

this is done by  measure_curvature_real function in the code

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in my code in the cells below the functions cells Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Issues faced :
the project was not hard to implement as it's steps was disscussed briefly in the lessons but something that took some search for me is how to write a text on an image to write the radius of curvature but it wasn't so hard really.

some little distribance happens at far pixels in the curved lanes I think tracking the frames will solve the problem
