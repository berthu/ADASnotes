# **学期1题目1: 寻找车道线程序** 

**Finding Lane Lines on the Road**

第一项功课是写一个简单在视频帧照片寻找车道线的程序。因为帧与帧间没有续编物体,
这是个比较基本计算机视觉上的练习.
<p>
<img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_images/post_hough/solidYellowCurve.jpg?raw=true" width="400">
<em>image_caption</em>
</p>
---

## Goal 目标
1. 目标是从几副像以上原图加上车道线（红）
2. 加完后在视频上运用

## Outline 大概解法与教材
1. 运用高斯模糊程序(Gaussian Blur)， 把所有不是厚黄白线的物体混淆(车山树云之类）
	- Computerphile (过滤程序基本) https://youtu.be/C_zFhWdM4ic
2. 把图色变成黑白(grayscale)。像素(pixel)黑白烈度是用0-255数字代表， 转了之后可以运用过滤程序，通常是一些矩阵运算(matrix operation)
3. 运用一下过滤程序前，需要缩小检测区域(region)
4. Canny边缘检测(Canny Edge Detection): 随处色差梯度巨变就定义是边缘
	- Computerphile (Sobel算子和边缘检测基本) https://youtu.be/uihBwtPIBxM
	- Computerphile (Canny边缘检测) https://youtu.be/sRFM5IEqR2w
<img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_images/post_canny/challenge7.jpg?raw=true" width="400">
5. Hough直线转变(Hough Line Transform): 这是功课里最复杂的程序，目标是把多半是直线的边缘的方程算出来。 其实逻辑挺简单：把每一条通过边缘像素的线性方程(linear equation)算出来，如果某边缘真代表一条直线的话，那么就一定存在一个线性方程，使得其线会通过边缘上图里其他像素。而且线上所有这样算出来的方程都会有同一坡度。这样就能把直线鉴定和把相当方程算出来。
－（How Hough Transform works）https://youtu.be/4zHbI-fFIlI
6. 最后步骤是把Hough算出来的直线方程外推延长。也可以吧大半不是车道线的方程丢弃。如果有多条直段的话，就要平均结果。

## Lessons 分享经验
- 

## Shortcomings 建议解决方案缺点
## Improvements 改进建议
## Ideas 远景


### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
