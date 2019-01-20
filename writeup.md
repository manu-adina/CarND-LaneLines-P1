# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.

1. Converted the image to grayscale
1. Applied gaussian blur to the image to reduce the noise for the Canny algorithm 
2. Canny to extract the edges in the image
3. Used region selection to isolate the lane lines.
5. Applied HoughLinesP function to detect straight lines in the selected region
6. Added weighted lines to the image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by,
1. Iterated through the lines output by the Hough algorithm
2. Separated the left and right lines by the sign of the gradient. Left lines had neg. gradient and right lines had pos. gradient.
    - After testing the pipeline on the solidYellowLeft.mp4, I noticed that the horizontal lines were skewing my average. So I had to filter out the horizontal lines by limiting the gradient. Left lines were counted when the gradient was below -0.5, and right lines only counter when grad. > 0.5.
    - Also found that gradient wasn't sufficient, some lines on the left had a positive gradient (so they counted as a right line). Hence it skewed the average. These lines needed to be filtered out. Hence, the left lines needed to be on the left hand side of the image (x1 < 500) and right lines needed to be on the right hand (x1 > 500) side of the image to count.
2. Iteratively found the average of left and right lines.
3. Used the found averages of x1, y1, x2, and y2 for left and right lines to determine the gradient and intercept of the average right and left line (using the np.polyfit).
 - Found that sometimes the hough edges weren't picked up. As a result it would cause an error. Hence, global params were used so that if the right or left average lines weren't found, it would use the previous average lines.


### 2. Identify potential shortcomings with your current pipeline

- The left and right lanes jitter a lot, so the Hough params need to be tuned more so that the lanes are more smooth.

- The curvature isn't recognised. The detected lanes are perceived as straight.

- Sometimes, Hough edges don't pick up for left or right side. Even though a lane is visible in the image.


### 3. Suggest possible improvements to your pipeline

- The Hough params can be tuned more to reduce the jitter. Also Hough params could be set up so that it is more sensitive, to prevent the situations where nothing is picked up.

- Curvature is hard to solve due to the Hough algorithm being designed for straight lines.
