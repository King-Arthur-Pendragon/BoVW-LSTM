Bag of Visual Words Image Feature Generator
============================================

This is an implementation of [bag of visual words model][1] in Python for feature extraction in video frames.

The current repository is just one layer of a framework for video classification, composed by:
- Bag-of-Visual-Words ( Feature extraction for each frame) 
- Long-Short Term Memory ( Maximizing Temporal Dependencies of features)
- Softmax Classifier ( Classify the video, given the outputs of LSTM)


The script `learning.py` will generate a visual vocabulary using the images provided by process_video.py provided set of already classified images.
After the learning phase `classify.py` will use the generated vocabulary and the trained classifier to predict the class for any image given to the script by the user.

The learning consists of:

1. Extracting local features of all the dataset images
2. Generating a codebook of visual words with clustering of the features
3. Aggregating the histograms of the visual words for each of the traning images
4. Feeding the histograms to the classifier to train a model

The classification consists of:

1. Extracting local features of the to be classified image
2. Aggregating the histograms of the visual words for the image using the prior generated codebook
4. Feeding the histogram to the classifier to predict a class for the image

This code relies on:

 - SIFT features for local features
 - k-means for generation of the words via clustering
 - SVM as classifier using the LIBSVM library

### Example use:
  
You train the classifier for a specific dataset with: 

    python learn.py -d path_to_folders_with_images

To classify images use:

    python classify.py -c path_to_folders_with_images/codebook.file -m path_to_folders_with_images/trainingdata.svm.model images_you_want_to_classify

The dataset should have following structure, where all the images belonging to one class are in the same folder:

    .
    |-- path_to_folders_with_images
    |    |-- class1
    |    |-- class2
    |    |-- class3
    ...
    |    └-- classN

The folder can have any name. One example dataset would be the [Caltech 101 dataset][2].

### Prerequisites:

To install the necessary libraries run following code from working directory:

    # installing libsvm
    wget -O libsvm.tar.gz http://www.csie.ntu.edu.tw/~cjlin/cgi-bin/libsvm.cgi?+http://www.csie.ntu.edu.tw/~cjlin/libsvm+tar.gz
    tar -xzf libsvm.tar.gz
    mkdir libsvm
    cp -r libsvm-*/* libsvm/
    rm -r libsvm-*/
    cd libsvm
    make
    cp tools/grid.py ../grid.py
    cd ..
    
    # installing sift
    wget http://www.cs.ubc.ca/~lowe/keypoints/siftDemoV4.zip
    unzip siftDemoV4.zip
    cp sift*/sift sift
    

#### Notes
If you get an `IOError: SIFT executable not found` error, try `sudo apt-get install libc6-i386`. `sift` is a 32Bit executable and you need to install additional libraries to make it run on 64Bit systems. [More info and background on the misleading error message on unix.stackexchange](http://unix.stackexchange.com/a/13409/11381)
    
### References:

#### Libsvm:

Chih-Chung Chang and Chih-Jen Lin, LIBSVM : a library for support vector machines. ACM Transactions on Intelligent Systems and Technology, 2:27:1--27:27, 2011. Software available at http://www.csie.ntu.edu.tw/~cjlin/libsvm

#### SIFT:
David G. Lowe, "Distinctive image features from scale-invariant keypoints," International Journal of Computer Vision, 60, 2 (2004), pp. 91-110.

#### sift.py:
Taken from http://www.janeriksolem.net/2009/02/sift-python-implementation.html

#### libsvm.py:
Addapted from easy.py contained in the LIBSVM packet by Chih-Chung Chang and Chih-Jen Lin.

[1]: https://en.wikipedia.org/wiki/Bag-of-words_model_in_computer_vision
[2]: http://www.vision.caltech.edu/Image_Datasets/Caltech101/
[3]: https://github.com/shackenberg/Minimal-Bag-of-Visual-Words-Image-Classifier/blob/master/learn.py
[4]: https://github.com/shackenberg/Minimal-Bag-of-Visual-Words-Image-Classifier/blob/master/classify.py
