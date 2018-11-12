
## What's this TensorFlow business?

You've written a lot of code in this assignment to provide a whole host of neural network functionality. Dropout, Batch Norm, and 2D convolutions are some of the workhorses of deep learning in computer vision. You've also worked hard to make your code efficient and vectorized.

For the last part of this assignment, though, we're going to leave behind your beautiful codebase and instead migrate to one of two popular deep learning frameworks: in this instance, TensorFlow (or PyTorch, if you switch over to that notebook)

#### What is it?
TensorFlow is a system for executing computational graphs over Tensor objects, with native support for performing backpropogation for its Variables. In it, we work with Tensors which are n-dimensional arrays analogous to the numpy ndarray.

#### Why?

* Our code can now be adapted to run on GPUs! Much faster training. Writing your own modules to run on GPUs is beyond the scope of this class, unfortunately.
* We want you to be ready to use one of these frameworks for your project so you can experiment more efficiently than if you were writing every feature you want to use by hand. 
* We want you to stand on the shoulders of giants! TensorFlow and PyTorch are both excellent frameworks that will make your lives a lot easier, and now that you understand their guts, you are free to use them :) 
* We want you to be exposed to the sort of deep learning code you might run into in academia or industry. 

## How will I learn TensorFlow?

TensorFlow has many excellent tutorials available, including those from [Google themselves](https://www.tensorflow.org/get_started/get_started).

Otherwise, this notebook will walk you through much of what you need to do to train models in TensorFlow. See the end of the notebook for some links to helpful tutorials if you want to learn more or need further clarification on topics that aren't fully explained here.

## Load Datasets



```python
import tensorflow as tf
import numpy as np
import math
import timeit
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
from cs231n.data_utils import load_CIFAR10

def get_CIFAR10_data(num_training=49000, num_validation=1000, num_test=10000):
    """
    Load the CIFAR-10 dataset from disk and perform preprocessing to prepare
    it for the two-layer neural net classifier. These are the same steps as
    we used for the SVM, but condensed to a single function.  
    """
    # Load the raw CIFAR-10 data
    cifar10_dir = 'cs231n/datasets/cifar-10-batches-py'
    X_train, y_train, X_test, y_test = load_CIFAR10(cifar10_dir)

    # Subsample the data
    mask = range(num_training, num_training + num_validation)
    X_val = X_train[mask]
    y_val = y_train[mask]
    mask = range(num_training)
    X_train = X_train[mask]
    y_train = y_train[mask]
    mask = range(num_test)
    X_test = X_test[mask]
    y_test = y_test[mask]

    # Normalize the data: subtract the mean image
    mean_image = np.mean(X_train, axis=0)
    X_train -= mean_image
    X_val -= mean_image
    X_test -= mean_image

    return X_train, y_train, X_val, y_val, X_test, y_test


# Invoke the above function to get our data.
X_train, y_train, X_val, y_val, X_test, y_test = get_CIFAR10_data()
print('Train data shape: ', X_train.shape)
print('Train labels shape: ', y_train.shape)
print('Validation data shape: ', X_val.shape)
print('Validation labels shape: ', y_val.shape)
print('Test data shape: ', X_test.shape)
print('Test labels shape: ', y_test.shape)
```

    Train data shape:  (49000, 32, 32, 3)
    Train labels shape:  (49000,)
    Validation data shape:  (1000, 32, 32, 3)
    Validation labels shape:  (1000,)
    Test data shape:  (10000, 32, 32, 3)
    Test labels shape:  (10000,)


## Example Model

### Some useful utilities

. Remember that our image data is initially N x H x W x C, where:
* N is the number of datapoints
* H is the height of each image in pixels
* W is the height of each image in pixels
* C is the number of channels (usually 3: R, G, B)

This is the right way to represent the data when we are doing something like a 2D convolution, which needs spatial understanding of where the pixels are relative to each other. When we input image data into fully connected affine layers, however, we want each data example to be represented by a single vector -- it's no longer useful to segregate the different channels, rows, and columns of the data.

### The example model itself

The first step to training your own model is defining its architecture.

Here's an example of a convolutional neural network defined in TensorFlow -- try to understand what each line is doing, remembering that each layer is composed upon the previous layer. We haven't trained anything yet - that'll come next - for now, we want you to understand how everything gets set up. 

In that example, you see 2D convolutional layers (Conv2d), ReLU activations, and fully-connected layers (Linear). You also see the Hinge loss function, and the Adam optimizer being used. 

Make sure you understand why the parameters of the Linear layer are 5408 and 10.

### TensorFlow Details
In TensorFlow, much like in our previous notebooks, we'll first specifically initialize our variables, and then our network model.


```python
# clear old variables
tf.reset_default_graph()

# setup input (e.g. the data that changes every batch)
# The first dim is None, and gets sets automatically based on batch size fed in
X = tf.placeholder(tf.float32, [None, 32, 32, 3])
y = tf.placeholder(tf.int64, [None])
is_training = tf.placeholder(tf.bool)

def simple_model(X,y):
    # define our weights (e.g. init_two_layer_convnet)
    
    # setup variables
    Wconv1 = tf.get_variable("Wconv1", shape=[7, 7, 3, 32])
    bconv1 = tf.get_variable("bconv1", shape=[32])
    W1 = tf.get_variable("W1", shape=[5408, 10])
    b1 = tf.get_variable("b1", shape=[10])

    # define our graph (e.g. two_layer_convnet)
    a1 = tf.nn.conv2d(X, Wconv1, strides=[1,2,2,1], padding='VALID') + bconv1
    h1 = tf.nn.relu(a1)
    h1_flat = tf.reshape(h1,[-1,5408])
    y_out = tf.matmul(h1_flat,W1) + b1
    return y_out

y_out = simple_model(X,y)

# define our loss
total_loss = tf.losses.hinge_loss(tf.one_hot(y,10),logits=y_out)
mean_loss = tf.reduce_mean(total_loss)

# define our optimizer
optimizer = tf.train.AdamOptimizer(5e-4) # select optimizer and set learning rate
train_step = optimizer.minimize(mean_loss)
```

TensorFlow supports many other layer types, loss functions, and optimizers - you will experiment with these next. Here's the official API documentation for these (if any of the parameters used above were unclear, this resource will also be helpful). 

* Layers, Activations, Loss functions : https://www.tensorflow.org/api_guides/python/nn
* Optimizers: https://www.tensorflow.org/api_guides/python/train#Optimizers
* BatchNorm: https://www.tensorflow.org/api_docs/python/tf/layers/batch_normalization

### Training the model on one epoch
While we have defined a graph of operations above, in order to execute TensorFlow Graphs, by feeding them input data and computing the results, we first need to create a `tf.Session` object. A session encapsulates the control and state of the TensorFlow runtime. For more information, see the TensorFlow [Getting started](https://www.tensorflow.org/get_started/get_started) guide.

Optionally we can also specify a device context such as `/cpu:0` or `/gpu:0`. For documentation on this behavior see [this TensorFlow guide](https://www.tensorflow.org/tutorials/using_gpu)

You should see a validation loss of around 0.4 to 0.6 and an accuracy of 0.30 to 0.35 below


```python
def run_model(session, predict, loss_val, Xd, yd,
              epochs=1, batch_size=64, print_every=100,
              training=None, plot_losses=False):
    # have tensorflow compute accuracy
    correct_prediction = tf.equal(tf.argmax(predict,1), y)
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    
    # shuffle indicies
    train_indicies = np.arange(Xd.shape[0])
    np.random.shuffle(train_indicies)

    training_now = training is not None
    
    # setting up variables we want to compute (and optimizing)
    # if we have a training function, add that to things we compute
    variables = [mean_loss,correct_prediction,accuracy]
    if training_now:
        variables[-1] = training
    
    # counter 
    iter_cnt = 0
    for e in range(epochs):
        # keep track of losses and accuracy
        correct = 0
        losses = []
        # make sure we iterate over the dataset once
        for i in range(int(math.ceil(Xd.shape[0]/batch_size))):
            # generate indicies for the batch
            start_idx = (i*batch_size)%Xd.shape[0]
            idx = train_indicies[start_idx:start_idx+batch_size]
            
            # create a feed dictionary for this batch
            feed_dict = {X: Xd[idx,:],
                         y: yd[idx],
                         is_training: training_now }
            # get batch size
            actual_batch_size = yd[idx].shape[0]
            
            # have tensorflow compute loss and correct predictions
            # and (if given) perform a training step
            loss, corr, _ = session.run(variables,feed_dict=feed_dict)
            
            # aggregate performance stats
            losses.append(loss*actual_batch_size)
            correct += np.sum(corr)
            
            # print every now and then
            if training_now and (iter_cnt % print_every) == 0:
                print("Iteration {0}: with minibatch training loss = {1:.3g} and accuracy of {2:.2g}"\
                      .format(iter_cnt,loss,np.sum(corr)/np.float(actual_batch_size)))
            iter_cnt += 1
        total_correct = correct/Xd.shape[0]
        total_loss = np.sum(losses)/Xd.shape[0]
        print("Epoch {2}, Overall loss = {0:.3g} and accuracy of {1:.3g}"\
              .format(total_loss,total_correct,e+1))
        if plot_losses:
            plt.plot(losses)
            plt.grid(True)
            plt.title('Epoch {} Loss'.format(e+1))
            plt.xlabel('minibatch number')
            plt.ylabel('minibatch loss')
            plt.show()
    return total_loss,total_correct

with tf.Session() as sess:
    with tf.device("/cpu:0"): #"/cpu:0" or "/gpu:0" 
        sess.run(tf.global_variables_initializer())
        print('Training')
        run_model(sess,y_out,mean_loss,X_train,y_train,1,64,100,train_step,True)
        print('Validation')
        run_model(sess,y_out,mean_loss,X_val,y_val,1,64)
```

    Training
    Iteration 0: with minibatch training loss = 10.3 and accuracy of 0.078
    Iteration 100: with minibatch training loss = 1.28 and accuracy of 0.3
    Iteration 200: with minibatch training loss = 0.736 and accuracy of 0.3
    Iteration 300: with minibatch training loss = 0.535 and accuracy of 0.38
    Iteration 400: with minibatch training loss = 0.622 and accuracy of 0.3
    Iteration 500: with minibatch training loss = 0.555 and accuracy of 0.36
    Iteration 600: with minibatch training loss = 0.398 and accuracy of 0.44
    Iteration 700: with minibatch training loss = 0.408 and accuracy of 0.45
    Epoch 1, Overall loss = 0.769 and accuracy of 0.299



![png](./TensorFlowNetworks/output_10_1.png)


    Validation
    Epoch 1, Overall loss = 0.43 and accuracy of 0.379


## Training a specific model

In this section, we're going to specify a model for you to construct. The goal here isn't to get good performance (that'll be next), but instead to get comfortable with understanding the TensorFlow documentation and configuring your own model. 

Using the code provided above as guidance, and using the following TensorFlow documentation, specify a model with the following architecture:

* 7x7 Convolutional Layer with 32 filters and stride of 1
* ReLU Activation Layer
* Spatial Batch Normalization Layer (trainable parameters, with scale and centering)
* 2x2 Max Pooling layer with a stride of 2
* Affine layer with 1024 output units
* ReLU Activation Layer
* Affine layer from 1024 input units to 10 outputs




```python
# clear old variables
tf.reset_default_graph()

# define our input (e.g. the data that changes every batch)
# The first dim is None, and gets sets automatically based on batch size fed in
X = tf.placeholder(tf.float32, [None, 32, 32, 3])
y = tf.placeholder(tf.int64, [None])
is_training = tf.placeholder(tf.bool)

# define model
def complex_model(X, y, is_training):
    Wconv1 = tf.get_variable('Wconv1', shape=[7, 7, 3, 32])
    bconv1 = tf.get_variable('bconv1', shape=[32])
    conv   = tf.nn.conv2d(X, Wconv1, strides=[1, 1, 1, 1], padding='VALID') + bconv1
    conv1  = tf.nn.relu(conv)
# Dejo la implementacion de batch normalization pero en CPU da error por el orden de entrada NCHW
#>>> The CPU implementation of FusedBatchNorm only supports NHWC tensor format
# Se podria cambiar el orden en el que se pasan pero no me da el tiempo
#    batchnorm = tf.layers.batch_normalization(conv1, axis=1, training=is_training)
    batchnorm = conv1
    pooling   = tf.nn.max_pool(batchnorm, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='VALID')
    reshaped  = tf.reshape(pooling, [-1, 5408])
    W1  = tf.get_variable('W1', shape=[5408, 1024])
    b1  = tf.get_variable('b1', shape=[1024])
    fc  = tf.matmul(reshaped, W1) + b1
    fc1 = tf.nn.relu(fc)
    W2  = tf.get_variable('W2', shape=[1024, 10])
    b2  = tf.get_variable('b2', shape=[10])

    scores = tf.matmul(fc1, W2) + b2
    return scores


y_out = complex_model(X,y,is_training)
```

To make sure you're doing the right thing, use the following tool to check the dimensionality of your output (it should be 64 x 10, since our batches have size 64 and the output of the final affine layer should be 10, corresponding to our 10 classes):


```python
# Now we're going to feed a random batch into the model 
# and make sure the output is the right size
x = np.random.randn(64, 32, 32,3)
with tf.Session() as sess:
    with tf.device("/cpu:0"): #"/cpu:0" or "/gpu:0"
        tf.global_variables_initializer().run()

        ans = sess.run(y_out,feed_dict={X:x,is_training:True})
        %timeit sess.run(y_out,feed_dict={X:x,is_training:True})
        print(ans.shape)
        print(np.array_equal(ans.shape, np.array([64, 10])))
```

    55.1 ms ± 4.78 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
    (64, 10)
    True


You should see the following from the run above 

`(64, 10)`

`True`

### Train the model.

Now that you've seen how to define a model and do a single forward pass of some data through it, let's  walk through how you'd actually train one whole epoch over your training data (using the complex_model you created provided above).

Make sure you understand how each TensorFlow function used below corresponds to what you implemented in your custom neural network implementation.

First, set up an **RMSprop optimizer** (using a 1e-3 learning rate) and a **cross-entropy loss** function. See the TensorFlow documentation for more information
* Layers, Activations, Loss functions : https://www.tensorflow.org/api_guides/python/nn
* Optimizers: https://www.tensorflow.org/api_guides/python/train#Optimizers


```python
# Inputs
#     y_out: is what your model computes
#     y: is your TensorFlow variable with label information
# Outputs
#    mean_loss: a TensorFlow variable (scalar) with numerical loss
#    optimizer: a TensorFlow optimizer
# This should be ~3 lines of code!
mean_loss = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=tf.one_hot(y, 10), logits=y_out))
optimizer = tf.train.RMSPropOptimizer(1e-3)
```


```python
# batch normalization in tensorflow requires this extra dependency
extra_update_ops = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
with tf.control_dependencies(extra_update_ops):
    train_step = optimizer.minimize(mean_loss)
```

### Train the model
Below we'll create a session and train the model over one epoch. You should see a loss of 1.4 to 2.0 and an accuracy of 0.4 to 0.5. There will be some variation due to random seeds and differences in initialization


```python
sess = tf.Session()

sess.run(tf.global_variables_initializer())
print('Training')
run_model(sess,y_out,mean_loss,X_train,y_train,1,64,100,train_step)
```

    Training
    Iteration 0: with minibatch training loss = 48 and accuracy of 0.078
    Iteration 100: with minibatch training loss = 2.36 and accuracy of 0.12
    Iteration 200: with minibatch training loss = 6.6 and accuracy of 0.14
    Iteration 300: with minibatch training loss = 2.12 and accuracy of 0.22
    Iteration 400: with minibatch training loss = 2 and accuracy of 0.19
    Iteration 500: with minibatch training loss = 1.81 and accuracy of 0.3
    Iteration 600: with minibatch training loss = 1.98 and accuracy of 0.2
    Iteration 700: with minibatch training loss = 1.78 and accuracy of 0.34
    Epoch 1, Overall loss = 3.86 and accuracy of 0.215





    (3.86220444578054, 0.21546938775510205)



### Check the accuracy of the model.

Let's see the train and test code in action -- feel free to use these methods when evaluating the models you develop below. You should see a loss of 1.3 to 2.0 with an accuracy of 0.45 to 0.55.


```python
print('Validation')
run_model(sess,y_out,mean_loss,X_val,y_val,1,64)
```

    Validation
    Epoch 1, Overall loss = 1.73 and accuracy of 0.355





    (1.7250834484100341, 0.355)



## Train a _great_ model on CIFAR-10!

Now it's your job to experiment with architectures, hyperparameters, loss functions, and optimizers to train a model that achieves ** >= 70% accuracy on the validation set** of CIFAR-10. You can use the `run_model` function from above.

### Things you should try:
- **Filter size**: Above we used 7x7; this makes pretty pictures but smaller filters may be more efficient
- **Number of filters**: Above we used 32 filters. Do more or fewer do better?
- **Pooling vs Strided Convolution**: Do you use max pooling or just stride convolutions?
- **Batch normalization**: Try adding spatial batch normalization after convolution layers and vanilla batch normalization after affine layers. Do your networks train faster?
- **Network architecture**: The network above has two layers of trainable parameters. Can you do better with a deep network? Good architectures to try include:
    - [conv-relu-pool]xN -> [affine]xM -> [softmax or SVM]
    - [conv-relu-conv-relu-pool]xN -> [affine]xM -> [softmax or SVM]
    - [batchnorm-relu-conv]xN -> [affine]xM -> [softmax or SVM]
- **Use TensorFlow Scope**: Use TensorFlow scope and/or [tf.layers](https://www.tensorflow.org/api_docs/python/tf/layers) to make it easier to write deeper networks. See [this tutorial](https://www.tensorflow.org/tutorials/layers) for how to use `tf.layers`. 
- **Use Learning Rate Decay**: [As the notes point out](http://cs231n.github.io/neural-networks-3/#anneal), decaying the learning rate might help the model converge. Feel free to decay every epoch, when loss doesn't change over an entire epoch, or any other heuristic you find appropriate. See the [Tensorflow documentation](https://www.tensorflow.org/versions/master/api_guides/python/train#Decaying_the_learning_rate) for learning rate decay.
- **Global Average Pooling**: Instead of flattening and then having multiple affine layers, perform convolutions until your image gets small (7x7 or so) and then perform an average pooling operation to get to a 1x1 image picture (1, 1 , Filter#), which is then reshaped into a (Filter#) vector. This is used in [Google's Inception Network](https://arxiv.org/abs/1512.00567) (See Table 1 for their architecture).
- **Regularization**: Add l2 weight regularization, or perhaps use [Dropout as in the TensorFlow MNIST tutorial](https://www.tensorflow.org/get_started/mnist/pros)

### Tips for training
For each network architecture that you try, you should tune the learning rate and regularization strength. When doing this there are a couple important things to keep in mind:

- If the parameters are working well, you should see improvement within a few hundred iterations
- Remember the coarse-to-fine approach for hyperparameter tuning: start by testing a large range of hyperparameters for just a few training iterations to find the combinations of parameters that are working at all.
- Once you have found some sets of parameters that seem to work, search more finely around these parameters. You may need to train for more epochs.
- You should use the validation set for hyperparameter search, and we'll save the test set for evaluating your architecture on the best parameters as selected by the validation set.

### Going above and beyond
If you are feeling adventurous there are many other features you can implement to try and improve your performance. You are **not required** to implement any of these; however they would be good things to try for extra credit.

- Alternative update steps: For the assignment we implemented SGD+momentum, RMSprop, and Adam; you could try alternatives like AdaGrad or AdaDelta.
- Alternative activation functions such as leaky ReLU, parametric ReLU, ELU, or MaxOut.
- Model ensembles
- Data augmentation
- New Architectures
  - [ResNets](https://arxiv.org/abs/1512.03385) where the input from the previous layer is added to the output.
  - [DenseNets](https://arxiv.org/abs/1608.06993) where inputs into previous layers are concatenated together.
  - [This blog has an in-depth overview](https://chatbotslife.com/resnets-highwaynets-and-densenets-oh-my-9bb15918ee32)

If you do decide to implement something extra, clearly describe it in the "Extra Credit Description" cell below.

### What we expect
At the very least, you should be able to train a ConvNet that gets at **>= 70% accuracy on the validation set**. This is just a lower bound - if you are careful it should be possible to get accuracies much higher than that! Extra credit points will be awarded for particularly high-scoring models or unique approaches.

You should use the space below to experiment and train your network. The final cell in this notebook should contain the training and validation set accuracies for your final trained network.

Have fun and happy training!


```python
# Feel free to play with this cell

def my_model(X,y,is_training):
    Wconv1 = tf.get_variable('Wconv1', shape=[3, 3, 3, 64])
    bconv1 = tf.get_variable('bconv1', shape=[64])
    conv   = tf.nn.conv2d(X, Wconv1, strides=[1, 1, 1, 1], padding='VALID') + bconv1
    conv1  = tf.nn.relu(conv)
# Dejo la implementacion de batch normalization pero en CPU da error por el orden de entrada NCHW
#>>> The CPU implementation of FusedBatchNorm only supports NHWC tensor format
# Se podria cambiar el orden en el que se pasan pero no me da el tiempo
#    batchnorm = tf.layers.batch_normalization(conv1, axis=1, training=is_training)
    batchnorm = conv1
    pooling = tf.nn.max_pool(batchnorm, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='VALID')
    h_reshaped = tf.reshape(pooling, [-1, 14400])
    W1 = tf.get_variable('W1', shape=[14400, 1024])
    b1 = tf.get_variable('b1', shape=[1024])
    fc  = tf.matmul(h_reshaped, W1) + b1
    fc1 = tf.nn.relu(fc)
    W2 = tf.get_variable('W2', shape=[1024, 10])
    b2 = tf.get_variable('b2', shape=[10])
    scores = tf.matmul(fc1, W2) + b2
    return scores

tf.reset_default_graph()

X = tf.placeholder(tf.float32, [None, 32, 32, 3])
y = tf.placeholder(tf.int64, [None])
is_training = tf.placeholder(tf.bool)

y_out = my_model(X,y,is_training)
mean_loss = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=tf.one_hot(y, 10), logits=y_out))
# optimizer = tf.train.RMSPropOptimizer(1e-3)
optimizer = tf.train.AdadeltaOptimizer(1e-3)


# batch normalization in tensorflow requires this extra dependency
extra_update_ops = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
with tf.control_dependencies(extra_update_ops):
    train_step = optimizer.minimize(mean_loss)
```

# Adadelta Optimizer
sess = tf.Session()

sess.run(tf.global_variables_initializer())
print('Training')
run_model(sess,y_out,mean_loss,X_train,y_train,10,64,100,train_step,True)
print('Validation')
run_model(sess,y_out,mean_loss,X_val,y_val,1,64)


```python
# RMSProp Optimizer
sess = tf.Session()

sess.run(tf.global_variables_initializer())
print('Training')
run_model(sess,y_out,mean_loss,X_train,y_train,10,64,100,train_step,True)
print('Validation')
run_model(sess,y_out,mean_loss,X_val,y_val,1,64)
```

    Training
    Iteration 0: with minibatch training loss = 40.8 and accuracy of 0.016
    Iteration 100: with minibatch training loss = 2.32 and accuracy of 0.22
    Iteration 200: with minibatch training loss = 2.02 and accuracy of 0.17
    Iteration 300: with minibatch training loss = 1.88 and accuracy of 0.27
    Iteration 400: with minibatch training loss = 2.55 and accuracy of 0.23
    Iteration 500: with minibatch training loss = 2.32 and accuracy of 0.27
    Iteration 600: with minibatch training loss = 2.1 and accuracy of 0.36
    Iteration 700: with minibatch training loss = 1.97 and accuracy of 0.39
    Epoch 1, Overall loss = 3.7 and accuracy of 0.254



![png](./TensorFlowNetworks/output_27_1.png)


    Iteration 800: with minibatch training loss = 1.62 and accuracy of 0.44
    Iteration 900: with minibatch training loss = 1.6 and accuracy of 0.45
    Iteration 1000: with minibatch training loss = 1.75 and accuracy of 0.38
    Iteration 1100: with minibatch training loss = 1.54 and accuracy of 0.44
    Iteration 1200: with minibatch training loss = 1.85 and accuracy of 0.34
    Iteration 1300: with minibatch training loss = 1.58 and accuracy of 0.44
    Iteration 1400: with minibatch training loss = 1.56 and accuracy of 0.45
    Iteration 1500: with minibatch training loss = 1.63 and accuracy of 0.45
    Epoch 2, Overall loss = 1.55 and accuracy of 0.445



![png](./TensorFlowNetworks/output_27_3.png)


    Iteration 1600: with minibatch training loss = 1.13 and accuracy of 0.59
    Iteration 1700: with minibatch training loss = 1.15 and accuracy of 0.55
    Iteration 1800: with minibatch training loss = 1.45 and accuracy of 0.45
    Iteration 1900: with minibatch training loss = 1.34 and accuracy of 0.48
    Iteration 2000: with minibatch training loss = 1.72 and accuracy of 0.47
    Iteration 2100: with minibatch training loss = 1.53 and accuracy of 0.42
    Iteration 2200: with minibatch training loss = 1.34 and accuracy of 0.42
    Epoch 3, Overall loss = 1.34 and accuracy of 0.522



![png](./TensorFlowNetworks/output_27_5.png)


    Iteration 2300: with minibatch training loss = 1.38 and accuracy of 0.48
    Iteration 2400: with minibatch training loss = 1.35 and accuracy of 0.45
    Iteration 2500: with minibatch training loss = 1.28 and accuracy of 0.48
    Iteration 2600: with minibatch training loss = 1.38 and accuracy of 0.5
    Iteration 2700: with minibatch training loss = 2.28 and accuracy of 0.34
    Iteration 2800: with minibatch training loss = 1.28 and accuracy of 0.44
    Iteration 2900: with minibatch training loss = 1.08 and accuracy of 0.64
    Iteration 3000: with minibatch training loss = 1.11 and accuracy of 0.61
    Epoch 4, Overall loss = 1.26 and accuracy of 0.555



![png](./TensorFlowNetworks/output_27_7.png)


    Iteration 3100: with minibatch training loss = 1.15 and accuracy of 0.64
    Iteration 3200: with minibatch training loss = 1.25 and accuracy of 0.5
    Iteration 3300: with minibatch training loss = 1.1 and accuracy of 0.66
    Iteration 3400: with minibatch training loss = 1.04 and accuracy of 0.62
    Iteration 3500: with minibatch training loss = 1.14 and accuracy of 0.59
    Iteration 3600: with minibatch training loss = 1.01 and accuracy of 0.67
    Iteration 3700: with minibatch training loss = 1.54 and accuracy of 0.45
    Iteration 3800: with minibatch training loss = 1.13 and accuracy of 0.61
    Epoch 5, Overall loss = 1.2 and accuracy of 0.578



![png](./TensorFlowNetworks/output_27_9.png)


    Iteration 3900: with minibatch training loss = 1.12 and accuracy of 0.59
    Iteration 4000: with minibatch training loss = 1.31 and accuracy of 0.53
    Iteration 4100: with minibatch training loss = 1.35 and accuracy of 0.59
    Iteration 4200: with minibatch training loss = 1.28 and accuracy of 0.45
    Iteration 4300: with minibatch training loss = 1.2 and accuracy of 0.56
    Iteration 4400: with minibatch training loss = 0.877 and accuracy of 0.61
    Iteration 4500: with minibatch training loss = 0.935 and accuracy of 0.61
    Epoch 6, Overall loss = 1.16 and accuracy of 0.593



![png](./TensorFlowNetworks/output_27_11.png)


    Iteration 4600: with minibatch training loss = 1.18 and accuracy of 0.52
    Iteration 4700: with minibatch training loss = 1.19 and accuracy of 0.59
    Iteration 4800: with minibatch training loss = 1.25 and accuracy of 0.53
    Iteration 4900: with minibatch training loss = 1.43 and accuracy of 0.52
    Iteration 5000: with minibatch training loss = 1.41 and accuracy of 0.5
    Iteration 5100: with minibatch training loss = 0.993 and accuracy of 0.66
    Iteration 5200: with minibatch training loss = 1.15 and accuracy of 0.53
    Iteration 5300: with minibatch training loss = 1.18 and accuracy of 0.61
    Epoch 7, Overall loss = 1.13 and accuracy of 0.605



![png](./TensorFlowNetworks/output_27_13.png)


    Iteration 5400: with minibatch training loss = 1.08 and accuracy of 0.61
    Iteration 5500: with minibatch training loss = 1.2 and accuracy of 0.56
    Iteration 5600: with minibatch training loss = 0.942 and accuracy of 0.67
    Iteration 5700: with minibatch training loss = 1.3 and accuracy of 0.56
    Iteration 5800: with minibatch training loss = 0.88 and accuracy of 0.7
    Iteration 5900: with minibatch training loss = 1.09 and accuracy of 0.56
    Iteration 6000: with minibatch training loss = 1.08 and accuracy of 0.64
    Iteration 6100: with minibatch training loss = 1.19 and accuracy of 0.55
    Epoch 8, Overall loss = 1.11 and accuracy of 0.613



![png](./TensorFlowNetworks/output_27_15.png)


    Iteration 6200: with minibatch training loss = 0.892 and accuracy of 0.7
    Iteration 6300: with minibatch training loss = 1.14 and accuracy of 0.59
    Iteration 6400: with minibatch training loss = 1.2 and accuracy of 0.52
    Iteration 6500: with minibatch training loss = 0.93 and accuracy of 0.69
    Iteration 6600: with minibatch training loss = 1.15 and accuracy of 0.62
    Iteration 6700: with minibatch training loss = 0.981 and accuracy of 0.67
    Iteration 6800: with minibatch training loss = 1.03 and accuracy of 0.64
    Epoch 9, Overall loss = 1.08 and accuracy of 0.623



![png](./TensorFlowNetworks/output_27_17.png)


    Iteration 6900: with minibatch training loss = 1.09 and accuracy of 0.66
    Iteration 7000: with minibatch training loss = 0.923 and accuracy of 0.62
    Iteration 7100: with minibatch training loss = 0.904 and accuracy of 0.66
    Iteration 7200: with minibatch training loss = 0.89 and accuracy of 0.69
    Iteration 7300: with minibatch training loss = 1.28 and accuracy of 0.61
    Iteration 7400: with minibatch training loss = 1.13 and accuracy of 0.59
    Iteration 7500: with minibatch training loss = 0.931 and accuracy of 0.69
    Iteration 7600: with minibatch training loss = 0.839 and accuracy of 0.73
    Epoch 10, Overall loss = 1.06 and accuracy of 0.633



![png](./TensorFlowNetworks/output_27_19.png)


    Validation
    Epoch 1, Overall loss = 1.55 and accuracy of 0.572





    (1.545426100730896, 0.572)



Por temas de tiempo no pude ponerme a buscar un mejor modelo.


```python
# Test your model here, and make sure 
# the output of this cell is the accuracy
# of your best model on the training and val sets
# We're looking for >= 70% accuracy on Validation
print('Training')
run_model(sess,y_out,mean_loss,X_train,y_train,1,64)
print('Validation')
run_model(sess,y_out,mean_loss,X_val,y_val,1,64)
```

    Training
    Epoch 1, Overall loss = 1.15 and accuracy of 0.624
    Validation
    Epoch 1, Overall loss = 1.64 and accuracy of 0.545





    (1.6428911542892457, 0.545)



### Describe what you did here
In this cell you should also write an explanation of what you did, any additional features that you implemented, and any visualizations or graphs that you make in the process of training and evaluating your network

_No hice nada, solo cambie el tamaño del filtro 7x7 por el recomendado de 3x3 y agregue 32 neuronas_

### Test Set - Do this only once
Now that we've gotten a result that we're happy with, we test our final model on the test set. This would be the score we would achieve on a competition. Think about how this compares to your validation set accuracy.


```python
print('Test Adadelta')
run_model(sess,y_out,mean_loss,X_test,y_test,1,64)
```

    Test Adadelta
    Epoch 1, Overall loss = 5.17 and accuracy of 0.377





    (5.173642284011841, 0.3767)




```python
print('Test RMSProp')
run_model(sess,y_out,mean_loss,X_test,y_test,1,64)
```

    Test RMSProp
    Epoch 1, Overall loss = 1.45 and accuracy of 0.567





    (1.4525093048095703, 0.5674)



## Going further with TensorFlow

The next assignment will make heavy use of TensorFlow. You might also find it useful for your projects. 


# Extra Credit Description
If you implement any additional features for extra credit, clearly describe them here with pointers to any code in this or other files if applicable.
