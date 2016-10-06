# Author_Classifier

## Goal 
Create a author classifier using books from Poject Gutenberg 

## Approach

After some research on the topic I found an approach that is most interesting to me
because I have some experience in applying CNN to images.
I followed the schema outlined in Kim (2014). I also found helpful following posts:
Very nice overview on using CNN for NLP with a lot of useful links at the end
http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/#more-348
Thorough explanation of the Kim's approach and implementation in Tensor Flow
http://www.wildml.com/2015/12/implementing-a-cnn-for-text-classification-in-tensorflow/

## Basic Idea

Here I will brefly summarize the approach. I will just repeat what is described in the KIm's paper and links above. For more information please refer to the links and Kim (2014) paper.

In order to use CNNs for sentence classification Kim suggested to use fixed lenght word embedding and
create a matrix of the size (number of words in sentence) x (embedding dimensionality). This is already similar to a single channel image matrix. One can also add more channels by utilizing different types of embeddings. The different sentence lenght are handled by just adding a padding and fixing the maximum length of the sentence. This will allow batch training.
Once sentente translated to an image like representation convolutional operations can be used. The main difference between standard convolutional kernels for image processing and text processing is that kernels for text processing should be as wide as embeddings. For example Kim suggested to use kernals of size 3,4,5x(embedding dimensionality). This type of kernels will convolve phrases of 3, 4, 5 word length. After applying convolutions and ReLu activations max-pooling is used to create a feature vector and finaly fully connected layer with drop out and SoftMax for classification. For the experiments here I used word2vec 300 dimentional embeddings.

## Data Preprocessing

I did a simple preprocessing to create a training data set. I took about 60 books and partition them by sentences. After that I choose 25 books which has at least 2000 sentences. If book has more that that I just randomly picked a sentence. The preprocessing is a really crude. I didn't try to utilize a logical structure of the books. Ideally I would probably get rid of prefaces and some other insignificant to author style identification parts of the book. Here I just wanted to come up with a first iteration of the model and after that try to improve the model as well as input data. At the same time this approach will probably show what is really affecting the quality of the model. Since, I don't have wide experince with NLP, for me it will be interesting to see how quality of the parsing affects the quality of the model. 
So, the data set consists of 26 authors, each author contributes 2000 sentences. In a nootebook above I create an output file for [sent-conv-torch] (https://github.com/harvardnlp/sent-conv-torch). I had to fix some simple problems in their code, but overall the code was working out of the box. I also had to set up EC2 gpu instance to run the experiments since on my laptop the problem was intracable.

## Results
I use default 10-fold cross validation which is provided as an option in [sent-conv-torch] (https://github.com/harvardnlp/sent-conv-torch).
The best results I got by using non-static w2v embeddings. 
 
Model | dev | test
---| ---| ---
static |0.5359208847299    | 0.53507692307692  
non static | 0.58781369629945  |0.58798076923077
multy channel| N/A    | ~57%
random | N/A  | ~53% 

## Thoughts on how to improve the model
 1) More data. More data is always better. It will help with overfitting that I currently observe.
 2) Better data. As I mentioned before I used very crude way to pick the sentences out and i think it can be greately improved.
 3) Instead of sentence level we could probably use 2 consecutive sentences or even a paragraph to get a wider sample of the author style.
    I see that for example short sentence might be enough to detect sentiment (this what this particular code was used for), however it migh not be enought to detect a style.
 4) It might help to play around with network architecture for example to imcreasea filter length.       
