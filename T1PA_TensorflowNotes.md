# **TensorFlow, Deep Learning 入门** 

Tensorflow是负责简化机器学习程序的一整套软件，

---

## Goal 目标
－ 大概了解深度学习(Deep Learning)的主要重点和用Tensorflow写点简单程序。 
- https://zh.wikipedia.org/wiki/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0

## Outline 大概解法与教材
1. TensorFlow 安装
	```bash
	conda info --env
	conda create -n tensorflow python=3.6
	source activate tensorflow
	conda install tensorflow
	conda install jupyter notebook
	```
2. 或者，`Google carND Tensorflow Lab`
	- Bug in TensorFlowLab, `Google: No module named ipykernel_launcher
	- Solution:
	```bash
	python -m pip uninstall ipykernel
	python -m pip install ipykernel
	```
3. 视频
	－ https://www.youtube.com/watch?v=2FmcHiLCwTU
4. 教程
	－ https://www.tensorflow.org/get_started/mnist/beginners
5. 查看Dataset Size用｀mnist.train.images.shape｀
6. Tensorflow Lab
	- Train之前要先Normalize Data，比如用min-max scaling
	- *Epoch*: 1个Epoch是指所有数据用过一次
	- *Batch Size*: 批量,一次iteration用多少数据
	- *Learning Rate*: 汇集速度
	- *Gradient Descent*: 随机梯度下降法求解
	- https://stackoverflow.com/questions/4752626/epoch-vs-iteration-when-training-neural-networks
	- "Example: if you have 1000 training examples, and your batch size is 500, then it will take 2 iterations to complete 1 epoch."
7. Hvass Laboratories 视频: https://www.youtube.com/watch?v=wuo4JdG3SvU
	- Tutorial: https://github.com/Hvass-Labs/TensorFlow-Tutorials
	- 中文版: https://github.com/thrillerist/TensorFlow-Tutorials

##最基本程序
```python
import tensorflow as tf
import numpy as np
from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("/tmp/data/", one_hot=True)
# TF graph input
x = tf.placeholder("float", [None, 784])

# Create a model
# Set model weights
W = tf.Variable(tf.zeros([784, 10]))
b = tf.Variable(tf.zeros([10]))
# WX + b
y = tf.nn.softmax(tf.matmul(x,W) + b) #softmax
y_ = tf.placeholder("float", [None, 10])

# reduction_indices adds the elements in the second dimension of y
cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))
# GDO takes learning_rate
train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)

sess = tf.InteractiveSession()
tf.global_variables_initializer().run()
for _ in range(1000):
    batch_xs, batch_ys = mnist.train.next_batch(100)
    sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})
correct_prediction = tf.equal(tf.argmax(y,1), tf.argmax(y_,1))
accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
print(sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels}))

```