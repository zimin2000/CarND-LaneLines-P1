# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps. 

#### 1) Conver image to "canny image"
That means the following steps:
  * convert image to grayscale
  * apply gauss bluring (with kernel of size 7)
  * apply Canny edge detection (with 80 and 150 as the low and high thresholds)
    
#### 2) Select a tropeziod that is to capture the road
Remove all the other parts of the image but for the road trapezioid. The trapezioit is computed using the left and right bottom notes and the center. Thise points are computed from the size of the picture assuming the following:
  * the center is fixed at the middle of the piture along x axis and 5% below the center along y axis.
  * ignore the 10% from the left and right on the x axis
  * ignore the 20% of the area around the center (as there is no reliable image there anyway).

#### 3) Find lines
To find the lines the polar version of the Hough Transform is used with the following parameters:
```
    rho          = 2        # distance resolution in pixels of the Hough grid
    theta        = np.pi/90 # angular resolution in radians of the Hough grid
    threshold    = 15       # minimum number of votes (intersections in Hough grid cell)
    min_line_len = 70       # minimum number of pixels making up a line
    max_line_gap = 200      # maximum gap in pixels between connectable line segments
```    

####  4) Clustering the identified lines
4.1) The lines identified on the previous step get clustered at 2 largest clasters (assuming these are the lest and right lane lines). While doing this:
  * the lines that are directed in a wrong way get ignored.
  * the lines belonging to a cluster are resampled with the proportion of its length.

4.2) After then a stright line gets fit through the samples points.

4.3) The crossing point of the detected lines get computed and 

4.4) lines are drown atop of the image.

### 2. Identify potential shortcomings with your current pipeline

1. The pipeline assumes the center location in a fixed point that is not neceserely true (e.g. in case of hills of in case of a turn).

2. The pipeline is relaying on a single image frame to detect the lane lines and sometimes is very problematic, so as the result the lane times are not always correctly detected.


### 3. Suggest possible improvements to your pipeline

1. Reuse the information from previous frames to overcome temporary loosing the lane.