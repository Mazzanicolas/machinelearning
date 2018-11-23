
# Image Gradients
In this notebook we'll introduce the TinyImageNet dataset and a deep CNN that has been pretrained on this dataset. You will use this pretrained model to compute gradients with respect to images, and use these image gradients to produce class saliency maps and fooling images.


```python
# As usual, a bit of setup

import time, os, json
import numpy as np
import skimage.io
import matplotlib.pyplot as plt

from cs231n.classifiers.pretrained_cnn import PretrainedCNN
from cs231n.data_utils import load_tiny_imagenet
from cs231n.image_utils import blur_image, deprocess_image_2016

%matplotlib inline
plt.rcParams['figure.figsize'] = (10.0, 8.0) # set default size of plots
plt.rcParams['image.interpolation'] = 'nearest'
plt.rcParams['image.cmap'] = 'gray'

# for auto-reloading external modules
# see http://stackoverflow.com/questions/1907993/autoreload-of-modules-in-ipython
%load_ext autoreload
%autoreload 2
```

# Introducing TinyImageNet

The TinyImageNet dataset is a subset of the ILSVRC-2012 classification dataset. It consists of 200 object classes, and for each object class it provides 500 training images, 50 validation images, and 50 test images. All images have been downsampled to 64x64 pixels. We have provided the labels for all training and validation images, but have withheld the labels for the test images.

We have further split the full TinyImageNet dataset into two equal pieces, each with 100 object classes. We refer to these datasets as TinyImageNet-100-A and TinyImageNet-100-B; for this exercise you will work with TinyImageNet-100-A.

You should have already downloaded the data by changing to the cs231n/datasets directory and running the script get_assignment3_data.sh. If you haven't yet done so, go into the `cs231n/datasets` directory and run the script `get_tiny_imagenet_a.sh`. Then run the following code to load the TinyImageNet-100-A dataset into memory.

NOTE: The full TinyImageNet-100-A dataset will take up about 250MB of disk space, and loading the full TinyImageNet-100-A dataset into memory will use about 2.8GB of memory.


```python
data = load_tiny_imagenet('cs231n/datasets/tiny-imagenet-100-A', subtract_mean=True)
```

    loading training data for synset 20 / 100
    loading training data for synset 40 / 100
    loading training data for synset 60 / 100
    loading training data for synset 80 / 100
    loading training data for synset 100 / 100


# TinyImageNet-100-A classes
Since ImageNet is based on the WordNet ontology, each class in ImageNet (and TinyImageNet) actually has several different names. For example "pop bottle" and "soda bottle" are both valid names for the same class. Run the following to see a list of all classes in TinyImageNet-100-A:


```python
for i, names in enumerate(data['class_names']):
  print(i, ' '.join('"%s"' % name for name in names))
```

    0 "Egyptian cat"
    1 "reel"
    2 "volleyball"
    3 "rocking chair" "rocker"
    4 "lemon"
    5 "bullfrog" "Rana catesbeiana"
    6 "basketball"
    7 "cliff" "drop" "drop-off"
    8 "espresso"
    9 "plunger" "plumber's helper"
    10 "parking meter"
    11 "German shepherd" "German shepherd dog" "German police dog" "alsatian"
    12 "dining table" "board"
    13 "monarch" "monarch butterfly" "milkweed butterfly" "Danaus plexippus"
    14 "brown bear" "bruin" "Ursus arctos"
    15 "school bus"
    16 "pizza" "pizza pie"
    17 "guinea pig" "Cavia cobaya"
    18 "umbrella"
    19 "organ" "pipe organ"
    20 "oboe" "hautboy" "hautbois"
    21 "maypole"
    22 "goldfish" "Carassius auratus"
    23 "potpie"
    24 "hourglass"
    25 "seashore" "coast" "seacoast" "sea-coast"
    26 "computer keyboard" "keypad"
    27 "Arabian camel" "dromedary" "Camelus dromedarius"
    28 "ice cream" "icecream"
    29 "nail"
    30 "space heater"
    31 "cardigan"
    32 "baboon"
    33 "snail"
    34 "coral reef"
    35 "albatross" "mollymawk"
    36 "spider web" "spider's web"
    37 "sea cucumber" "holothurian"
    38 "backpack" "back pack" "knapsack" "packsack" "rucksack" "haversack"
    39 "Labrador retriever"
    40 "pretzel"
    41 "king penguin" "Aptenodytes patagonica"
    42 "sulphur butterfly" "sulfur butterfly"
    43 "tarantula"
    44 "lesser panda" "red panda" "panda" "bear cat" "cat bear" "Ailurus fulgens"
    45 "pop bottle" "soda bottle"
    46 "banana"
    47 "sock"
    48 "cockroach" "roach"
    49 "projectile" "missile"
    50 "beer bottle"
    51 "mantis" "mantid"
    52 "freight car"
    53 "guacamole"
    54 "remote control" "remote"
    55 "European fire salamander" "Salamandra salamandra"
    56 "lakeside" "lakeshore"
    57 "chimpanzee" "chimp" "Pan troglodytes"
    58 "pay-phone" "pay-station"
    59 "fur coat"
    60 "alp"
    61 "lampshade" "lamp shade"
    62 "torch"
    63 "abacus"
    64 "moving van"
    65 "barrel" "cask"
    66 "tabby" "tabby cat"
    67 "goose"
    68 "koala" "koala bear" "kangaroo bear" "native bear" "Phascolarctos cinereus"
    69 "bullet train" "bullet"
    70 "CD player"
    71 "teapot"
    72 "birdhouse"
    73 "gazelle"
    74 "academic gown" "academic robe" "judge's robe"
    75 "tractor"
    76 "ladybug" "ladybeetle" "lady beetle" "ladybird" "ladybird beetle"
    77 "miniskirt" "mini"
    78 "golden retriever"
    79 "triumphal arch"
    80 "cannon"
    81 "neck brace"
    82 "sombrero"
    83 "gasmask" "respirator" "gas helmet"
    84 "candle" "taper" "wax light"
    85 "desk"
    86 "frying pan" "frypan" "skillet"
    87 "bee"
    88 "dam" "dike" "dyke"
    89 "spiny lobster" "langouste" "rock lobster" "crawfish" "crayfish" "sea crawfish"
    90 "police van" "police wagon" "paddy wagon" "patrol wagon" "wagon" "black Maria"
    91 "iPod"
    92 "punching bag" "punch bag" "punching ball" "punchball"
    93 "beacon" "lighthouse" "beacon light" "pharos"
    94 "jellyfish"
    95 "wok"
    96 "potter's wheel"
    97 "sandal"
    98 "pill bottle"
    99 "butcher shop" "meat market"


# Visualize Examples
Run the following to visualize some example images from random classses in TinyImageNet-100-A. It selects classes and images randomly, so you can run it several times to see different images.


```python
# Visualize some examples of the training data
classes_to_show = 7
examples_per_class = 5

class_idxs = np.random.choice(len(data['class_names']), size=classes_to_show, replace=False)
for i, class_idx in enumerate(class_idxs):
  train_idxs, = np.nonzero(data['y_train'] == class_idx)
  train_idxs = np.random.choice(train_idxs, size=examples_per_class, replace=False)
  for j, train_idx in enumerate(train_idxs):
    img = deprocess_image_2016(data['X_train'][train_idx], data['mean_image'])
    plt.subplot(examples_per_class, classes_to_show, 1 + i + classes_to_show * j)
    if j == 0:
      plt.title(data['class_names'][class_idx][0])
    plt.imshow(img)
    plt.gca().axis('off')

plt.show()
```


![png](output_7_0.png)


# Pretrained model
We have trained a deep CNN for you on the TinyImageNet-100-A dataset that we will use for image visualization. The model has 9 convolutional layers (with spatial batch normalization) and 1 fully-connected hidden layer (with batch normalization).

To get the model, run the script `get_pretrained_model.sh` from the `cs231n/datasets` directory. After doing so, run the following to load the model from disk.


```python
model = PretrainedCNN(h5_file='cs231n/datasets/pretrained_model.h5')
```

## Pretrained model performance
Run the following to test the performance of the pretrained model on some random training and validation set images. You should see training accuracy around 90% and validation accuracy around 60%; this indicates a bit of overfitting, but it should work for our visualization experiments.


```python
batch_size = 100

# Test the model on training data
mask = np.random.randint(data['X_train'].shape[0], size=batch_size)
X, y = data['X_train'][mask], data['y_train'][mask]
y_pred = model.loss(X).argmax(axis=1)
print('Training accuracy: ', (y_pred == y).mean())

# Test the model on validation data
mask = np.random.randint(data['X_val'].shape[0], size=batch_size)
X, y = data['X_val'][mask], data['y_val'][mask]
y_pred = model.loss(X).argmax(axis=1)
print('Validation accuracy: ', (y_pred == y).mean())
```

    Training accuracy:  0.89
    Validation accuracy:  0.59


# Saliency Maps
Using this pretrained model, we will compute class saliency maps as described in Section 3.1 of [1].

As mentioned in Section 2 of the paper, you should compute the gradient of the image with respect to the unnormalized class score, not with respect to the normalized class probability.

You will need to use the `forward` and `backward` methods of the `PretrainedCNN` class to compute gradients with respect to the image. Open the file `cs231n/classifiers/pretrained_cnn.py` and read the documentation for these methods to make sure you know how they work. For example usage, you can see the `loss` method. Make sure to run the model in `test` mode when computing saliency maps.

[1] Karen Simonyan, Andrea Vedaldi, and Andrew Zisserman. "Deep Inside Convolutional Networks: Visualising
Image Classification Models and Saliency Maps", ICLR Workshop 2014.


```python
def compute_saliency_maps(X, y, model):
    """
    Compute a class saliency map using the model for images X and labels y.
  
    Input:
    - X: Input images, of shape (N, 3, H, W)
    - y: Labels for X, of shape (N,)
    - model: A PretrainedCNN that will be used to compute the saliency map.
  
    Returns:
    - saliency: An array of shape (N, H, W) giving the saliency maps for the input
    images.
    """
    saliency = None
    ##############################################################################
    # TODO: Implement this function. You should use the forward and backward     #
    # methods of the PretrainedCNN class, and compute gradients with respect to  #
    # the unnormalized class score of the ground-truth classes in y.             #
    ##############################################################################
    N, C, H, W = X.shape
    saliency   = np.zeros((N, H, W))
    
    scores, cache = model.forward(X)
#     opt = (scores[np.arange(N), y] - 0.01*np.sqrt(np.sum(X**2)))
#     print('Optimization: {0}'.format(opt))
    ds = np.zeros_like(scores)
    ds[np.arange(N), y] = 1
    dx, grads = model.backward(ds, cache)
    
    saliency += np.abs(np.sum(dx, axis=1))
    ##############################################################################
    #                             END OF YOUR CODE                               #
    ##############################################################################
    return saliency
```

Once you have completed the implementation in the cell above, run the following to visualize some class saliency maps on the validation set of TinyImageNet-100-A.


```python
def show_saliency_maps(mask):
    mask = np.asarray(mask)
    X = data['X_val'][mask]
    y = data['y_val'][mask]

    saliency = compute_saliency_maps(X, y, model)

    for i in range(mask.size):
        plt.subplot(2, mask.size, i + 1)
        plt.imshow(deprocess_image_2016(X[i], data['mean_image']))
        plt.axis('off')
        plt.title(data['class_names'][y[i]][0])
        plt.subplot(2, mask.size, mask.size + i + 1)
        plt.title(mask[i])
        plt.imshow(saliency[i])
        plt.axis('off')
    plt.gcf().set_size_inches(10, 4)
    plt.show()

# Show some random images
mask = np.random.randint(data['X_val'].shape[0], size=5)
show_saliency_maps(mask)
  
# These are some cherry-picked images that should give good results
show_saliency_maps([128, 3225, 2417, 1640, 4619])
```


![png](output_15_0.png)



![png](output_15_1.png)


# Fooling Images
We can also use image gradients to generate "fooling images" as discussed in [2]. Given an image and a target class, we can perform gradient ascent over the image to maximize the target class, stopping when the network classifies the image as the target class. Implement the following function to generate fooling images.

[2] Szegedy et al, "Intriguing properties of neural networks", ICLR 2014


```python
def make_fooling_image(X, target_y, model):
    """
    Generate a fooling image that is close to X, but that the model classifies
    as target_y.
  
    Inputs:
    - X: Input image, of shape (1, 3, 64, 64)
    - target_y: An integer in the range [0, 100)
    - model: A PretrainedCNN
  
    Returns:
    - X_fooling: An image that is close to X, but that is classifed as target_y
      by the model.
    """
    X_fooling = X.copy()
    ##############################################################################
    # TODO: Generate a fooling image X_fooling that the model will classify as   #
    # the class target_y. Use gradient ascent on the target class score, using   #
    # the model.forward method to compute scores and the model.backward method   #
    # to compute image gradients.                                                #
    #                                                                            #
    # HINT: For most examples, you should be able to generate a fooling image    #
    # in fewer than 100 iterations of gradient ascent.                           #
    ##############################################################################
    N, C, H, W = X_fooling.shape
    
    iterations = 150
    y_pred = None
    
    for _ in range(iterations):
        if(y_pred == target_y):
            break
        print('Trying to predict {0} but predicted {1}'.format(target_y, y_pred))
        scores, cache = model.forward(X_fooling)
        loss = scores[np.arange(N), target_y]
        
        ds = np.zeros_like(scores)
        ds[np.arange(N), target_y] = 1
        
        dx, grads = model.backward(ds, cache)
        
        X_fooling += 1000*dx
        
        y_pred = scores[0].argmax()
    ##############################################################################
    #                             END OF YOUR CODE                               #
    ##############################################################################
    return X_fooling
```

Run the following to choose a random validation set image that is correctly classified by the network, and then make a fooling image.


```python
# Find a correctly classified validation image
while True:
    i = np.random.randint(data['X_val'].shape[0])
    X = data['X_val'][i:i+1]
    y = data['y_val'][i:i+1]
    y_pred = model.loss(X)[0].argmax()
    if y_pred == y: break

y_0=y[0]
target_y = 67
X_fooling = make_fooling_image(X, target_y, model)

# Make sure that X_fooling is classified as y_target
scores = model.loss(X_fooling)
assert scores[0].argmax() == target_y, 'The network is not fooled!'

# Show original image, fooling image, and difference
plt.subplot(1, 3, 1)
plt.imshow(deprocess_image_2016(X, data['mean_image']))
plt.axis('off')
plt.title(data['class_names'][y_0][0])
plt.subplot(1, 3, 2)
plt.imshow(deprocess_image_2016(X_fooling, data['mean_image'], renorm=True))
plt.title(data['class_names'][target_y][0])
plt.axis('off')
plt.subplot(1, 3, 3)
plt.title('Difference')
plt.imshow(deprocess_image_2016(X - X_fooling, data['mean_image']))
plt.axis('off')
plt.show()
```

    Trying to predict 67 but predicted None
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 55
    Trying to predict 67 but predicted 5



![png](output_19_1.png)

