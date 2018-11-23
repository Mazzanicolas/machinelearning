
# Image Captioning with LSTMs
In the previous exercise you implemented a vanilla RNN and applied it to image captioning. In this notebook you will implement the LSTM update rule and use it for image captioning.


```python
# As usual, a bit of setup
from __future__ import print_function
import time, os, json
import numpy as np
import matplotlib.pyplot as plt

from cs231n.gradient_check import eval_numerical_gradient, eval_numerical_gradient_array
from cs231n.rnn_layers import *
from cs231n.captioning_solver import CaptioningSolver
from cs231n.classifiers.rnn import CaptioningRNN
from cs231n.coco_utils import load_coco_data, sample_coco_minibatch, decode_captions
from cs231n.image_utils import image_from_url

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

# Load MS-COCO data
As in the previous notebook, we will use the Microsoft COCO dataset for captioning.


```python
# Load COCO data from disk; this returns a dictionary
# We'll work with dimensionality-reduced features for this notebook, but feel
# free to experiment with the original features by changing the flag below.
data = load_coco_data(pca_features=True)

# Print out all the keys and values from the data dictionary
for k, v in data.items():
    if type(v) == np.ndarray:
        print(k, type(v), v.shape, v.dtype)
    else:
        print(k, type(v), len(v))
```

    word_to_idx <class 'dict'> 1004
    train_urls <class 'numpy.ndarray'> (82783,) <U63
    val_features <class 'numpy.ndarray'> (40504, 512) float32
    idx_to_word <class 'list'> 1004
    val_captions <class 'numpy.ndarray'> (195954, 17) int32
    train_captions <class 'numpy.ndarray'> (400135, 17) int32
    train_image_idxs <class 'numpy.ndarray'> (400135,) int32
    val_image_idxs <class 'numpy.ndarray'> (195954,) int32
    train_features <class 'numpy.ndarray'> (82783, 512) float32
    val_urls <class 'numpy.ndarray'> (40504,) <U63


# LSTM
If you read recent papers, you'll see that many people use a variant on the vanialla RNN called Long-Short Term Memory (LSTM) RNNs. Vanilla RNNs can be tough to train on long sequences due to vanishing and exploding gradiants caused by repeated matrix multiplication. LSTMs solve this problem by replacing the simple update rule of the vanilla RNN with a gating mechanism as follows.

Similar to the vanilla RNN, at each timestep we receive an input $x_t\in\mathbb{R}^D$ and the previous hidden state $h_{t-1}\in\mathbb{R}^H$; the LSTM also maintains an $H$-dimensional *cell state*, so we also receive the previous cell state $c_{t-1}\in\mathbb{R}^H$. The learnable parameters of the LSTM are an *input-to-hidden* matrix $W_x\in\mathbb{R}^{4H\times D}$, a *hidden-to-hidden* matrix $W_h\in\mathbb{R}^{4H\times H}$ and a *bias vector* $b\in\mathbb{R}^{4H}$.

At each timestep we first compute an *activation vector* $a\in\mathbb{R}^{4H}$ as $a=W_xx_t + W_hh_{t-1}+b$. We then divide this into four vectors $a_i,a_f,a_o,a_g\in\mathbb{R}^H$ where $a_i$ consists of the first $H$ elements of $a$, $a_f$ is the next $H$ elements of $a$, etc. We then compute the *input gate* $g\in\mathbb{R}^H$, *forget gate* $f\in\mathbb{R}^H$, *output gate* $o\in\mathbb{R}^H$ and *block input* $g\in\mathbb{R}^H$ as

$$
\begin{align*}
i = \sigma(a_i) \hspace{2pc}
f = \sigma(a_f) \hspace{2pc}
o = \sigma(a_o) \hspace{2pc}
g = \tanh(a_g)
\end{align*}
$$

where $\sigma$ is the sigmoid function and $\tanh$ is the hyperbolic tangent, both applied elementwise.

Finally we compute the next cell state $c_t$ and next hidden state $h_t$ as

$$
c_{t} = f\odot c_{t-1} + i\odot g \hspace{4pc}
h_t = o\odot\tanh(c_t)
$$

where $\odot$ is the elementwise product of vectors.

In the rest of the notebook we will implement the LSTM update rule and apply it to the image captioning task. 

In the code, we assume that data is stored in batches so that $X_t \in \mathbb{R}^{N\times D}$, and will work with *transposed* versions of the parameters: $W_x \in \mathbb{R}^{D \times 4H}$, $W_h \in \mathbb{R}^{H\times 4H}$ so that activations $A \in \mathbb{R}^{N\times 4H}$ can be computed efficiently as $A = X_t W_x + H_{t-1} W_h$

# LSTM: step forward
Implement the forward pass for a single timestep of an LSTM in the `lstm_step_forward` function in the file `cs231n/rnn_layers.py`. This should be similar to the `rnn_step_forward` function that you implemented above, but using the LSTM update rule instead.

Once you are done, run the following to perform a simple test of your implementation. You should see errors around `1e-8` or less.


```python
N, D, H = 3, 4, 5
x = np.linspace(-0.4, 1.2, num=N*D).reshape(N, D)
prev_h = np.linspace(-0.3, 0.7, num=N*H).reshape(N, H)
prev_c = np.linspace(-0.4, 0.9, num=N*H).reshape(N, H)
Wx = np.linspace(-2.1, 1.3, num=4*D*H).reshape(D, 4 * H)
Wh = np.linspace(-0.7, 2.2, num=4*H*H).reshape(H, 4 * H)
b = np.linspace(0.3, 0.7, num=4*H)

next_h, next_c, cache = lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)

expected_next_h = np.asarray([
    [ 0.24635157,  0.28610883,  0.32240467,  0.35525807,  0.38474904],
    [ 0.49223563,  0.55611431,  0.61507696,  0.66844003,  0.7159181 ],
    [ 0.56735664,  0.66310127,  0.74419266,  0.80889665,  0.858299  ]])
expected_next_c = np.asarray([
    [ 0.32986176,  0.39145139,  0.451556,    0.51014116,  0.56717407],
    [ 0.66382255,  0.76674007,  0.87195994,  0.97902709,  1.08751345],
    [ 0.74192008,  0.90592151,  1.07717006,  1.25120233,  1.42395676]])

print('next_h error: ', rel_error(expected_next_h, next_h))
print('next_c error: ', rel_error(expected_next_c, next_c))
```

    next_h error:  5.7054130404539434e-09
    next_c error:  5.8143123088804145e-09


# LSTM: step backward
Implement the backward pass for a single LSTM timestep in the function `lstm_step_backward` in the file `cs231n/rnn_layers.py`. Once you are done, run the following to perform numeric gradient checking on your implementation. You should see errors around `1e-6` or less.


```python
np.random.seed(231)

N, D, H = 4, 5, 6
x = np.random.randn(N, D)
prev_h = np.random.randn(N, H)
prev_c = np.random.randn(N, H)
Wx = np.random.randn(D, 4 * H)
Wh = np.random.randn(H, 4 * H)
b = np.random.randn(4 * H)

next_h, next_c, cache = lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)

dnext_h = np.random.randn(*next_h.shape)
dnext_c = np.random.randn(*next_c.shape)

fx_h = lambda x: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]
fh_h = lambda h: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]
fc_h = lambda c: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]
fWx_h = lambda Wx: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]
fWh_h = lambda Wh: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]
fb_h = lambda b: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[0]

fx_c = lambda x: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]
fh_c = lambda h: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]
fc_c = lambda c: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]
fWx_c = lambda Wx: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]
fWh_c = lambda Wh: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]
fb_c = lambda b: lstm_step_forward(x, prev_h, prev_c, Wx, Wh, b)[1]

num_grad = eval_numerical_gradient_array

dx_num = num_grad(fx_h, x, dnext_h) + num_grad(fx_c, x, dnext_c)
dh_num = num_grad(fh_h, prev_h, dnext_h) + num_grad(fh_c, prev_h, dnext_c)
dc_num = num_grad(fc_h, prev_c, dnext_h) + num_grad(fc_c, prev_c, dnext_c)
dWx_num = num_grad(fWx_h, Wx, dnext_h) + num_grad(fWx_c, Wx, dnext_c)
dWh_num = num_grad(fWh_h, Wh, dnext_h) + num_grad(fWh_c, Wh, dnext_c)
db_num = num_grad(fb_h, b, dnext_h) + num_grad(fb_c, b, dnext_c)

dx, dh, dc, dWx, dWh, db = lstm_step_backward(dnext_h, dnext_c, cache)

print('dx error: ', rel_error(dx_num, dx))
print('dh error: ', rel_error(dh_num, dh))
print('dc error: ', rel_error(dc_num, dc))
print('dWx error: ', rel_error(dWx_num, dWx))
print('dWh error: ', rel_error(dWh_num, dWh))
print('db error: ', rel_error(db_num, db))
```

    dx error:  5.60639146653518e-10
    dh error:  3.004141341657747e-10
    dc error:  3.498107768721507e-11
    dWx error:  1.6933643922734908e-09
    dWh error:  4.8937523109148826e-08
    db error:  1.7349247160222088e-10


# LSTM: forward
In the function `lstm_forward` in the file `cs231n/rnn_layers.py`, implement the `lstm_forward` function to run an LSTM forward on an entire timeseries of data.

When you are done, run the following to check your implementation. You should see an error around `1e-7`.


```python
N, D, H, T = 2, 5, 4, 3
x = np.linspace(-0.4, 0.6, num=N*T*D).reshape(N, T, D)
h0 = np.linspace(-0.4, 0.8, num=N*H).reshape(N, H)
Wx = np.linspace(-0.2, 0.9, num=4*D*H).reshape(D, 4 * H)
Wh = np.linspace(-0.3, 0.6, num=4*H*H).reshape(H, 4 * H)
b = np.linspace(0.2, 0.7, num=4*H)

h, cache = lstm_forward(x, h0, Wx, Wh, b)

expected_h = np.asarray([
 [[ 0.01764008,  0.01823233,  0.01882671,  0.0194232 ],
  [ 0.11287491,  0.12146228,  0.13018446,  0.13902939],
  [ 0.31358768,  0.33338627,  0.35304453,  0.37250975]],
 [[ 0.45767879,  0.4761092,   0.4936887,   0.51041945],
  [ 0.6704845,   0.69350089,  0.71486014,  0.7346449 ],
  [ 0.81733511,  0.83677871,  0.85403753,  0.86935314]]])

print('h error: ', rel_error(expected_h, h))
```

    h error:  8.610537452106624e-08


# LSTM: backward
Implement the backward pass for an LSTM over an entire timeseries of data in the function `lstm_backward` in the file `cs231n/rnn_layers.py`. When you are done, run the following to perform numeric gradient checking on your implementation. You should see errors around `1e-7` or less.


```python
from cs231n.rnn_layers import lstm_forward, lstm_backward
np.random.seed(231)

N, D, T, H = 2, 3, 10, 6

x = np.random.randn(N, T, D)
h0 = np.random.randn(N, H)
Wx = np.random.randn(D, 4 * H)
Wh = np.random.randn(H, 4 * H)
b = np.random.randn(4 * H)

out, cache = lstm_forward(x, h0, Wx, Wh, b)

dout = np.random.randn(*out.shape)

dx, dh0, dWx, dWh, db = lstm_backward(dout, cache)

fx = lambda x: lstm_forward(x, h0, Wx, Wh, b)[0]
fh0 = lambda h0: lstm_forward(x, h0, Wx, Wh, b)[0]
fWx = lambda Wx: lstm_forward(x, h0, Wx, Wh, b)[0]
fWh = lambda Wh: lstm_forward(x, h0, Wx, Wh, b)[0]
fb = lambda b: lstm_forward(x, h0, Wx, Wh, b)[0]

dx_num = eval_numerical_gradient_array(fx, x, dout)
dh0_num = eval_numerical_gradient_array(fh0, h0, dout)
dWx_num = eval_numerical_gradient_array(fWx, Wx, dout)
dWh_num = eval_numerical_gradient_array(fWh, Wh, dout)
db_num = eval_numerical_gradient_array(fb, b, dout)

print('dx error: ', rel_error(dx_num, dx))
print('dh0 error: ', rel_error(dh0_num, dh0))
print('dWx error: ', rel_error(dWx_num, dWx))
print('dWh error: ', rel_error(dWh_num, dWh))
print('db error: ', rel_error(db_num, db))
```

    dx error:  4.5622504640570264e-09
    dh0 error:  2.3832047548384276e-09
    dWx error:  1.7818983147550243e-09
    dWh error:  9.896888305121735e-07
    db error:  2.283123767053995e-10


# LSTM captioning model

Now that you have implemented an LSTM, update the implementation of the `loss` method of the `CaptioningRNN` class in the file `cs231n/classifiers/rnn.py` to handle the case where `self.cell_type` is `lstm`. This should require adding less than 10 lines of code.

Once you have done so, run the following to check your implementation. You should see a difference of less than `1e-10`.


```python
N, D, W, H = 10, 20, 30, 40
word_to_idx = {'<NULL>': 0, 'cat': 2, 'dog': 3}
V = len(word_to_idx)
T = 13

model = CaptioningRNN(word_to_idx,
          input_dim=D,
          wordvec_dim=W,
          hidden_dim=H,
          cell_type='lstm',
          dtype=np.float64)

# Set all model parameters to fixed values
for k, v in model.params.items():
  model.params[k] = np.linspace(-1.4, 1.3, num=v.size).reshape(*v.shape)

features = np.linspace(-0.5, 1.7, num=N*D).reshape(N, D)
captions = (np.arange(N * T) % V).reshape(N, T)

loss, grads = model.loss(features, captions)
expected_loss = 9.82445935443

print('loss: ', loss)
print('expected loss: ', expected_loss)
print('difference: ', abs(loss - expected_loss))
```

    loss:  9.824459354432262
    expected loss:  9.82445935443
    difference:  2.263078613395919e-12


# Overfit LSTM captioning model
Run the following to overfit an LSTM captioning model on the same small dataset as we used for the RNN previously. You should see losses less than 0.5.


```python
np.random.seed(231)

small_data = load_coco_data(max_train=50)

small_lstm_model = CaptioningRNN(
          cell_type='lstm',
          word_to_idx=data['word_to_idx'],
          input_dim=data['train_features'].shape[1],
          hidden_dim=512,
          wordvec_dim=256,
          dtype=np.float32,
        )

small_lstm_solver = CaptioningSolver(small_lstm_model, small_data,
           update_rule='adam',
           num_epochs=50,
           batch_size=25,
           optim_config={
             'learning_rate': 5e-3,
           },
           lr_decay=0.995,
           verbose=True, print_every=10,
         )

small_lstm_solver.train()

# Plot the training losses
plt.plot(small_lstm_solver.loss_history)
plt.xlabel('Iteration')
plt.ylabel('Loss')
plt.title('Training loss history')
plt.show()
```

    (Iteration 1 / 100) loss: 79.551150
    (Iteration 11 / 100) loss: 43.829081
    (Iteration 21 / 100) loss: 30.062754
    (Iteration 31 / 100) loss: 14.018904
    (Iteration 41 / 100) loss: 5.980963
    (Iteration 51 / 100) loss: 1.838480
    (Iteration 61 / 100) loss: 0.654598
    (Iteration 71 / 100) loss: 0.290548
    (Iteration 81 / 100) loss: 0.261193
    (Iteration 91 / 100) loss: 0.170291



![png](output_16_1.png)


# LSTM test-time sampling
Modify the `sample` method of the `CaptioningRNN` class to handle the case where `self.cell_type` is `lstm`. This should take fewer than 10 lines of code.

When you are done run the following to sample from your overfit LSTM model on some training and validation set samples.


```python
np.random.seed(231)

small_data = load_coco_data(max_train=1000)

small_lstm_model = CaptioningRNN(
          cell_type='lstm',
          word_to_idx=data['word_to_idx'],
          input_dim=data['train_features'].shape[1],
          hidden_dim=512,
          wordvec_dim=256,
          dtype=np.float32,
        )

small_lstm_solver = CaptioningSolver(small_lstm_model, small_data,
           update_rule='adam',
           num_epochs=50,
           batch_size=25,
           optim_config={
             'learning_rate': 3e-3,
           },
           lr_decay=0.995,
           verbose=True, print_every=10,
         )

small_lstm_solver.train()

# Plot the training losses
plt.plot(small_lstm_solver.loss_history)
plt.xlabel('Iteration')
plt.ylabel('Loss')
plt.title('Training loss history')
plt.show()

for split in ['train', 'val']:
    minibatch = sample_coco_minibatch(small_data, split=split, batch_size=2)
    gt_captions, features, urls = minibatch
    gt_captions = decode_captions(gt_captions, data['idx_to_word'])

    sample_captions = small_lstm_model.sample(features)
    sample_captions = decode_captions(sample_captions, data['idx_to_word'])

    for gt_caption, sample_caption, url in zip(gt_captions, sample_captions, urls):
        plt.imshow(image_from_url(url))
        plt.title('%s\n%s\nGT:%s' % (split, sample_caption, gt_caption))
        plt.axis('off')
        plt.show()
```

    (Iteration 1 / 2000) loss: 79.581730
    (Iteration 11 / 2000) loss: 54.830719
    (Iteration 21 / 2000) loss: 52.277570
    (Iteration 31 / 2000) loss: 53.092640
    (Iteration 41 / 2000) loss: 46.496372
    (Iteration 51 / 2000) loss: 44.962076
    (Iteration 61 / 2000) loss: 43.230637
    (Iteration 71 / 2000) loss: 43.656380
    (Iteration 81 / 2000) loss: 40.454232
    (Iteration 91 / 2000) loss: 31.953225
    (Iteration 101 / 2000) loss: 34.801443
    (Iteration 111 / 2000) loss: 32.585927
    (Iteration 121 / 2000) loss: 31.674742
    (Iteration 131 / 2000) loss: 29.113654
    (Iteration 141 / 2000) loss: 27.439888
    (Iteration 151 / 2000) loss: 28.395802
    (Iteration 161 / 2000) loss: 27.101569
    (Iteration 171 / 2000) loss: 23.182300
    (Iteration 181 / 2000) loss: 22.336984
    (Iteration 191 / 2000) loss: 19.211849
    (Iteration 201 / 2000) loss: 19.016961
    (Iteration 211 / 2000) loss: 20.790798
    (Iteration 221 / 2000) loss: 21.865415
    (Iteration 231 / 2000) loss: 18.906079
    (Iteration 241 / 2000) loss: 22.041964
    (Iteration 251 / 2000) loss: 17.065395
    (Iteration 261 / 2000) loss: 16.756419
    (Iteration 271 / 2000) loss: 15.685668
    (Iteration 281 / 2000) loss: 12.359284
    (Iteration 291 / 2000) loss: 14.737323
    (Iteration 301 / 2000) loss: 11.968473
    (Iteration 311 / 2000) loss: 9.566314
    (Iteration 321 / 2000) loss: 11.214794
    (Iteration 331 / 2000) loss: 8.124977
    (Iteration 341 / 2000) loss: 7.677010
    (Iteration 351 / 2000) loss: 11.977653
    (Iteration 361 / 2000) loss: 8.679917
    (Iteration 371 / 2000) loss: 9.692548
    (Iteration 381 / 2000) loss: 6.925709
    (Iteration 391 / 2000) loss: 7.824962
    (Iteration 401 / 2000) loss: 6.558051
    (Iteration 411 / 2000) loss: 5.553387
    (Iteration 421 / 2000) loss: 5.644859
    (Iteration 431 / 2000) loss: 5.003079
    (Iteration 441 / 2000) loss: 4.665414
    (Iteration 451 / 2000) loss: 5.020443
    (Iteration 461 / 2000) loss: 5.841846
    (Iteration 471 / 2000) loss: 3.667565
    (Iteration 481 / 2000) loss: 3.434209
    (Iteration 491 / 2000) loss: 3.992069
    (Iteration 501 / 2000) loss: 3.744020
    (Iteration 511 / 2000) loss: 3.735308
    (Iteration 521 / 2000) loss: 3.450564
    (Iteration 531 / 2000) loss: 3.412977
    (Iteration 541 / 2000) loss: 2.100904
    (Iteration 551 / 2000) loss: 2.123574
    (Iteration 561 / 2000) loss: 2.752522
    (Iteration 571 / 2000) loss: 2.057750
    (Iteration 581 / 2000) loss: 2.744519
    (Iteration 591 / 2000) loss: 2.450234
    (Iteration 601 / 2000) loss: 2.120482
    (Iteration 611 / 2000) loss: 1.573735
    (Iteration 621 / 2000) loss: 1.810007
    (Iteration 631 / 2000) loss: 1.766707
    (Iteration 641 / 2000) loss: 0.760949
    (Iteration 651 / 2000) loss: 1.209343
    (Iteration 661 / 2000) loss: 2.080899
    (Iteration 671 / 2000) loss: 0.934098
    (Iteration 681 / 2000) loss: 0.673748
    (Iteration 691 / 2000) loss: 0.798502
    (Iteration 701 / 2000) loss: 0.682590
    (Iteration 711 / 2000) loss: 0.547197
    (Iteration 721 / 2000) loss: 0.843689
    (Iteration 731 / 2000) loss: 0.533715
    (Iteration 741 / 2000) loss: 0.659855
    (Iteration 751 / 2000) loss: 0.507624
    (Iteration 761 / 2000) loss: 0.396016
    (Iteration 771 / 2000) loss: 0.368386
    (Iteration 781 / 2000) loss: 0.258040
    (Iteration 791 / 2000) loss: 0.271627
    (Iteration 801 / 2000) loss: 0.257977
    (Iteration 811 / 2000) loss: 0.267286
    (Iteration 821 / 2000) loss: 0.313924
    (Iteration 831 / 2000) loss: 0.192959
    (Iteration 841 / 2000) loss: 0.242730
    (Iteration 851 / 2000) loss: 0.219307
    (Iteration 861 / 2000) loss: 0.291267
    (Iteration 871 / 2000) loss: 0.155973
    (Iteration 881 / 2000) loss: 0.158087
    (Iteration 891 / 2000) loss: 0.143888
    (Iteration 901 / 2000) loss: 0.207100
    (Iteration 911 / 2000) loss: 0.181028
    (Iteration 921 / 2000) loss: 0.100595
    (Iteration 931 / 2000) loss: 0.155329
    (Iteration 941 / 2000) loss: 0.101745
    (Iteration 951 / 2000) loss: 0.128483
    (Iteration 961 / 2000) loss: 0.116271
    (Iteration 971 / 2000) loss: 0.124554
    (Iteration 981 / 2000) loss: 0.115973
    (Iteration 991 / 2000) loss: 0.158172
    (Iteration 1001 / 2000) loss: 0.106140
    (Iteration 1011 / 2000) loss: 0.111185
    (Iteration 1021 / 2000) loss: 0.083310
    (Iteration 1031 / 2000) loss: 0.092715
    (Iteration 1041 / 2000) loss: 0.081535
    (Iteration 1051 / 2000) loss: 0.085896
    (Iteration 1061 / 2000) loss: 0.084964
    (Iteration 1071 / 2000) loss: 0.075231
    (Iteration 1081 / 2000) loss: 0.084928
    (Iteration 1091 / 2000) loss: 0.064561
    (Iteration 1101 / 2000) loss: 0.082277
    (Iteration 1111 / 2000) loss: 0.119092
    (Iteration 1121 / 2000) loss: 0.077395
    (Iteration 1131 / 2000) loss: 0.067176
    (Iteration 1141 / 2000) loss: 0.062634
    (Iteration 1151 / 2000) loss: 0.083103
    (Iteration 1161 / 2000) loss: 0.074357
    (Iteration 1171 / 2000) loss: 0.061942
    (Iteration 1181 / 2000) loss: 0.061248
    (Iteration 1191 / 2000) loss: 0.060667
    (Iteration 1201 / 2000) loss: 0.061010
    (Iteration 1211 / 2000) loss: 0.063030
    (Iteration 1221 / 2000) loss: 0.208799
    (Iteration 1231 / 2000) loss: 0.056715
    (Iteration 1241 / 2000) loss: 0.054777
    (Iteration 1251 / 2000) loss: 0.059330
    (Iteration 1261 / 2000) loss: 0.082793
    (Iteration 1271 / 2000) loss: 0.057077
    (Iteration 1281 / 2000) loss: 0.056554
    (Iteration 1291 / 2000) loss: 0.054432
    (Iteration 1301 / 2000) loss: 0.054468
    (Iteration 1311 / 2000) loss: 0.060059
    (Iteration 1321 / 2000) loss: 0.058455
    (Iteration 1331 / 2000) loss: 0.054073
    (Iteration 1341 / 2000) loss: 0.052818
    (Iteration 1351 / 2000) loss: 0.050462
    (Iteration 1361 / 2000) loss: 0.049557
    (Iteration 1371 / 2000) loss: 0.044944
    (Iteration 1381 / 2000) loss: 0.044351
    (Iteration 1391 / 2000) loss: 0.048784
    (Iteration 1401 / 2000) loss: 0.046339
    (Iteration 1411 / 2000) loss: 0.044617
    (Iteration 1421 / 2000) loss: 0.047304
    (Iteration 1431 / 2000) loss: 0.044838
    (Iteration 1441 / 2000) loss: 0.043450
    (Iteration 1451 / 2000) loss: 0.043509
    (Iteration 1461 / 2000) loss: 0.049445
    (Iteration 1471 / 2000) loss: 0.047732
    (Iteration 1481 / 2000) loss: 0.045831
    (Iteration 1491 / 2000) loss: 0.041737
    (Iteration 1501 / 2000) loss: 0.047556
    (Iteration 1511 / 2000) loss: 0.040651
    (Iteration 1521 / 2000) loss: 0.042224
    (Iteration 1531 / 2000) loss: 0.038123
    (Iteration 1541 / 2000) loss: 0.042440
    (Iteration 1551 / 2000) loss: 0.045844
    (Iteration 1561 / 2000) loss: 0.055580
    (Iteration 1571 / 2000) loss: 0.038101
    (Iteration 1581 / 2000) loss: 0.034266
    (Iteration 1591 / 2000) loss: 0.036626
    (Iteration 1601 / 2000) loss: 0.035969
    (Iteration 1611 / 2000) loss: 0.063630
    (Iteration 1621 / 2000) loss: 0.038144
    (Iteration 1631 / 2000) loss: 0.164598
    (Iteration 1641 / 2000) loss: 0.038143
    (Iteration 1651 / 2000) loss: 0.036216
    (Iteration 1661 / 2000) loss: 0.035498
    (Iteration 1671 / 2000) loss: 0.035664
    (Iteration 1681 / 2000) loss: 0.035414
    (Iteration 1691 / 2000) loss: 0.035154
    (Iteration 1701 / 2000) loss: 0.031335
    (Iteration 1711 / 2000) loss: 0.058053
    (Iteration 1721 / 2000) loss: 0.036111
    (Iteration 1731 / 2000) loss: 0.030785
    (Iteration 1741 / 2000) loss: 0.034189
    (Iteration 1751 / 2000) loss: 0.042867
    (Iteration 1761 / 2000) loss: 0.031107
    (Iteration 1771 / 2000) loss: 0.031342
    (Iteration 1781 / 2000) loss: 0.030560
    (Iteration 1791 / 2000) loss: 0.031447
    (Iteration 1801 / 2000) loss: 0.031230
    (Iteration 1811 / 2000) loss: 0.027880
    (Iteration 1821 / 2000) loss: 0.033464
    (Iteration 1831 / 2000) loss: 0.029036
    (Iteration 1841 / 2000) loss: 0.137599
    (Iteration 1851 / 2000) loss: 0.028267
    (Iteration 1861 / 2000) loss: 0.028932
    (Iteration 1871 / 2000) loss: 0.025586
    (Iteration 1881 / 2000) loss: 0.031587
    (Iteration 1891 / 2000) loss: 0.029012
    (Iteration 1901 / 2000) loss: 0.026172
    (Iteration 1911 / 2000) loss: 0.027960
    (Iteration 1921 / 2000) loss: 0.026556
    (Iteration 1931 / 2000) loss: 0.027711
    (Iteration 1941 / 2000) loss: 0.025474
    (Iteration 1951 / 2000) loss: 0.027339
    (Iteration 1961 / 2000) loss: 0.024753
    (Iteration 1971 / 2000) loss: 0.025015
    (Iteration 1981 / 2000) loss: 0.023776
    (Iteration 1991 / 2000) loss: 0.023665



![png](output_18_1.png)



![png](output_18_2.png)



![png](output_18_3.png)



![png](output_18_4.png)



![png](output_18_5.png)


Primero se busco buenos numeros con numeros aleatorios generados con distribucion uniforme y luego se refino con pruebas en intervalos reducidos. Los resultados se comprobaron "a ojo"

# Train a good captioning model!
Using the pieces you have implemented in this and the previous notebook, try to train a captioning model that gives decent qualitative results (better than the random garbage you saw with the overfit models) when sampling on the validation set. You can subsample the training set if you want; we just want to see samples on the validation set that are better than random.

Don't spend too much time on this part; we don't have any explicit accuracy thresholds you need to meet.


```python
for split in ['train', 'val']:
    minibatch = sample_coco_minibatch(small_data, split=split, batch_size=5)
    gt_captions, features, urls = minibatch
    gt_captions = decode_captions(gt_captions, data['idx_to_word'])

    sample_captions = small_lstm_model.sample(features)
    sample_captions = decode_captions(sample_captions, data['idx_to_word'])

    for gt_caption, sample_caption, url in zip(gt_captions, sample_captions, urls):
        plt.imshow(image_from_url(url))
        plt.title('%s\n%s\nGT:%s' % (split, sample_caption, gt_caption))
        plt.axis('off')
        plt.show()
```


![png](output_21_0.png)



![png](output_21_1.png)



![png](output_21_2.png)



![png](output_21_3.png)



![png](output_21_4.png)



![png](output_21_5.png)



![png](output_21_6.png)



![png](output_21_7.png)



![png](output_21_8.png)



![png](output_21_9.png)

