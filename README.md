# Customer goods Classification - NLP & DIP

![bannière1](https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/dc9a9712-81dd-45ce-a1e2-ecd952e4e3fe)

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

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/61082c5e-2204-45d8-8101-c98e35f58bd1" height="400">

Descriptions are between 13 and 587 words long, with a majority between 13 and 100.

#### Product Name

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/dc024de7-754c-4cf5-be58-a40299af7294" height="400">
Product names are between 2 and 27 words long, with a majority between 4 and 10 words.

These two features will be used for the NLP part of the classification model.

#### Categories

7 categories are identified for the products.

<img src="https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/da81f2f9-3e65-4239-8c8a-a73fa80762c0" height="400">

This is a perfect distribution.
You can also look at wordclouds by product category :
- **Kitchen**
![kitchen](https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/9501d043-3773-4b91-bdd5-d64bd4b9daea)
- **Computers**
![computers](https://github.com/pgrondein/goods_classification_NLP_DIP/assets/113172845/8dba4cb1-4160-42a6-9262-f0c9ea0943db)

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

