
# Convolutional Networks
So far we have worked with deep fully-connected networks, using them to explore different optimization strategies and network architectures. Fully-connected networks are a good testbed for experimentation because they are very computationally efficient, but in practice all state-of-the-art results use convolutional networks instead.

First you will implement several layer types that are used in convolutional networks. You will then use these layers to train a convolutional network on the CIFAR-10 dataset.


```python
# As usual, a bit of setup
from __future__ import print_function
import numpy as np
import matplotlib.pyplot as plt
from cs231n.classifiers.cnn import *
from cs231n.data_utils import get_CIFAR10_data
from cs231n.gradient_check import eval_numerical_gradient_array, eval_numerical_gradient
from cs231n.layers import *
from cs231n.fast_layers import *
from cs231n.solver import Solver

%matplotlib inline
plt.rcParams['figure.figsize'] = (10.0, 8.0) # set default size of plots
plt.rcParams['image.interpolation'] = 'nearest'
plt.rcParams['image.cmap'] = 'gray'

# for auto-reloading external modules
# see http://stackoverflow.com/questions/1907993/autoreload-of-modules-in-ipython
%load_ext autoreload
%autoreload 2

def rel_error(x, y):
  """ returns relative error """
  return np.max(np.abs(x - y) / (np.maximum(1e-8, np.abs(x) + np.abs(y))))
```


```python
# Load the (preprocessed) CIFAR10 data.

data = get_CIFAR10_data()
for k, v in data.items():
  print('%s: ' % k, v.shape)
```

    X_train:  (49000, 3, 32, 32)
    X_val:  (1000, 3, 32, 32)
    y_test:  (1000,)
    y_train:  (49000,)
    X_test:  (1000, 3, 32, 32)
    y_val:  (1000,)


# Convolution: Naive forward pass
The core of a convolutional network is the convolution operation. In the file `cs231n/layers.py`, implement the forward pass for the convolution layer in the function `conv_forward_naive`. 

You don't have to worry too much about efficiency at this point; just write the code in whatever way you find most clear.

You can test your implementation by running the following:


```python
x_shape = (2, 3, 4, 4)
w_shape = (3, 3, 4, 4)
x = np.linspace(-0.1, 0.5, num=np.prod(x_shape)).reshape(x_shape)
w = np.linspace(-0.2, 0.3, num=np.prod(w_shape)).reshape(w_shape)
b = np.linspace(-0.1, 0.2, num=3)

conv_param = {'stride': 2, 'pad': 1}
out, _ = conv_forward_naive(x, w, b, conv_param)
correct_out = np.array([[[[-0.08759809, -0.10987781],
                           [-0.18387192, -0.2109216 ]],
                          [[ 0.21027089,  0.21661097],
                           [ 0.22847626,  0.23004637]],
                          [[ 0.50813986,  0.54309974],
                           [ 0.64082444,  0.67101435]]],
                         [[[-0.98053589, -1.03143541],
                           [-1.19128892, -1.24695841]],
                          [[ 0.69108355,  0.66880383],
                           [ 0.59480972,  0.56776003]],
                          [[ 2.36270298,  2.36904306],
                           [ 2.38090835,  2.38247847]]]])

# Compare your output to ours; difference should be around 2e-8
print('Testing conv_forward_naive')
print('difference: ', rel_error(out, correct_out))
```

    Testing conv_forward_naive
    difference:  2.2121476575931688e-08


# Aside: Image processing via convolutions

As fun way to both check your implementation and gain a better understanding of the type of operation that convolutional layers can perform, we will set up an input containing two images and manually set up filters that perform common image processing operations (grayscale conversion and edge detection). The convolution forward pass will apply these operations to each of the input images. We can then visualize the results as a sanity check.


```python
from scipy.misc import imread, imresize

kitten, puppy = imread('kitten.jpg'), imread('puppy.jpg')

# kitten is wide, and puppy is already square
d = kitten.shape[1] - kitten.shape[0]
kitten_cropped = kitten[:, d//2:-d//2, :]

img_size = 200   # Make this smaller if it runs too slow
x = np.zeros((2, 3, img_size, img_size))
x[0, :, :, :] = imresize(puppy, (img_size, img_size)).transpose((2, 0, 1))
x[1, :, :, :] = imresize(kitten_cropped, (img_size, img_size)).transpose((2, 0, 1))

# Set up a convolutional weights holding 2 filters, each 3x3
w = np.zeros((2, 3, 3, 3))

# The first filter converts the image to grayscale.
# Set up the red, green, and blue channels of the filter.
w[0, 0, :, :] = [[0, 0, 0], [0, 0.3, 0], [0, 0, 0]]
w[0, 1, :, :] = [[0, 0, 0], [0, 0.6, 0], [0, 0, 0]]
w[0, 2, :, :] = [[0, 0, 0], [0, 0.1, 0], [0, 0, 0]]

# Second filter detects horizontal edges in the blue channel.
w[1, 2, :, :] = [[1, 2, 1], [0, 0, 0], [-1, -2, -1]]

# Vector of biases. We don't need any bias for the grayscale
# filter, but for the edge detection filter we want to add 128
# to each output so that nothing is negative.
b = np.array([0, 128])

# Compute the result of convolving each input in x with each filter in w,
# offsetting by b, and storing the results in out.
out, _ = conv_forward_naive(x, w, b, {'stride': 1, 'pad': 1})

def imshow_noax(img, normalize=True):
    """ Tiny helper to show images as uint8 and remove axis labels """
    if normalize:
        img_max, img_min = np.max(img), np.min(img)
        img = 255.0 * (img - img_min) / (img_max - img_min)
    plt.imshow(img.astype('uint8'))
    plt.gca().axis('off')

# Show the original images and the results of the conv operation
plt.subplot(2, 3, 1)
imshow_noax(puppy, normalize=False)
plt.title('Original image')
plt.subplot(2, 3, 2)
imshow_noax(out[0, 0])
plt.title('Grayscale')
plt.subplot(2, 3, 3)
imshow_noax(out[0, 1])
plt.title('Edges')
plt.subplot(2, 3, 4)
imshow_noax(kitten_cropped, normalize=False)
plt.subplot(2, 3, 5)
imshow_noax(out[1, 0])
plt.subplot(2, 3, 6)
imshow_noax(out[1, 1])
plt.show()
```

    /Users/decemberlabs/anaconda3/lib/python3.5/site-packages/ipykernel_launcher.py:3: DeprecationWarning: `imread` is deprecated!
    `imread` is deprecated in SciPy 1.0.0, and will be removed in 1.2.0.
    Use ``imageio.imread`` instead.
      This is separate from the ipykernel package so we can avoid doing imports until
    /Users/decemberlabs/anaconda3/lib/python3.5/site-packages/ipykernel_launcher.py:11: DeprecationWarning: `imresize` is deprecated!
    `imresize` is deprecated in SciPy 1.0.0, and will be removed in 1.2.0.
    Use ``skimage.transform.resize`` instead.
      # This is added back by InteractiveShellApp.init_path()
    /Users/decemberlabs/anaconda3/lib/python3.5/site-packages/ipykernel_launcher.py:12: DeprecationWarning: `imresize` is deprecated!
    `imresize` is deprecated in SciPy 1.0.0, and will be removed in 1.2.0.
    Use ``skimage.transform.resize`` instead.
      if sys.path[0] == '':



![png](./ConvolutionalNetworks/output_6_1.png)


# Convolution: Naive backward pass
Implement the backward pass for the convolution operation in the function `conv_backward_naive` in the file `cs231n/layers.py`. Again, you don't need to worry too much about computational efficiency.

When you are done, run the following to check your backward pass with a numeric gradient check.


```python
np.random.seed(231)
x = np.random.randn(4, 3, 5, 5)
w = np.random.randn(2, 3, 3, 3)
b = np.random.randn(2,)
dout = np.random.randn(4, 2, 5, 5)
conv_param = {'stride': 1, 'pad': 1}

dx_num = eval_numerical_gradient_array(lambda x: conv_forward_naive(x, w, b, conv_param)[0], x, dout)
dw_num = eval_numerical_gradient_array(lambda w: conv_forward_naive(x, w, b, conv_param)[0], w, dout)
db_num = eval_numerical_gradient_array(lambda b: conv_forward_naive(x, w, b, conv_param)[0], b, dout)

out, cache = conv_forward_naive(x, w, b, conv_param)
dx, dw, db = conv_backward_naive(dout, cache)

# Your errors should be around 1e-8'
print('Testing conv_backward_naive function')
print('dx error: ', rel_error(dx, dx_num))
print('dw error: ', rel_error(dw, dw_num))
print('db error: ', rel_error(db, db_num))
```

    Testing conv_backward_naive function
    dx error:  1.5742819260583776e-08
    dw error:  1.794673097886394e-10
    db error:  2.584820872913033e-11


# Max pooling: Naive forward
Implement the forward pass for the max-pooling operation in the function `max_pool_forward_naive` in the file `cs231n/layers.py`. Again, don't worry too much about computational efficiency.

Check your implementation by running the following:


```python
x_shape = (2, 3, 4, 4)
x = np.linspace(-0.3, 0.4, num=np.prod(x_shape)).reshape(x_shape)
pool_param = {'pool_width': 2, 'pool_height': 2, 'stride': 2}

out, _ = max_pool_forward_naive(x, pool_param)

correct_out = np.array([[[[-0.26315789, -0.24842105],
                          [-0.20421053, -0.18947368]],
                         [[-0.14526316, -0.13052632],
                          [-0.08631579, -0.07157895]],
                         [[-0.02736842, -0.01263158],
                          [ 0.03157895,  0.04631579]]],
                        [[[ 0.09052632,  0.10526316],
                          [ 0.14947368,  0.16421053]],
                         [[ 0.20842105,  0.22315789],
                          [ 0.26736842,  0.28210526]],
                         [[ 0.32631579,  0.34105263],
                          [ 0.38526316,  0.4       ]]]])

# Compare your output with ours. Difference should be around 1e-8.
print('Testing max_pool_forward_naive function:')
print('difference: ', rel_error(out, correct_out))
```

    Testing max_pool_forward_naive function:
    difference:  4.1666665157267834e-08


# Max pooling: Naive backward
Implement the backward pass for the max-pooling operation in the function `max_pool_backward_naive` in the file `cs231n/layers.py`. You don't need to worry about computational efficiency.

Check your implementation with numeric gradient checking by running the following:


```python
np.random.seed(231)
x = np.random.randn(3, 2, 8, 8)
dout = np.random.randn(3, 2, 4, 4)
pool_param = {'pool_height': 2, 'pool_width': 2, 'stride': 2}

dx_num = eval_numerical_gradient_array(lambda x: max_pool_forward_naive(x, pool_param)[0], x, dout)

out, cache = max_pool_forward_naive(x, pool_param)
dx = max_pool_backward_naive(dout, cache)

# Your error should be around 1e-12
print('Testing max_pool_backward_naive function:')
print('dx error: ', rel_error(dx, dx_num))
```

    Testing max_pool_backward_naive function:
    dx error:  3.27562514223145e-12


# Fast layers
Making convolution and pooling layers fast can be challenging. To spare you the pain, we've provided fast implementations of the forward and backward passes for convolution and pooling layers in the file `cs231n/fast_layers.py`.

The fast convolution implementation depends on a Cython extension; to compile it you need to run the following from the `cs231n` directory:

```bash
python setup.py build_ext --inplace
```

The API for the fast versions of the convolution and pooling layers is exactly the same as the naive versions that you implemented above: the forward pass receives data, weights, and parameters and produces outputs and a cache object; the backward pass recieves upstream derivatives and the cache object and produces gradients with respect to the data and weights.

**NOTE:** The fast implementation for pooling will only perform optimally if the pooling regions are non-overlapping and tile the input. If these conditions are not met then the fast pooling implementation will not be much faster than the naive implementation.

You can compare the performance of the naive and fast versions of these layers by running the following:


```python
from cs231n.fast_layers import conv_forward_fast, conv_backward_fast
from time import time
np.random.seed(231)
x = np.random.randn(100, 3, 31, 31)
w = np.random.randn(25, 3, 3, 3)
b = np.random.randn(25,)
dout = np.random.randn(100, 25, 16, 16)
conv_param = {'stride': 2, 'pad': 1}

t0 = time()
out_naive, cache_naive = conv_forward_naive(x, w, b, conv_param)
t1 = time()
out_fast, cache_fast = conv_forward_fast(x, w, b, conv_param)
t2 = time()

print('Testing conv_forward_fast:')
print('Naive: %fs' % (t1 - t0))
print('Fast: %fs' % (t2 - t1))
print('Speedup: %fx' % ((t1 - t0) / (t2 - t1)))
print('Difference: ', rel_error(out_naive, out_fast))

t0 = time()
dx_naive, dw_naive, db_naive = conv_backward_naive(dout, cache_naive)
t1 = time()
dx_fast, dw_fast, db_fast = conv_backward_fast(dout, cache_fast)
t2 = time()

print('\nTesting conv_backward_fast:')
print('Naive: %fs' % (t1 - t0))
print('Fast: %fs' % (t2 - t1))
print('Speedup: %fx' % ((t1 - t0) / (t2 - t1)))
print('dx difference: ', rel_error(dx_naive, dx_fast))
print('dw difference: ', rel_error(dw_naive, dw_fast))
print('db difference: ', rel_error(db_naive, db_fast))
```

    Testing conv_forward_fast:
    Naive: 0.110068s
    Fast: 0.016488s
    Speedup: 6.675714x
    Difference:  0.0
    
    Testing conv_backward_fast:
    Naive: 22.331175s
    Fast: 0.019612s
    Speedup: 1138.658088x
    dx difference:  9.43434568725122e-12
    dw difference:  4.420587653909754e-13
    db difference:  3.481354613192702e-14



```python
from cs231n.fast_layers import max_pool_forward_fast, max_pool_backward_fast
np.random.seed(231)
x = np.random.randn(100, 3, 32, 32)
dout = np.random.randn(100, 3, 16, 16)
pool_param = {'pool_height': 2, 'pool_width': 2, 'stride': 2}

t0 = time()
out_naive, cache_naive = max_pool_forward_naive(x, pool_param)
t1 = time()
out_fast, cache_fast = max_pool_forward_fast(x, pool_param)
t2 = time()

print('Testing pool_forward_fast:')
print('Naive: %fs' % (t1 - t0))
print('fast: %fs' % (t2 - t1))
print('speedup: %fx' % ((t1 - t0) / (t2 - t1)))
print('difference: ', rel_error(out_naive, out_fast))

t0 = time()
dx_naive = max_pool_backward_naive(dout, cache_naive)
t1 = time()
dx_fast = max_pool_backward_fast(dout, cache_fast)
t2 = time()

print('\nTesting pool_backward_fast:')
print('Naive: %fs' % (t1 - t0))
print('speedup: %fx' % ((t1 - t0) / (t2 - t1)))
print('dx difference: ', rel_error(dx_naive, dx_fast))
```

    Testing pool_forward_fast:
    Naive: 0.449392s
    fast: 0.006171s
    speedup: 72.823320x
    difference:  0.0
    
    Testing pool_backward_fast:
    Naive: 1.868815s
    speedup: 116.814623x
    dx difference:  0.0


# Inline question: 
Which is the key function in the fast implementation of the convolutional layers? What does it do and why does it result in such a big speed-up?

# Answer:
En la version `Naive`, se recorre la imagen con varios for (lo cual es muy lento, mas en python) para poder calcular el producto punto entre cada seccion de la imagen y el filtro mientras que en la version `fast` se consiguen todas las posibles localizaciones en las cuales se van  aplicar los filtros, despues se realiza una unica multiplicacion de matrices paraconseguir el producto punto en cada una de esas localizaciones. Para esto se convierten las imagenes en columnas y para que las imagenes sean compatibles se reordena (reshapeea) el filtro.

# Convolutional "sandwich" layers
Previously we introduced the concept of "sandwich" layers that combine multiple operations into commonly used patterns. In the file `cs231n/layer_utils.py` you will find sandwich layers that implement a few commonly used patterns for convolutional networks.


```python
from cs231n.layer_utils import conv_relu_pool_forward, conv_relu_pool_backward
np.random.seed(231)
x = np.random.randn(2, 3, 16, 16)
w = np.random.randn(3, 3, 3, 3)
b = np.random.randn(3,)
dout = np.random.randn(2, 3, 8, 8)
conv_param = {'stride': 1, 'pad': 1}
pool_param = {'pool_height': 2, 'pool_width': 2, 'stride': 2}

out, cache = conv_relu_pool_forward(x, w, b, conv_param, pool_param)
dx, dw, db = conv_relu_pool_backward(dout, cache)

dx_num = eval_numerical_gradient_array(lambda x: conv_relu_pool_forward(x, w, b, conv_param, pool_param)[0], x, dout)
dw_num = eval_numerical_gradient_array(lambda w: conv_relu_pool_forward(x, w, b, conv_param, pool_param)[0], w, dout)
db_num = eval_numerical_gradient_array(lambda b: conv_relu_pool_forward(x, w, b, conv_param, pool_param)[0], b, dout)

print('Testing conv_relu_pool')
print('dx error: ', rel_error(dx_num, dx))
print('dw error: ', rel_error(dw_num, dw))
print('db error: ', rel_error(db_num, db))
```

    Testing conv_relu_pool
    dx error:  4.397502834267091e-09
    dw error:  3.651673815374511e-09
    db error:  3.721670750819115e-10



```python
from cs231n.layer_utils import conv_relu_forward, conv_relu_backward
np.random.seed(231)
x = np.random.randn(2, 3, 8, 8)
w = np.random.randn(3, 3, 3, 3)
b = np.random.randn(3,)
dout = np.random.randn(2, 3, 8, 8)
conv_param = {'stride': 1, 'pad': 1}

out, cache = conv_relu_forward(x, w, b, conv_param)
dx, dw, db = conv_relu_backward(dout, cache)

dx_num = eval_numerical_gradient_array(lambda x: conv_relu_forward(x, w, b, conv_param)[0], x, dout)
dw_num = eval_numerical_gradient_array(lambda w: conv_relu_forward(x, w, b, conv_param)[0], w, dout)
db_num = eval_numerical_gradient_array(lambda b: conv_relu_forward(x, w, b, conv_param)[0], b, dout)

print('Testing conv_relu:')
print('dx error: ', rel_error(dx_num, dx))
print('dw error: ', rel_error(dw_num, dw))
print('db error: ', rel_error(db_num, db))
```

    Testing conv_relu:
    dx error:  4.84744795054139e-09
    dw error:  3.828316580414188e-10
    db error:  2.9449034603190923e-10


# Three-layer ConvNet
Now that you have implemented all the necessary layers, we can put them together into a simple convolutional network.

Open the file `cs231n/classifiers/cnn.py` and complete the implementation of the `ThreeLayerConvNet` class. Run the following cells to help you debug:

## Sanity check loss
After you build a new network, one of the first things you should do is sanity check the loss. When we use the softmax loss, we expect the loss for random weights (and no regularization) to be about `log(C)` for `C` classes. When we add regularization this should go up.


```python
model = ThreeLayerConvNet()

N = 50
X = np.random.randn(N, 3, 32, 32)
y = np.random.randint(10, size=N)

loss, grads = model.loss(X, y)
print('Initial loss (no regularization): ', loss)

model.reg = 0.5
loss, grads = model.loss(X, y)
print('Initial loss (with regularization): ', loss)
```

    Initial loss (no regularization):  2.3025836107765825
    Initial loss (with regularization):  2.5083878716607613


## Gradient check
After the loss looks reasonable, use numeric gradient checking to make sure that your backward pass is correct. When you use numeric gradient checking you should use a small amount of artifical data and a small number of neurons at each layer. Note: correct implementations may still have relative errors up to 1e-2.


```python
num_inputs = 2
input_dim = (3, 16, 16)
reg = 0.0
num_classes = 10
np.random.seed(231)
X = np.random.randn(num_inputs, *input_dim)
y = np.random.randint(num_classes, size=num_inputs)

model = ThreeLayerConvNet(num_filters=3, filter_size=3,
                          input_dim=input_dim, hidden_dim=7,
                          dtype=np.float64)
loss, grads = model.loss(X, y)
for param_name in sorted(grads):
    f = lambda _: model.loss(X, y)[0]
    param_grad_num = eval_numerical_gradient(f, model.params[param_name], verbose=False, h=1e-6)
    e = rel_error(param_grad_num, grads[param_name])
    print('%s max relative error: %e' % (param_name, rel_error(param_grad_num, grads[param_name])))
```

    W1 max relative error: 1.380104e-04
    W2 max relative error: 1.822723e-02
    W3 max relative error: 3.064049e-04
    b1 max relative error: 3.477652e-05
    b2 max relative error: 2.516375e-03
    b3 max relative error: 7.945660e-10


## Overfit small data
A nice trick is to train your model with just a few training samples. You should be able to overfit small datasets, which will result in very high training accuracy and comparatively low validation accuracy.


```python
np.random.seed(231)

num_train = 100
small_data = {
  'X_train': data['X_train'][:num_train],
  'y_train': data['y_train'][:num_train],
  'X_val': data['X_val'],
  'y_val': data['y_val'],
}

model = ThreeLayerConvNet(weight_scale=1e-2)

solver = Solver(model, small_data,
                num_epochs=15, batch_size=50,
                update_rule='adam',
                optim_config={
                  'learning_rate': 1e-3, 
                },
                verbose=True, print_every=1)
solver.train()
```

    (Iteration 1 / 30) loss: 2.414060
    (Epoch 0 / 15) train acc: 0.190000; val_acc: 0.128000
    (Iteration 2 / 30) loss: 2.609504
    (Epoch 1 / 15) train acc: 0.230000; val_acc: 0.094000
    (Iteration 3 / 30) loss: 2.113380
    (Iteration 4 / 30) loss: 1.971811
    (Epoch 2 / 15) train acc: 0.310000; val_acc: 0.098000
    (Iteration 5 / 30) loss: 1.676728
    (Iteration 6 / 30) loss: 1.801782
    (Epoch 3 / 15) train acc: 0.570000; val_acc: 0.191000
    (Iteration 7 / 30) loss: 1.652683
    (Iteration 8 / 30) loss: 1.598651
    (Epoch 4 / 15) train acc: 0.570000; val_acc: 0.194000
    (Iteration 9 / 30) loss: 1.070849
    (Iteration 10 / 30) loss: 1.408982
    (Epoch 5 / 15) train acc: 0.740000; val_acc: 0.188000
    (Iteration 11 / 30) loss: 0.816042
    (Iteration 12 / 30) loss: 0.807953
    (Epoch 6 / 15) train acc: 0.820000; val_acc: 0.256000
    (Iteration 13 / 30) loss: 0.971160
    (Iteration 14 / 30) loss: 0.568949
    (Epoch 7 / 15) train acc: 0.860000; val_acc: 0.236000
    (Iteration 15 / 30) loss: 0.394380
    (Iteration 16 / 30) loss: 0.401405
    (Epoch 8 / 15) train acc: 0.910000; val_acc: 0.194000
    (Iteration 17 / 30) loss: 0.723469
    (Iteration 18 / 30) loss: 0.258560
    (Epoch 9 / 15) train acc: 0.910000; val_acc: 0.169000
    (Iteration 19 / 30) loss: 0.242372
    (Iteration 20 / 30) loss: 0.235556
    (Epoch 10 / 15) train acc: 0.940000; val_acc: 0.199000
    (Iteration 21 / 30) loss: 0.212628
    (Iteration 22 / 30) loss: 0.126721
    (Epoch 11 / 15) train acc: 0.940000; val_acc: 0.213000
    (Iteration 23 / 30) loss: 0.113127
    (Iteration 24 / 30) loss: 0.270010
    (Epoch 12 / 15) train acc: 0.970000; val_acc: 0.204000
    (Iteration 25 / 30) loss: 0.067624
    (Iteration 26 / 30) loss: 0.081088
    (Epoch 13 / 15) train acc: 1.000000; val_acc: 0.207000
    (Iteration 27 / 30) loss: 0.029606
    (Iteration 28 / 30) loss: 0.051089
    (Epoch 14 / 15) train acc: 1.000000; val_acc: 0.209000
    (Iteration 29 / 30) loss: 0.027549
    (Iteration 30 / 30) loss: 0.025578
    (Epoch 15 / 15) train acc: 1.000000; val_acc: 0.209000


Plotting the loss, training accuracy, and validation accuracy should show clear overfitting:


```python
plt.subplot(2, 1, 1)
plt.plot(solver.loss_history, 'o')
plt.xlabel('iteration')
plt.ylabel('loss')

plt.subplot(2, 1, 2)
plt.plot(solver.train_acc_history, '-o')
plt.plot(solver.val_acc_history, '-o')
plt.legend(['train', 'val'], loc='upper left')
plt.xlabel('epoch')
plt.ylabel('accuracy')
plt.show()
```


![png](./ConvolutionalNetworks/output_28_0.png)


## Train the net
By training the three-layer convolutional network for one epoch, you should achieve greater than 35% accuracy on the training set:


```python
model = ThreeLayerConvNet(weight_scale=0.001, hidden_dim=500, reg=0.001)

solver = Solver(model, data,
                num_epochs=1, batch_size=50,
                update_rule='adam',
                optim_config={
                  'learning_rate': 1e-3,
                },
                verbose=True, print_every=20)
solver.train()
```

    (Iteration 1 / 980) loss: 2.304740
    (Epoch 0 / 1) train acc: 0.103000; val_acc: 0.107000
    (Iteration 21 / 980) loss: 2.132736
    (Iteration 41 / 980) loss: 1.890223
    (Iteration 61 / 980) loss: 1.790478
    (Iteration 81 / 980) loss: 1.745026
    (Iteration 101 / 980) loss: 1.892161
    (Iteration 121 / 980) loss: 1.890149
    (Iteration 141 / 980) loss: 1.964029
    (Iteration 161 / 980) loss: 1.741861
    (Iteration 181 / 980) loss: 1.886438
    (Iteration 201 / 980) loss: 1.958140
    (Iteration 221 / 980) loss: 1.789366
    (Iteration 241 / 980) loss: 1.693459
    (Iteration 261 / 980) loss: 1.507454
    (Iteration 281 / 980) loss: 1.622408
    (Iteration 301 / 980) loss: 1.845511
    (Iteration 321 / 980) loss: 1.737180
    (Iteration 341 / 980) loss: 1.680309
    (Iteration 361 / 980) loss: 1.749498
    (Iteration 381 / 980) loss: 1.420701
    (Iteration 401 / 980) loss: 1.685015
    (Iteration 421 / 980) loss: 1.733135
    (Iteration 441 / 980) loss: 1.591582
    (Iteration 461 / 980) loss: 1.700545
    (Iteration 481 / 980) loss: 1.492935
    (Iteration 501 / 980) loss: 1.404087
    (Iteration 521 / 980) loss: 1.712702
    (Iteration 541 / 980) loss: 1.459696
    (Iteration 561 / 980) loss: 1.607686
    (Iteration 581 / 980) loss: 1.232315
    (Iteration 601 / 980) loss: 1.641778
    (Iteration 621 / 980) loss: 1.560701
    (Iteration 641 / 980) loss: 1.642898
    (Iteration 661 / 980) loss: 1.651685
    (Iteration 681 / 980) loss: 1.799905
    (Iteration 701 / 980) loss: 1.447530
    (Iteration 721 / 980) loss: 1.538806
    (Iteration 741 / 980) loss: 1.893407
    (Iteration 761 / 980) loss: 1.408911
    (Iteration 781 / 980) loss: 2.160169
    (Iteration 801 / 980) loss: 1.850402
    (Iteration 821 / 980) loss: 1.542580
    (Iteration 841 / 980) loss: 1.441510
    (Iteration 861 / 980) loss: 1.779246
    (Iteration 881 / 980) loss: 1.656049
    (Iteration 901 / 980) loss: 1.455208
    (Iteration 921 / 980) loss: 1.820103
    (Iteration 941 / 980) loss: 1.526195
    (Iteration 961 / 980) loss: 1.639982
    (Epoch 1 / 1) train acc: 0.484000; val_acc: 0.485000


## Visualize Filters
You can visualize the first-layer convolutional filters from the trained network by running the following:


```python
from cs231n.vis_utils import visualize_grid

grid = visualize_grid(model.params['W1'].transpose(0, 2, 3, 1))
plt.imshow(grid.astype('uint8'))
plt.axis('off')
plt.gcf().set_size_inches(5, 5)
plt.show()
```


![png](./ConvolutionalNetworks/output_32_0.png)


# Spatial Batch Normalization
We already saw that batch normalization is a very useful technique for training deep fully-connected networks. Batch normalization can also be used for convolutional networks, but we need to tweak it a bit; the modification will be called "spatial batch normalization."

Normally batch-normalization accepts inputs of shape `(N, D)` and produces outputs of shape `(N, D)`, where we normalize across the minibatch dimension `N`. For data coming from convolutional layers, batch normalization needs to accept inputs of shape `(N, C, H, W)` and produce outputs of shape `(N, C, H, W)` where the `N` dimension gives the minibatch size and the `(H, W)` dimensions give the spatial size of the feature map.

If the feature map was produced using convolutions, then we expect the statistics of each feature channel to be relatively consistent both between different imagesand different locations within the same image. Therefore spatial batch normalization computes a mean and variance for each of the `C` feature channels by computing statistics over both the minibatch dimension `N` and the spatial dimensions `H` and `W`.

## Spatial batch normalization: forward

In the file `cs231n/layers.py`, implement the forward pass for spatial batch normalization in the function `spatial_batchnorm_forward`. Check your implementation by running the following:


```python
np.random.seed(231)
# Check the training-time forward pass by checking means and variances
# of features both before and after spatial batch normalization

N, C, H, W = 2, 3, 4, 5
x = 4 * np.random.randn(N, C, H, W) + 10

print('Before spatial batch normalization:')
print('  Shape: ', x.shape)
print('  Means: ', x.mean(axis=(0, 2, 3)))
print('  Stds: ', x.std(axis=(0, 2, 3)))

# Means should be close to zero and stds close to one
gamma, beta = np.ones(C), np.zeros(C)
bn_param = {'mode': 'train'}
out, _ = spatial_batchnorm_forward(x, gamma, beta, bn_param)
print('After spatial batch normalization:')
print('  Shape: ', out.shape)
print('  Means: ', out.mean(axis=(0, 2, 3)))
print('  Stds: ', out.std(axis=(0, 2, 3)))

# Means should be close to beta and stds close to gamma
gamma, beta = np.asarray([3, 4, 5]), np.asarray([6, 7, 8])
out, _ = spatial_batchnorm_forward(x, gamma, beta, bn_param)
print('After spatial batch normalization (nontrivial gamma, beta):')
print('  Shape: ', out.shape)
print('  Means: ', out.mean(axis=(0, 2, 3)))
print('  Stds: ', out.std(axis=(0, 2, 3)))
```

    Before spatial batch normalization:
      Shape:  (2, 3, 4, 5)
      Means:  [9.33463814 8.90909116 9.11056338]
      Stds:  [3.61447857 3.19347686 3.5168142 ]
    After spatial batch normalization:
      Shape:  (2, 3, 4, 5)
      Means:  [ 5.85642645e-16  5.93969318e-16 -8.88178420e-17]
      Stds:  [0.99999962 0.99999951 0.9999996 ]
    After spatial batch normalization (nontrivial gamma, beta):
      Shape:  (2, 3, 4, 5)
      Means:  [6. 7. 8.]
      Stds:  [2.99999885 3.99999804 4.99999798]



```python
np.random.seed(231)
# Check the test-time forward pass by running the training-time
# forward pass many times to warm up the running averages, and then
# checking the means and variances of activations after a test-time
# forward pass.
N, C, H, W = 10, 4, 11, 12

bn_param = {'mode': 'train'}
gamma = np.ones(C)
beta = np.zeros(C)
for t in range(50):
  x = 2.3 * np.random.randn(N, C, H, W) + 13
  spatial_batchnorm_forward(x, gamma, beta, bn_param)
bn_param['mode'] = 'test'
x = 2.3 * np.random.randn(N, C, H, W) + 13
a_norm, _ = spatial_batchnorm_forward(x, gamma, beta, bn_param)

# Means should be close to zero and stds close to one, but will be
# noisier than training-time forward passes.
print('After spatial batch normalization (test-time):')
print('  means: ', a_norm.mean(axis=(0, 2, 3)))
print('  stds: ', a_norm.std(axis=(0, 2, 3)))
```

    After spatial batch normalization (test-time):
      means:  [-0.08034406  0.07562881  0.05716371  0.04378383]
      stds:  [0.96718744 1.0299714  1.02887624 1.00585577]


## Spatial batch normalization: backward
In the file `cs231n/layers.py`, implement the backward pass for spatial batch normalization in the function `spatial_batchnorm_backward`. Run the following to check your implementation using a numeric gradient check:


```python
np.random.seed(231)
N, C, H, W = 2, 3, 4, 5
x = 5 * np.random.randn(N, C, H, W) + 12
gamma = np.random.randn(C)
beta = np.random.randn(C)
dout = np.random.randn(N, C, H, W)

bn_param = {'mode': 'train'}
fx = lambda x: spatial_batchnorm_forward(x, gamma, beta, bn_param)[0]
fg = lambda a: spatial_batchnorm_forward(x, gamma, beta, bn_param)[0]
fb = lambda b: spatial_batchnorm_forward(x, gamma, beta, bn_param)[0]

dx_num = eval_numerical_gradient_array(fx, x, dout)
da_num = eval_numerical_gradient_array(fg, gamma, dout)
db_num = eval_numerical_gradient_array(fb, beta, dout)

_, cache = spatial_batchnorm_forward(x, gamma, beta, bn_param)
dx, dgamma, dbeta = spatial_batchnorm_backward(dout, cache)
print('dx error: ', rel_error(dx_num, dx))
print('dgamma error: ', rel_error(da_num, dgamma))
print('dbeta error: ', rel_error(db_num, dbeta))
```

    dx error:  3.0838468285639314e-07
    dgamma error:  7.09738489671469e-12
    dbeta error:  3.275608725278405e-12


# Experiment!
Experiment and try to get the best performance that you can on CIFAR-10 using a ConvNet. Here are some ideas to get you started:

### Things you should try:
- Filter size: Above we used 7x7; this makes pretty pictures but smaller filters may be more efficient
- Number of filters: Above we used 32 filters. Do more or fewer do better?
- Batch normalization: Try adding spatial batch normalization after convolution layers and vanilla batch normalization aafter affine layers. Do your networks train faster?
- Network architecture: The network above has two layers of trainable parameters. Can you do better with a deeper network? You can implement alternative architectures in the file `cs231n/classifiers/convnet.py`. Some good architectures to try include:
    - [conv-relu-pool]xN - conv - relu - [affine]xM - [softmax or SVM]
    - [conv-relu-pool]XN - [affine]XM - [softmax or SVM]
    - [conv-relu-conv-relu-pool]xN - [affine]xM - [softmax or SVM]

### Tips for training
For each network architecture that you try, you should tune the learning rate and regularization strength. When doing this there are a couple important things to keep in mind:

- If the parameters are working well, you should see improvement within a few hundred iterations
- Remember the course-to-fine approach for hyperparameter tuning: start by testing a large range of hyperparameters for just a few training iterations to find the combinations of parameters that are working at all.
- Once you have found some sets of parameters that seem to work, search more finely around these parameters. You may need to train for more epochs.

### Going above and beyond
If you are feeling adventurous there are many other features you can implement to try and improve your performance. You are **not required** to implement any of these; however they would be good things to try for extra credit.

- Alternative update steps: For the assignment we implemented SGD+momentum, RMSprop, and Adam; you could try alternatives like AdaGrad or AdaDelta.
- Alternative activation functions such as leaky ReLU, parametric ReLU, or MaxOut.
- Model ensembles
- Data augmentation

If you do decide to implement something extra, clearly describe it in the "Extra Credit Description" cell below.

### What we expect
At the very least, you should be able to train a ConvNet that gets at least 60% accuracy on the validation set. This is just a lower bound - if you are careful it should be possible to get accuracies much higher than that! Extra credit points will be awarded for particularly high-scoring models or unique approaches.

You should use the space below to experiment and train your network. The final cell in this notebook should contain the training, validation, and test set accuracies for your final trained network. In this notebook you should also write an explanation of what you did, any additional features that you implemented, and any visualizations or graphs that you make in the process of training and evaluating your network.

You should always explain how you find/set the hyperparamenters/parameters of your model.

### Number of parameters
How many parameters does your final model have? 

Have fun and happy training!


```python
# Train a really good model on CIFAR-10
best_model = None
best_val_acc=0
best_params = []

for i in range(10):
    w_s, h_d, r, lr = [10**np.random.uniform(-5,-1),
                   int(np.random.uniform(100,1000)),
                   10**np.random.uniform(-5,-1),
                   10**np.random.uniform(-5,-1)]
    params = 'Weight Scale: {0} \nHidden dim: {1} \nReg: {2} \nLearning Rate: {3} \n'.format(w_s, h_d, r, lr)
    print(params)
    model  = ThreeLayerConvNet(weight_scale=w_s, hidden_dim=h_d, reg=r)
    solver = Solver(model, data,
                      num_epochs=5, batch_size=100,
                      update_rule='adam',
                      optim_config={
                        'learning_rate':lr
                       },
                     lr_decay=0.95,
                     print_every=100,
                      verbose=True)
    solver.train()
    if solver.best_val_acc>best_val_acc:
        best_model  = model
        best_params = params

    plt.subplot(2, 1, 1)
    plt.title('Training loss')
    plt.xlabel('Iteration')
    plt.plot(solver.loss_history, 'o')

    plt.subplot(2, 1, 2)
    plt.title('Training accuracy VS Validation accuracy')
    plt.xlabel('Epoch')
    plt.plot(solver.train_acc_history, '-o',label='train_acc')
    plt.plot(solver.val_acc_history, '-o',label='val_acc')
    plt.grid(True)
    plt.legend(loc='upper center', ncol=4)
    plt.gcf().set_size_inches(15, 15)
    plt.show()
    
    #0.0009682913656994314, 564, 5.890517019381717e-05
```

    Weight Scale: 4.405668470341841e-05 
    Hidden dim: 502 
    Reg: 0.028524585802001113 
    Learning Rate: 1.461515748587362e-05 
    
    (Iteration 1 / 2450) loss: 2.302699
    (Epoch 0 / 5) train acc: 0.124000; val_acc: 0.139000
    (Iteration 101 / 2450) loss: 2.121917
    (Iteration 201 / 2450) loss: 1.955619
    (Iteration 301 / 2450) loss: 1.989264
    (Iteration 401 / 2450) loss: 1.749800
    (Epoch 1 / 5) train acc: 0.324000; val_acc: 0.345000
    (Iteration 501 / 2450) loss: 1.755122
    (Iteration 601 / 2450) loss: 1.802071
    (Iteration 701 / 2450) loss: 1.744182
    (Iteration 801 / 2450) loss: 1.760463
    (Iteration 901 / 2450) loss: 1.679439
    (Epoch 2 / 5) train acc: 0.414000; val_acc: 0.418000
    (Iteration 1001 / 2450) loss: 1.776117
    (Iteration 1101 / 2450) loss: 1.785868
    (Iteration 1201 / 2450) loss: 1.816420
    (Iteration 1301 / 2450) loss: 1.495592
    (Iteration 1401 / 2450) loss: 1.598099
    (Epoch 3 / 5) train acc: 0.453000; val_acc: 0.444000
    (Iteration 1501 / 2450) loss: 1.811753
    (Iteration 1601 / 2450) loss: 1.624932
    (Iteration 1701 / 2450) loss: 1.524900
    (Iteration 1801 / 2450) loss: 1.560200
    (Iteration 1901 / 2450) loss: 1.478349
    (Epoch 4 / 5) train acc: 0.478000; val_acc: 0.466000
    (Iteration 2001 / 2450) loss: 1.368136
    (Iteration 2101 / 2450) loss: 1.409698
    (Iteration 2201 / 2450) loss: 1.561507
    (Iteration 2301 / 2450) loss: 1.514267
    (Iteration 2401 / 2450) loss: 1.520782
    (Epoch 5 / 5) train acc: 0.487000; val_acc: 0.484000



![png](./ConvolutionalNetworks/output_40_1.png)


    Weight Scale: 0.0002579016749719559 
    Hidden dim: 818 
    Reg: 0.01631863575142174 
    Learning Rate: 0.0001414323386211939 
    
    (Iteration 1 / 2450) loss: 2.306226
    (Epoch 0 / 5) train acc: 0.088000; val_acc: 0.098000
    (Iteration 101 / 2450) loss: 1.651583
    (Iteration 201 / 2450) loss: 1.505065
    (Iteration 301 / 2450) loss: 1.402176
    (Iteration 401 / 2450) loss: 1.609032
    (Epoch 1 / 5) train acc: 0.528000; val_acc: 0.526000
    (Iteration 501 / 2450) loss: 1.373211
    (Iteration 601 / 2450) loss: 1.411114
    (Iteration 701 / 2450) loss: 1.427150
    (Iteration 801 / 2450) loss: 1.125614
    (Iteration 901 / 2450) loss: 1.381660
    (Epoch 2 / 5) train acc: 0.623000; val_acc: 0.586000
    (Iteration 1001 / 2450) loss: 1.328061
    (Iteration 1101 / 2450) loss: 1.253620
    (Iteration 1201 / 2450) loss: 1.295357
    (Iteration 1301 / 2450) loss: 1.128632
    (Iteration 1401 / 2450) loss: 1.310350
    (Epoch 3 / 5) train acc: 0.636000; val_acc: 0.599000
    (Iteration 1501 / 2450) loss: 1.247159
    (Iteration 1601 / 2450) loss: 1.114727
    (Iteration 1701 / 2450) loss: 1.051889
    (Iteration 1801 / 2450) loss: 0.946447
    (Iteration 1901 / 2450) loss: 0.935212
    (Epoch 4 / 5) train acc: 0.683000; val_acc: 0.646000
    (Iteration 2001 / 2450) loss: 1.055434
    (Iteration 2101 / 2450) loss: 1.066920
    (Iteration 2201 / 2450) loss: 1.070829
    (Iteration 2301 / 2450) loss: 0.980542
    (Iteration 2401 / 2450) loss: 0.969689
    (Epoch 5 / 5) train acc: 0.712000; val_acc: 0.633000



![png](./ConvolutionalNetworks/output_40_3.png)


    Weight Scale: 0.0018363167904865165 
    Hidden dim: 994 
    Reg: 2.398820107273961e-05 
    Learning Rate: 9.184902864604297e-05 
    
    (Iteration 1 / 2450) loss: 2.302840
    (Epoch 0 / 5) train acc: 0.085000; val_acc: 0.114000



    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-20-eeba33402a01> in <module>()
         21                      print_every=100,
         22                       verbose=True)
    ---> 23     solver.train()
         24     if solver.best_val_acc>best_val_acc:
         25         best_model  = model


    ~/Desktop/postgrado/assignment2/cs231n/solver.py in train(self)
        264 
        265         for t in range(num_iterations):
    --> 266             self._step()
        267 
        268             # Maybe print training loss


    ~/Desktop/postgrado/assignment2/cs231n/solver.py in _step(self)
        187             dw = grads[p]
        188             config = self.optim_configs[p]
    --> 189             next_w, next_config = self.update_rule(w, dw, config)
        190             self.model.params[p] = next_w
        191             self.optim_configs[p] = next_config


    ~/Desktop/postgrado/assignment2/cs231n/optim.py in adam(x, dx, config)
        140     ###########################################################################
        141     config['t'] += 1
    --> 142     config['m'] = config['beta1']*config['m'] + (1-config['beta1'])*dx
        143     mt = config['m'] / (1-config['beta1']**config['t'])
        144     config['v'] = config['beta2']*config['v'] + (1-config['beta2'])*(dx**2)


    KeyboardInterrupt: 


Con estos valores llego a 0.73 en 15 iteraciones pero los borre porque pense que podia encontrar mejores, no fue asi 🙃.

Para encontrar los valores, al igual que con todos los otros modelos, primero busque valores en grandes intervalos y me fui quedando con los mejores y refinando los intervalos.

Corte la ejecucion por tema de tiempos.

Weight Scale: 1.5611608977457342e-05

Hidden dim: 405 

Reg: 0.000969157472523697

Learning Rate: 0.00016519193832288155

# Extra Credit Description
If you implement any additional features for extra credit, clearly describe them here with pointers to any code in this or other files if applicable.


```python
solver.optim_config
```




    {'learning_rate': 0.00016519193832288155}


