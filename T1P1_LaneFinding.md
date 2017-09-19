# **学期1题目1: 寻找车道线程序** 

**Finding Lane Lines on the Road**

第一项功课是写一个简单在视频帧照片寻找车道线的程序。因为帧与帧间没有续编物体,
这是个比较基本计算机视觉上的练习.

<table class="image">
<caption align="bottom">目标</caption>
<tr><td><img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_images/post_hough/solidYellowCurve.jpg?raw=true" width="400"/></td></tr>
</table>

---

## Goal 目标
1. 目标是从几幅像以上原图加上车道线（红）
2. 加完后在视频上运用

## Outline 大概解法与教材
1. 运用高斯模糊程序(Gaussian Blur)， 把所有不是厚黄白线的物体混淆(车山树云之类）
	- Computerphile (过滤程序基本) https://youtu.be/C_zFhWdM4ic
2. 把图色变成黑白(grayscale)。像素(pixel)黑白烈度是用0-255数字代表， 转了之后可以运用过滤程序，通常是一些矩阵运算(matrix operation)
3. 运用一下过滤程序前，需要缩小检测区域(region)
4. Canny边界检测(Canny Edge Detection):随处色差梯度巨变就定义是边界
	- Computerphile (Sobel算子和边界检测基本) https://youtu.be/uihBwtPIBxM
	- Computerphile (Canny边界检测) https://youtu.be/sRFM5IEqR2w
5. Hough直线转变(Hough Line Transform):这是功课里最复杂的程序，目标是把多半是直线的边界的方程算出来。 其实逻辑挺简单：把每一条通过边缘像素的线性方程(linear equation)算出来，如果某边界真代表一条直线的话，那么就一定存在一个线性方程，使得其线会通过边界缘上图里其他像素。而且线上所有这样算出来的方程都会有同一坡度。这样就能把直线鉴定和把相当方程算出来。
－（How Hough Transform works）https://youtu.be/4zHbI-fFIlI
6. 最后步骤是把Hough算出来的直线方程外推延长。也可以吧大半不是车道线的方程丢弃。如果有多条直段的话，就要平均结果。

<table class="image">
<caption align="bottom">Canny边缘检测</caption>
<tr><td><img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_images/post_canny/challenge7.jpg?raw=true" width="400"/></td></tr>
</table>

## Result 成果
<table class="image">
<caption align="bottom">Result</caption>
<tr><td><img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_videos_output/output.gif?raw=true" width="400"/></td></tr>
</table>

## Lessons 分享经验
- Reading in an image 图像输入:用的是`matplotlib.image`object
	```python
	import matplotlib.pyplot as plt
	import matplotlib.image as mpimg
	```
	```python
	#reading in an image
	image = mpimg.imread('test_images/solidYellowCurve.jpg')
	#printing out some stats and plotting
	print('This image is:', type(image), 'with dimensions:', image.shape)
	plt.imshow(image)  # if you wanted to show a single color channel image called 'gray', for example, call as plt.imshow(gray, cmap='gray')
	```
- Parameter selection:
	- Guassian Blur:因为正态分布正负对称，`kernel_size`一定要是单数。我用的是11。
	- Canny:目标是把车道线变色的区域清楚的应出来，我用
		```python
		result = canny(result, 15, 80)
		```
	- Hough(`Google: cv2 hough`):
		```python
		result = hough_lines(result, 1,np.pi/180,15,15,50)
		```
	- Region Selection:我缩小了区域两次，Canny之前一次，之后一次。
		- 第一次把大概区域分清
		- Canny会把区域边界也应出来，因为不想Hough程序在这也画条线，所以区域要再缩小一点
		－ 区域是个梯形，坐标要用照片圆周算出 (`Google: stackoverflow polygon fillpoly cv2`)
		```python
		trapezoid = np.array([ [0,img_h], [img_w,img_h], [img_w/2+img_w/32,round(img_h*0.5,0)], [img_w/2-img_w/32,round(img_h*0.5,0)] ], np.int32)
    	result = region_of_interest(result, [trapezoid])
    	```
- No Hough lines detected:最后一步是要吧`draw_lines`函数改进，如果Hough没算出任何线的话要不理。
- Create snapshots of `challenge.mp4`:`ffmpeg`在视频方面能说是救命工具。功课里最后挑战`challenge.mp4`,难度大因为视频里多树影跟路色多变使得寻找车道线困难。第一步最好把几幅帧推出来(`Google: ffmpeg snapshot mp4`)：
	```bash
	ffmpeg -ss 00:00:01 -i challenge.mp4 -vframes 1 -q:v 2 challenge1.jpg
	```

- Create gifs of videos:`ffmpeg`也能把`mp4`转成`gif`.`Google: ffmpeg mp4 gif`

## Shortcomings 建议解决方案缺点
Shakiness 颤抖:震动原因是因为有些帧测出来的边界数量不够，可以用一些移动平均的方法，或者Bayes Rule贝叶斯定理)稳定下来。

## Improvements 改进建议
最重要是在Canny步骤能把更多车道线分清就最好，否则就要使用稳定程序。但这不能保证在所有天气光阴条件下测到车线。

## Ideas 远景
- 可以用类似方法把数据图表里数据推算出来
- 愚笨估计OCR就是这种像素上的检测加上机器学习吧

## Resources
- https://github.com/frankkanis/CarND-Student-Blogs
- http://darienmt.com/self-driving-cars/2017/05/30/self-driving-car-nanodegree-term-1.html
