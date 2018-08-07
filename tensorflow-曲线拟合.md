# tensorflow 曲线拟合

Python代码：

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
# from tensorflow.examples.tutorials.mnist import input_data

# creating data
mu,sigma=0, 0.1
data_size = 300
x_data = np.linspace(-1, 1,data_size)[:, np.newaxis]
# noise = np.random.normal(0,0.05, x_data.shape)
y_data = np.sign(x_data)

# mnist data
# mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
# x_data, y_data = mnist.train.next_batch(300)

# input layer
xs = tf.placeholder(tf.float32, [None, 1])
ys = tf.placeholder(tf.float32, [None, 1])

# layer function
def layer(data_in, size, func = None):
    w = tf.Variable(tf.random_normal(size))
    b = tf.Variable(tf.zeros([1, size[1]]))
    z = tf.matmul(data_in, w) + b
    if(func):
        data_out = func(z)
    else:
        data_out = z
    return data_out

# hidden layer
output1 = layer(xs, [1, 10], tf.nn.relu)
output2 = layer(output1, [10, 20], tf.nn.softmax)
output3 = layer(output2, [20, 20], tf.nn.relu)
output4 = layer(output3, [20, 10], tf.nn.softmax)
output5 = layer(output4, [10, 10], tf.nn.relu)

# output layer
out = layer(output5, [10, 1])

# loss function
# loss = tf.reduce_sum(ys * tf.log(out))
loss = tf.reduce_mean(tf.reduce_sum(tf.square((out - ys))))

# trainning method
# train = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
train = tf.train.AdamOptimizer().minimize(loss)

# init all variables
init = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init)

# print loss value for every 50 times loop
print_step = 50
# loop less than 50 * 1000
loop_max_count = 1000
while True:
    print_step -= 1
    _,loss_value = sess.run([train,loss],feed_dict={xs:x_data,ys:y_data})
    if(print_step == 0):
        print(loss_value)
        print_step = 50
        loop_max_count -= 1 
    if(loss_value < .00001 or loop_max_count <= 0):
        break

# print loop times and show the output
print("loop_count = ", (1000 - loop_max_count) * 50)
y_out = sess.run(out, feed_dict={xs:x_data})
plt.plot(x_data, y_out, label="out")
plt.plot(x_data, y_data, label="in")
plt.show()
```

**可以用来看看不同数目的*隐含层*和不同的*激活函数*对曲线拟合的训练*性能*和训练*结果*有何影响。**