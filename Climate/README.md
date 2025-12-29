
# Natural Language-Based Climate Technology Classification Study

**Project Period:** December 2021 - February 2022

The project's task is to develop an algorithm for classifying national R&D projects according to a predefined 'Climate Technology Classification System.' We utilized text content, such as project titles, research objectives, and summaries, to accurately assign climate technology labels.

**Dacon Competition Link:** [https://dacon.io/competitions/official/235744/overview/description](https://dacon.io/competitions/official/235744/overview/description)



> **Understanding Natural Language Processing (NLP)**  
NLP is an AI field focused on enabling computers to understand, interpret, and process human language. Our project's NLP workflow typically involved:  **Tokenization** (breaking text into units), **Stopword Removal** (filtering out common, less meaningful words), **Encoding** (converting text into numerical representations), and **Padding** (standardizing text lengths).


## 0. Data Overview

* `train.csv`: Contains 174,304 observations with 13 variables, including the **climate technology label**.
* `test.csv`: Features 43,576 observations across 12 variables, without the label for prediction.
* `sample_submission.csv`: An example submission file (43,576 rows, 2 columns).
* `labels_mapping.csv`: Metadata linking labels to the climate technology classification system.

### Key Variables:

* `index`: Unique record identifier
* `제출년도` (Submission Year)
* `사업명` (Project Name)
* `사업_부처명` (Ministry/Department of Project)
* `계속과제여부` (Continuation Project Status)
* `내역사업명` (Detailed Project Name)
* `과제명` (Task Name/Title)
* `요약문_연구목표` (Abstract_Research Goal)
* `요약문_연구내용` (Abstract_Research Content)
* `요약문_기대효과` (Abstract_Expected Outcome)
* `요약문_한글키워드` (Abstract_Korean Keywords)
* `요약문_영문키워드` (Abstract_English Keywords)
* **`label`**: The target variable representing the climate technology classification.


## 1. Konlpy for Korean NLP

Given the morphological complexities of the Korean language, we used **Konlpy** for natural language processing. After evaluating various morphological analyzers like Kkma, Mecab, and Okt, we selected Okt for its balanced performance.

### 1) Exploratory Data Analysis (EDA) on `과제명` (Task Name/Title)

Our initial data exploration focused heavily on the `과제명` variable:

* Length analysis : We analyzed the distribution of title lengths and handled instances that were either too short or excessively long.
* Duplicate Handling: We identified and addressed cases where identical project titles were surprisingly associated with different labels.
* Text Cleaning: We processed and normalized English characters, numbers, and special characters within the titles to ensure data consistency.

### 2) Preprocessing

Our text preprocessing pipeline was crucial for preparing the data for the neural network:

* **Stopword Removal:** We systematically removed common, less informative words (stopwords) that could add noise. This included:
    * Identifying and removing Korean particles, endings, suffixes, and conjunctions.
    * Addressing words incorrectly classified by the morphological analyzer.
    * Eliminating generally meaningless or overly frequent terms.
    * Removing single-character words.
* **Tokenization, Integer Encoding, & Padding:**
    * Tokenization: We broke down raw text into individual words or morphemes, creating a comprehensive vocabulary.
    * Integer Encoding: Each unique token was then converted into a numerical representation.
    * Padding: To enable efficient batch processing by the neural network, all sequences were standardized to a uniform length. We applied post-padding, adding zeros to the end of shorter sequences.
* **Word Embedding:** Instead of sparse representations (like one-hot encoding), we transformed words into *dense vector forms*. These low-dimensional, real-valued vectors were learned during training, allowing words with similar semantic meanings to have similar vector representations.



## 2. Model Development

### 1) Neural Network Architecture

We built a deep learning model using TensorFlow's `Sequential()` API. The architecture was designed to effectively process the embedded text features for classification:

* Embedding Layer: Converts integer-encoded input sequences into dense word embeddings.
* Batch Normalization Layer: Used to stabilize and accelerate training by normalizing the activations of previous layers.
* Pooling Layer (Global Average Pooling - GAP): Efficiently reduces the dimensionality of feature maps, capturing salient features.
* (Optional) LSTM Layer: While not always included in the final model, we explored Long Short-Term Memory (LSTM) layers to capture long-range dependencies within the sequential text data.
* Dense Layer (ReLU Activation): Fully connected layers with a ReLU activation function for non-linear transformations.
* (Optional) Dropout Layer: Incorporated for **regularization** to prevent overfitting. This layer randomly drops units during training, forcing the network to learn more robust features.
* Dense Layer (Softmax Activation): The final output layer, using a **Softmax activation function**, which provides probability distributions over the climate technology classes.

We extensively experimented with adding and modifying layers to achieve optimal performance.

### 2) Model Compilation

We configured the model for training with the following parameters:

* Loss Function: Categorical Cross-Entropy, ideal for multi-class classification problems.
* Optimizer: Adam optimizer, chosen for its adaptive learning rate capabilities and strong performance across various deep learning tasks.
* Metrics: Accuracy, used to monitor and evaluate the model's classification performance throughout the training process.

### 3) Model Training (Fit)

* Epochs: 20      
* Data Split: 8:2 ratio for training and validation   
* Early Stopping: To prevent overfitting. This callback automatically halts training when the validation performance (e.g., validation loss) ceases to improve after a specified number of epochs.



## 3. Prediction & Submission

Following successful model training and validation, we generated predictions on the unseen `test.csv` dataset. These predictions were then formatted according to the competition's specifications and prepared for submission.  






---
Reference

* Wikidocs 딥러닝을 이용한 자연어 처리 입문      
텍스트 전처리 : https://wikidocs.net/21694   
딥러닝(Keras) : https://wikidocs.net/32105    
워드 임베딩 : https://wikidocs.net/22644     

* Understaing NLP concepts 
Text preprocessing : https://marketingscribbler.tistory.com/20#Sentence_Tokenization         
NLP 정리 : https://han-py.tistory.com/category/%EC%9D%B8%EA%B3%B5%EC%A7%80%EB%8A%A5%28Artificial%20Intelligence%29/%EC%9E%90%EC%97%B0%EC%96%B4%20%EC%B2%98%EB%A6%AC%28natural%20language%20processing%29?page=1          

* Tensorflow Model       
BERT example code   
https://github.com/NLP-kr/tensorflow-ml-nlp-tf2/blob/master/7.PRETRAIN_METHOD/7.2.1.bert_finetune_NSMC.ipynb      
RNN example code     
https://ml-ko.kr/dl-with-python/6.2-understanding-recurrent-neural-networks.html     
