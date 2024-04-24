# Consumer goods Classification - NLP & DIP

![bannière1](https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/f078eac1-246a-49df-a220-4b4053667563)

## Problem Definition

On a marketplace  site, sellers offer items to buyers by posting images of the item and providing a detailed description. In order to make the user experience (sellers and buyers) as smooth as possible, and with a view to scaling up, automation of item classification is necessary.

This project studies the feasibility of a **classification engine** of articles into different predefined categories, with a sufficient level of precision, based on an image and a description.

The methodology will be as follows:
- A pre-processing step on text and image
- A step for extracting features from text and image
- A reduction of variables
- A clustering step based on the extracted variables
- Finally, a comparison to the real categories of the products for evaluation of the classification model

## Data Collection

A data sheet contains all the product information, including the description necessary for the NLP, as well as a product image file.

## Exploratory Data Analysis

### Single feature analysis
#### Description

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/c9294aea-a160-4b74-97ec-537310ba3cb2" height="400">

Descriptions are between 13 and 587 words long, with a majority between 13 and 100.

#### Product Name

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/8970dfa6-d780-4843-a5ac-1d42fb287e52" height="400">

Product names are between 2 and 27 words long, with a majority between 4 and 10 words.

These two features will be used for the NLP part of the classification model.

#### Categories

7 categories are identified for the products.

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/5a1cbeb4-6181-48d4-a86e-c0778ad08d1b" height="400">

This is a perfect distribution.
You can also look at wordclouds by product category :

- **Kitchen**
  
<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/cea1b1bc-d120-4b5d-a823-36712f0a59bb" height="200">

- **Computers**
  
<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/0ed2fa9a-f2f1-4d1f-8468-c2a7f5fce54b" height="200">

## Data Pre-processing

A preprocessing step is required for text and images.

### Text 

The “Description” and “Product name” variables are grouped together.

The following treatments are then applied:
- *lower*: removes capital letters
- *expand_contraction*: the text being in English, it is necessary to expand the contractions
- *noise_removal*: remove urls, HTML tags, non-ASCII characters…
- *punctuation removal*: removes punctuation
- *number removal*: removes numbers

We continue with:
- *Tokenization*: separates the sentences into a list of tokens
- *Stopwords*: removes very frequent words without impact
- *Lemmatization*: keeps only the root of a word taking into account the context.

### Image

In order to make feature extraction more efficient, it is necessary to pre-process images.

For *Bag-of-Features* techniques such as Sift or ORB, apply in order:

- *grey scale*: converts the image to grayscale
- *histogram equalization*: contrast improvement
- *histogram stretching*: exposure correction
- *mean filter*: noise attenuation by local averaging

However, CNN techniques such as VGG16 have their own pre-processing implemented in their library.

![image preprocessing](https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/15cff901-3fcc-485d-925a-94cea4db65ce)

## Model

Text and image parts are developed and evaluated separately. They are then evaluated together, in order to determine the interest of coupling the two methods.

### Feature Extraction
#### Text

Two types of methods are available:

- *bag-of-word* type algorithms: gives a reduced and simplified representation of a text document in the form of **vectors** based on specific criteria such as the **frequency** of words. 
Example: CountVectorizer, TF-IDF.
**Pros**: fast, works with unknown words.
**Cons**: does not consider the place of the word in the sentence, and does not capture the meaning of the word.
- the so-called *Sentence Embedding* methods: give digital vector representations of the semantics or meaning of words, including the literal and implicit meaning. Thus, these word vectors can capture their connotation, and are combined into one dense vector per sentence. 
Example: Word2Vec, BERT, USE
**Pros**: often pre-trained, takes word position into account, understands semantics.
**Cons**: does not consider words outside the corpus, more complex, “black box”.

#### Image
For feature extraction from images, several types of methods are available.

- *Bag-of-visual-words* algorithms: takes an image and returns key points of this image in the form of features/vectors, the digital fingerprint of the image, invariant regardless of transformations.
Example: SIFT, ORB
- *CNN Transfer Learning* algorithms: pre-trained convolutional neural network taking an input image and automatically returning the features of this image, by automatic extraction and prioritization of said features.
Example: VGG16, in Standalone Feature Extractor

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/95c64e25-eea8-41f2-b2ab-38fd3dfec272" height="300">
<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/45cf7f44-954a-4b17-941c-4d27055790ba" height="200">

### Reduction & Clustering

The reduction method is t-SNE.
The clustering method is k-Means.

We then draw a projection of the products with real and calculated categorization.

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/bad10dff-8e0e-4171-a52b-489a9dd0b579" height="400">

### Evaluation

In order to evaluate the accuracy of the method used and the efficiency of feature extraction algorithm tested, the ARI (Adjusted Rand Index) is calculated, which gives a measure of similarity between calculated categories and the real ones. Computation time is also considered, another important element.

#### Text

| Algorithm | ARI | Computation time |
| :---: | :---: | :---: |
| `Countvectorizer`  | 0.49 | 19 s |
| `TF-IDF`  | 0.50 | 18 s |
| `Word2Vec`  | 0.41 | 15 s |
| `BERT`  | 0.32 | 2 min 30 s |
| `USE`  | 0.63 | 10 s |

USE seems to be the correct method for our problem for the NLP part.

#### Image

| Algorithm | ARI | Computation time |
| :---: | :---: | :---: |
| `SIFT`  | 0.04 | 10 min 45 s |
| `ORB`  | 0.03 | 1 min 50 s |
| `VGG16`  | 0.45 | 4 min |

VGG16 is the most efficient algorithm for the DIP part.

#### Text & Image

| Algorithm | ARI | Computation time |
| :---: | :---: | :---: |
| `USE + VGG16`  | 0.65 | 4 min 20 s |

## Conclusion

The association of classification model for image and text makes it possible to reach an ARI of 0.65. The feasibility of the classification engine is therefore proven.

From a performance + computation time point of view, the use of text only can be considered.
