# K-NN

![K-NN](./1_src/img/knn.png)

## K-NN implementation

```python
from past.builtins import xrange

import numpy as np

class KNearestNeighbor(object):
  """ a kNN classifier with L2 distance """

  def __init__(self):
    pass

  def train(self, X, y):
    """
    Train the classifier. For k-nearest neighbors this is just 
    memorizing the training data.

    Inputs:
    - X: A numpy array of shape (num_train, D) containing the training data
      consisting of num_train samples each of dimension D.
    - y: A numpy array of shape (N,) containing the training labels, where
         y[i] is the label for X[i].
    """
    self.X_train = X
    self.y_train = y
    
  def predict(self, X, k=1, num_loops=0):
    """
    Predict labels for test data using this classifier.

    Inputs:
    - X: A numpy array of shape (num_test, D) containing test data consisting
         of num_test samples each of dimension D.
    - k: The number of nearest neighbors that vote for the predicted labels.
    - num_loops: Determines which implementation to use to compute distances
      between training points and testing points.

    Returns:
    - y: A numpy array of shape (num_test,) containing predicted labels for the
      test data, where y[i] is the predicted label for the test point X[i].  
    """
    if num_loops == 0:
      dists = self.compute_distances_no_loops(X)
    elif num_loops == 1:
      dists = self.compute_distances_one_loop(X)
    elif num_loops == 2:
      dists = self.compute_distances_two_loops(X)
    else:
      raise ValueError('Invalid value %d for num_loops' % num_loops)

    return self.predict_labels(dists, k=k)

  def compute_distances_two_loops(self, X):
    """
    Compute the distance between each test point in X and each training point
    in self.X_train using a nested loop over both the training data and the 
    test data.

    Inputs:
    - X: A numpy array of shape (num_test, D) containing test data.

    Returns:
    - dists: A numpy array of shape (num_test, num_train) where dists[i, j]
      is the Euclidean distance between the ith test point and the jth training
      point.
    """
    num_test  = X.shape[0]
    num_train = self.X_train.shape[0]
    dists = np.zeros((num_test, num_train))
    for i in xrange(num_test):#    500
      for j in xrange(num_train):# 5000
        #####################################################################
        # TODO:                                                             #
        # Compute the l2 distance between the ith test point and the jth    #
        # training point, and store the result in dists[i, j]. You should   #
        # not use a loop over dimension.                                    #
        #####################################################################
        dists[i][j] = np.linalg.norm(X[i] - self.X_train[j])
        #####################################################################
        #                       END OF YOUR CODE                            #
        #####################################################################
    return dists
    
  def compute_distances_one_loop(self, X):
    """
    Compute the distance between each test point in X and each training point
    in self.X_train using a single loop over the test data.

    Input / Output: Same as compute_distances_two_loops
    """
    num_test  = X.shape[0]
    num_train = self.X_train.shape[0]
    dists = np.zeros((num_test, num_train))
    for i in xrange(num_test):
      #######################################################################
      # TODO:                                                               #
      # Compute the l2 distance between the ith test point and all training #
      # points, and store the result in dists[i, :].                        #
      #######################################################################
      dists[i,:] = np.linalg.norm((X[i,:] - self.X_train), axis = 1)
      #######################################################################
      #                         END OF YOUR CODE                            #
      #######################################################################
    return dists

  def compute_distances_no_loops(self, X):
    """
    Compute the distance between each test point in X and each training point
    in self.X_train using no explicit loops.

    Input / Output: Same as compute_distances_two_loops
    """
    num_test  = X.shape[0]
    num_train = self.X_train.shape[0]
    dists = np.zeros((num_test, num_train)) 
    #########################################################################
    # TODO:                                                                 #
    # Compute the l2 distance between all test points and all training      #
    # points without using any explicit loops, and store the result in      #
    # dists.                                                                #
    #                                                                       #
    # You should implement this function using only basic array operations; #
    # in particular you should not use functions from scipy.                #
    #                                                                       #
    # HINT: Try to formulate the l2 distance using matrix multiplication    #
    #       and two broadcast sums.                                         #
    #########################################################################
    dists = np.sqrt(np.sum((X**2), axis=1)[:, np.newaxis] + np.sum((self.X_train**2),axis=1) - 2 * X.dot(self.X_train.T))
    #########################################################################
    #                         END OF YOUR CODE                              #
    #########################################################################
    return dists

  def predict_labels(self, dists, k=1):
    """
    Given a matrix of distances between test points and training points,
    predict a label for each test point.

    Inputs:
    - dists: A numpy array of shape (num_test, num_train) where dists[i, j]
      gives the distance betwen the ith test point and the jth training point.

    Returns:
    - y: A numpy array of shape (num_test,) containing predicted labels for the
      test data, where y[i] is the predicted label for the test point X[i].  
    """
    num_test = dists.shape[0]
    y_pred   = np.zeros(num_test)
    for i in xrange(num_test):
      # A list of length k storing the labels of the k nearest neighbors to
      # the ith test point.
      closest_y = []
      #########################################################################
      # TODO:                                                                 #
      # Use the distance matrix to find the k nearest neighbors of the ith    #
      # testing point, and use self.y_train to find the labels of these       #
      # neighbors. Store these labels in closest_y.                           #
      # Hint: Look up the function numpy.argsort.                             #
      #########################################################################
      k_closest = np.argsort(dists[i,:])[:k]
      closest_y = self.y_train[k_closest]
      #########################################################################
      # TODO:                                                                 #
      # Now that you have found the labels of the k nearest neighbors, you    #
      # need to find the most common label in the list closest_y of labels.   #
      # Store this label in y_pred[i]. Break ties by choosing the smaller     #
      # label.                                                                #
      #########################################################################
      counts = np.bincount(closest_y)
      y_pred[i] = np.argmax(counts)
      #########################################################################
      #                           END OF YOUR CODE                            # 
      #########################################################################
    return y_pred
```


## k-Nearest Neighbor (kNN) exercise

*Complete and hand in this completed worksheet (including its outputs and any supporting code outside of the worksheet) with your assignment submission. For more details see the [assignments page](https://eva.fing.edu.uy/mod/page/view.php?id=69213) on the course website.*

The kNN classifier consists of two stages:

- During training, the classifier takes the training data and simply remembers it
- During testing, kNN classifies every test image by comparing to all training images and transfering the labels of the k most similar training examples
- The value of k is cross-validated

In this exercise you will implement these steps and understand the basic Image Classification pipeline, cross-validation, and gain proficiency in writing efficient, vectorized code.


```python
# Run some setup code for this notebook.

import random
import numpy as np
from cs231n.data_utils import load_CIFAR10
import matplotlib.pyplot as plt

from __future__ import print_function

# This is a bit of magic to make matplotlib figures appear inline in the notebook
# rather than in a new window.
%matplotlib inline
plt.rcParams['figure.figsize'] = (10.0, 8.0) # set default size of plots
plt.rcParams['image.interpolation'] = 'nearest'
plt.rcParams['image.cmap'] = 'gray'

# Some more magic so that the notebook will reload external python modules;
# see http://stackoverflow.com/questions/1907993/autoreload-of-modules-in-ipython
%load_ext autoreload
%autoreload 2
```


```python
# Load the raw CIFAR-10 data.
cifar10_dir = 'cs231n/datasets/cifar-10-batches-py'
X_train, y_train, X_test, y_test = load_CIFAR10(cifar10_dir)

# As a sanity check, we print out the size of the training and test data.
print('Training data shape: ', X_train.shape)
print('Training labels shape: ', y_train.shape)
print('Test data shape: ', X_test.shape)
print('Test labels shape: ', y_test.shape)
```

    Training data shape:  (50000, 32, 32, 3)
    Training labels shape:  (50000,)
    Test data shape:  (10000, 32, 32, 3)
    Test labels shape:  (10000,)



```python
# Visualize some examples from the dataset.
# We show a few examples of training images from each class.
classes = ['plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
num_classes = len(classes)
samples_per_class = 7
for y, cls in enumerate(classes):
    idxs = np.flatnonzero(y_train == y)
    idxs = np.random.choice(idxs, samples_per_class, replace=False)
    for i, idx in enumerate(idxs):
        plt_idx = i * num_classes + y + 1
        plt.subplot(samples_per_class, num_classes, plt_idx)
        plt.imshow(X_train[idx].astype('uint8'))
        plt.axis('off')
        if i == 0:
            plt.title(cls)
plt.show()
```


![png](./1_src/img/output_3_0.png)



```python
# Subsample the data for more efficient code execution in this exercise
num_training = 5000
mask = range(num_training)
X_train = X_train[mask]
y_train = y_train[mask]

num_test = 500
mask = range(num_test)
X_test = X_test[mask]
y_test = y_test[mask]
```


```python
# Reshape the image data into rows
X_train = np.reshape(X_train, (X_train.shape[0], -1))
X_test  = np.reshape(X_test, (X_test.shape[0], -1))
print(X_train.shape, X_test.shape)
```

    (5000, 3072) (500, 3072)



```python
from cs231n.classifiers import KNearestNeighbor

# Create a kNN classifier instance. 
# Remember that training a kNN classifier is a noop: 
# the Classifier simply remembers the data and does no further processing 
classifier = KNearestNeighbor()
classifier.train(X_train, y_train)
```

We would now like to classify the test data with the kNN classifier. Recall that we can break down this process into two steps: 

1. First we must compute the distances between all test examples and all train examples. 
2. Given these distances, for each test example we find the k nearest examples and have them vote for the label

Lets begin with computing the distance matrix between all training and test examples. For example, if there are **Ntr** training examples and **Nte** test examples, this stage should result in a **Nte x Ntr** matrix where each element (i,j) is the distance between the i-th test and j-th train example.

First, open `cs231n/classifiers/k_nearest_neighbor.py` and implement the function `compute_distances_two_loops` that uses a (very inefficient) double loop over all pairs of (test, train) examples and computes the distance matrix one element at a time.


```python
# Open cs231n/classifiers/k_nearest_neighbor.py and implement
# compute_distances_two_loops.

# Test your implementation:
dists = classifier.compute_distances_two_loops(X_test)
print(dists.shape)
```

    (500, 5000)



```python
# We can visualize the distance matrix: each row is a single test example and
# its distances to training examples
plt.rcParams['figure.figsize'] = (20.0, 16.0)
plt.imshow(dists, interpolation='none')
plt.show()
plt.rcParams['figure.figsize'] = (10.0, 8.0)
```


![png](./1_src/img/output_9_0.png)


**Inline Question #1:** Notice the structured patterns in the distance matrix, where some rows or columns are visible brighter. (Note that with the default color scheme black indicates low distances while white indicates high distances.)

- What in the data is the cause behind the distinctly bright rows?
- What causes the columns?

**Your Answer**: *Las lineas blancas indican una alta distancia entre el ejemplo de test y la mayoria de los casos de training, es decir, una baja similitud. Las columnas indican una baja similitud entre un ejemplo de training y la mayoria de los casos de test.*




```python
# Now implement the function predict_labels and run the code below:
# We use k = 1 (which is Nearest Neighbor).
y_test_pred = classifier.predict_labels(dists, k=1)

# Compute and print the fraction of correctly predicted examples
num_correct = np.sum(y_test_pred == y_test)
accuracy = float(num_correct) / num_test
print('Got %d / %d correct => accuracy: %f' % (num_correct, num_test, accuracy))
```

    Got 137 / 500 correct => accuracy: 0.274000


You should expect to see approximately `27%` accuracy. Now lets try out a larger `k`, say `k = 5`:


```python
y_test_pred = classifier.predict_labels(dists, k=5)
num_correct = np.sum(y_test_pred == y_test)
accuracy = float(num_correct) / num_test
print ('Got %d / %d correct => accuracy: %f' % (num_correct, num_test, accuracy))
```

    Got 139 / 500 correct => accuracy: 0.278000


You should expect to see a slightly better performance than with `k = 1`.


```python
# Now lets speed up distance matrix computation by using partial vectorization
# with one loop. Implement the function compute_distances_one_loop and run the
# code below:
dists_one = classifier.compute_distances_one_loop(X_test)

# To ensure that our vectorized implementation is correct, we make sure that it
# agrees with the naive implementation. There are many ways to decide whether
# two matrices are similar; one of the simplest is the Frobenius norm. In case
# you haven't seen it before, the Frobenius norm of two matrices is the square
# root of the squared sum of differences of all elements; in other words, reshape
# the matrices into vectors and compute the Euclidean distance between them.
difference = np.linalg.norm(dists - dists_one, ord='fro')
print('Difference was: %f' % (difference, ))
if difference < 0.001:
    print('Good! The distance matrices are the same')
else:
    print('Uh-oh! The distance matrices are different')
```

    Difference was: 0.000000
    Good! The distance matrices are the same



```python
# Now implement the fully vectorized version inside compute_distances_no_loops
# and run the code
dists_two = classifier.compute_distances_no_loops(X_test)

# check that the distance matrix agrees with the one we computed before:
difference = np.linalg.norm(dists - dists_two, ord='fro')
print('Difference was: %f' % (difference, ))
if difference < 0.001:
    print('Good! The distance matrices are the same')
else:
    print('Uh-oh! The distance matrices are different')
```

    Difference was: 0.000000
    Good! The distance matrices are the same



```python
# Let's compare how fast the implementations are
def time_function(f, *args):
  """
  Call a function f with args and return the time (in seconds) that it took to execute.
  """
  import time
  tic = time.time()
  f(*args)
  toc = time.time()
  return toc - tic

two_loop_time = time_function(classifier.compute_distances_two_loops, X_test)
print('Two loop version took %f seconds' % two_loop_time)

one_loop_time = time_function(classifier.compute_distances_one_loop, X_test)
print('One loop version took %f seconds' % one_loop_time)

no_loop_time = time_function(classifier.compute_distances_no_loops, X_test)
print('No loop version took %f seconds' % no_loop_time)

# you should see significantly faster performance with the fully vectorized implementation
```

    Two loop version took 54.537742 seconds
    One loop version took 66.792842 seconds
    No loop version took 0.724008 seconds


### Cross-validation

We have implemented the k-Nearest Neighbor classifier but we set the value k = 5 arbitrarily. We will now determine the best value of this hyperparameter with cross-validation.


```python
num_folds = 5
k_choices = [1, 3, 5, 8, 10, 12, 15, 20, 50, 100]

X_train_folds = []
y_train_folds = []
################################################################################
# TODO:                                                                        #
# Split up the training data into folds. After splitting, X_train_folds and    #
# y_train_folds should each be lists of length num_folds, where                #
# y_train_folds[i] is the label vector for the points in X_train_folds[i].     #
# Hint: Look up the numpy array_split function.                                #
################################################################################
X_train_folds = np.array_split(X_train, num_folds, axis=0)
y_train_folds = np.array_split(y_train, num_folds, axis=0)
################################################################################
#                                 END OF YOUR CODE                             #
################################################################################

# A dictionary holding the accuracies for different values of k that we find
# when running cross-validation. After running cross-validation,
# k_to_accuracies[k] should be a list of length num_folds giving the different
# accuracy values that we found when using that value of k.
k_to_accuracies = {}

################################################################################
# TODO:                                                                        #
# Perform k-fold cross validation to find the best value of k. For each        #
# possible value of k, run the k-nearest-neighbor algorithm num_folds times,   #
# where in each case you use all but one of the folds as training data and the #
# last fold as a validation set. Store the accuracies for all fold and all     #
# values of k in the k_to_accuracies dictionary.                               #
################################################################################
for k in k_choices:
    temp_acrcy = []
    for i in range(num_folds):
        X_train_ = np.concatenate(X_train_folds[:i] + X_train_folds[i+1:])
        y_train_ = np.concatenate(y_train_folds[:i] + y_train_folds[i+1:])
        X_test   = X_train_folds[i]
        y_test   = y_train_folds[i]
        classifier = KNearestNeighbor()
        classifier.train(X_train_, y_train_)
        y_test_pred = classifier.predict(X_test,k,0)
#         y_test_pred = classifier.predict_labels(dists, k=k)
        num_correct = np.sum(y_test_pred == y_test)
        acrcy = float(num_correct) / len(y_test)
        print('K = %d | Got %d / %d correct => accuracy: %f' % (k, num_correct, len(y_train_), acrcy))
        temp_acrcy.append(acrcy)

    k_to_accuracies[k] = (np.average(temp_acrcy))

################################################################################
#                                 END OF YOUR CODE                             #
################################################################################
# Print out the computed accuracies
for k in sorted(k_to_accuracies):
    print('k = %d, accuracy = %f' % (k, k_to_accuracies[k]))
```

    K = 1 | Got 263 / 4000 correct => accuracy: 0.263000
    K = 1 | Got 257 / 4000 correct => accuracy: 0.257000
    K = 1 | Got 264 / 4000 correct => accuracy: 0.264000
    K = 1 | Got 278 / 4000 correct => accuracy: 0.278000
    K = 1 | Got 266 / 4000 correct => accuracy: 0.266000
    K = 3 | Got 239 / 4000 correct => accuracy: 0.239000
    K = 3 | Got 249 / 4000 correct => accuracy: 0.249000
    K = 3 | Got 240 / 4000 correct => accuracy: 0.240000
    K = 3 | Got 266 / 4000 correct => accuracy: 0.266000
    K = 3 | Got 254 / 4000 correct => accuracy: 0.254000
    K = 5 | Got 248 / 4000 correct => accuracy: 0.248000
    K = 5 | Got 266 / 4000 correct => accuracy: 0.266000
    K = 5 | Got 280 / 4000 correct => accuracy: 0.280000
    K = 5 | Got 292 / 4000 correct => accuracy: 0.292000
    K = 5 | Got 280 / 4000 correct => accuracy: 0.280000
    K = 8 | Got 262 / 4000 correct => accuracy: 0.262000
    K = 8 | Got 282 / 4000 correct => accuracy: 0.282000
    K = 8 | Got 273 / 4000 correct => accuracy: 0.273000
    K = 8 | Got 290 / 4000 correct => accuracy: 0.290000
    K = 8 | Got 273 / 4000 correct => accuracy: 0.273000
    K = 10 | Got 265 / 4000 correct => accuracy: 0.265000
    K = 10 | Got 296 / 4000 correct => accuracy: 0.296000
    K = 10 | Got 276 / 4000 correct => accuracy: 0.276000
    K = 10 | Got 284 / 4000 correct => accuracy: 0.284000
    K = 10 | Got 280 / 4000 correct => accuracy: 0.280000
    K = 12 | Got 260 / 4000 correct => accuracy: 0.260000
    K = 12 | Got 295 / 4000 correct => accuracy: 0.295000
    K = 12 | Got 279 / 4000 correct => accuracy: 0.279000
    K = 12 | Got 283 / 4000 correct => accuracy: 0.283000
    K = 12 | Got 280 / 4000 correct => accuracy: 0.280000
    K = 15 | Got 252 / 4000 correct => accuracy: 0.252000
    K = 15 | Got 289 / 4000 correct => accuracy: 0.289000
    K = 15 | Got 278 / 4000 correct => accuracy: 0.278000
    K = 15 | Got 282 / 4000 correct => accuracy: 0.282000
    K = 15 | Got 274 / 4000 correct => accuracy: 0.274000
    K = 20 | Got 270 / 4000 correct => accuracy: 0.270000
    K = 20 | Got 279 / 4000 correct => accuracy: 0.279000
    K = 20 | Got 279 / 4000 correct => accuracy: 0.279000
    K = 20 | Got 282 / 4000 correct => accuracy: 0.282000
    K = 20 | Got 285 / 4000 correct => accuracy: 0.285000
    K = 50 | Got 271 / 4000 correct => accuracy: 0.271000
    K = 50 | Got 288 / 4000 correct => accuracy: 0.288000
    K = 50 | Got 278 / 4000 correct => accuracy: 0.278000
    K = 50 | Got 269 / 4000 correct => accuracy: 0.269000
    K = 50 | Got 266 / 4000 correct => accuracy: 0.266000
    K = 100 | Got 256 / 4000 correct => accuracy: 0.256000
    K = 100 | Got 270 / 4000 correct => accuracy: 0.270000
    K = 100 | Got 263 / 4000 correct => accuracy: 0.263000
    K = 100 | Got 256 / 4000 correct => accuracy: 0.256000
    K = 100 | Got 263 / 4000 correct => accuracy: 0.263000
    k = 1, accuracy = 0.265600
    k = 3, accuracy = 0.249600
    k = 5, accuracy = 0.273200
    k = 8, accuracy = 0.276000
    k = 10, accuracy = 0.280200
    k = 12, accuracy = 0.279400
    k = 15, accuracy = 0.275000
    k = 20, accuracy = 0.279000
    k = 50, accuracy = 0.274400
    k = 100, accuracy = 0.261600


### Guardo solo el promedo para que quede una grafica mas clara y porque tiene sentido.


```python
# plot the raw observations
for k in k_choices:
  accuracies = k_to_accuracies[k]
  plt.scatter([k] * len([accuracies]), accuracies)

# plot the trend line with error bars that correspond to standard deviation
accuracies_mean = np.array([np.mean(v) for k,v in sorted(k_to_accuracies.items())])
accuracies_std = np.array([np.std(v) for k,v in sorted(k_to_accuracies.items())])
plt.errorbar(k_choices, accuracies_mean, yerr=accuracies_std)
plt.title('Cross-validation on k')
plt.xlabel('k')
plt.ylabel('Cross-validation accuracy')
plt.show()
```


![png](./1_src/img/output_22_0.png)



```python
# Based on the cross-validation results above, choose the best value for k,   
# retrain the classifier using all the training data, and test it on the test
# data. You should be able to get above 28% accuracy on the test data.
best_k = 1

classifier = KNearestNeighbor()
classifier.train(X_train, y_train)
y_test_pred = classifier.predict(X_test, k=best_k)

# Compute and display the accuracy
num_correct = np.sum(y_test_pred == y_test)
accuracy = float(num_correct) / num_test
print('Got %d / %d correct => accuracy: %f' % (num_correct, num_test, accuracy))
```

    Got 1000 / 500 correct => accuracy: 2.000000


**Inline Question #2:**
- Explain why k is an hyperparameter. Why do not just use k = 1 or k big enough?
- Suppose you have a training dataset ten times bigger. How do you think the optimal value of k will be affected?
- Someone suggests applying a dimensional reduction technique as a pre-processing step. Do you think it can help?

**Your Answer**: *K es parametro que utilizamos para considerar _k_ cantidades de vecinos, junto con el metodo para calcular la distancia entre los vecinos, son los parametros configurables para clasificar con k-NN.
Utilizar k=1 en la mayoria de los casos lleva a overfitting. Con k=1 el vecino mas cercano es si mismo lo que lleva a una precisión de 1. Por otro lado utilizar un K muy alto puede llevar a clasificar todo con la clase con mayor ocurrencias ya que la mayoria de los vecinos (si consideramos un numero muy alto) suelen ser de la clase predominante.
En caso de tener mas datos de entrenamiento con k pueden ocurrir dos situaciones:*

* Los datos nuevos siguen una distribucion muy similar a los anteriores por lo cual k no deberia de verse muy afectado ya que los vecinos segurian siendo de la misma clase.
* Los datos siguen otra distribucion lo cual puede requerir un k mayor ya que cambiaria la clase de los vecinos cercanos.

*Si se reducen las dimensiones, digamos, por ejemplo, promediando vecinos de una misma clase, podriamos de pasar de tener 10 de una clase a solo 1 en un área. Suena bien, menos elementos que comparar, pero, en caso de hacer esto podriamos estar perdiendo el sentido del k-nn que es encontrar la clase predominante. Por otra parte, si se considera como parte de preprocesamiento para eliminar ruido, si, es una muy buena idea.*
