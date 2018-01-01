# **学期1题目1: 寻找车道线程序** 

**Finding Lane Lines on the Road**

第一项功课是写一个简单在视频帧照片寻找车道线的程序。因为帧与帧间没有续编物体,
这是个比较基本计算机视觉上的练习.

<table class="image">
<caption align="bottom">目标</caption>
<tr><td><img src="https://github.com/berthu/CarND-LaneLines-P1/blob/master/test_images/post_hough/solidYellowCurve.jpg?raw=true" width="400"/></td></tr>
</table>


- https://www.youtube.com/watch?v=2FmcHiLCwTU
- Batch size: https://stackoverflow.com/questions/41175401/what-is-a-batch-in-tensorflow
- Epochs: https://stackoverflow.com/questions/38000189/is-it-ok-to-only-use-one-epoch

---

## Goal 目标

## Outline 大概解法与教材
1. TensorFlow Lab
```bash
conda info --env
conda create -n tensorflow python=3.6
source activate tensorflow
conda install -c conda-forge tensorflow
```


## Lessons 分享经验
- Bug in TensorFlowLab, `Google: No module named ipykernel_launcher
	- Solution:
	```bash
	python -m pip uninstall ipykernel
	python -m pip install ipykernel
	```



## Shortcomings 建议解决方案缺点

## Improvements 改进建议

## Ideas 远景

## Resources
- https://github.com/frankkanis/CarND-Student-Blogs
- http://darienmt.com/self-driving-cars/2017/05/30/self-driving-car-nanodegree-term-1.html
