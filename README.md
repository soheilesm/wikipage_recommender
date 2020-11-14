* __Author__: Soheil Esmaeilzadeh
* __Date__: Nov. 6th - 2020
* __Email__: soes@stanford.edu

# Wikipedia Article Recommendation
This repo presents a content-based recommendation framework for recommending users wikipedia pages. A recommendation system that given an input Wikipedia article, recommends a series of  articles to read.

### Problem Description
Here we present a recommendation system that is based on the very basic principles of the Content-Based Recommendation approach [[1](https://link.springer.com/chapter/10.1007/978-3-540-72079-9_10)] for recommending users Wikipedia articles. In summary, a content-based recommender system utilizes the features of the product(s) in order to recommend other product(s) similar to what a user has liked, or purchased, or used.

### Dataset Gathering and Cleaning
Downloading all Wikipedia articles leads to 78 GB of data when unzipped. Accordingly, as the dataset, here we only gather a few thousands of articles from Wikipedia. 

In order to gather such articles, we use Wikipedia's Python API that can be accessed through this [link](https://pypi.org/project/wikipedia/). This API could be installed by using, <code>pip</code>, as the standard package-management system for Python, using the command <code>pip install wikipedia</code>. 

In this work, we only gather a random set of <code>num_random_articles=25000</code> wikipedia articles. Although, the more articles we gather the better the recommendation system could work in terms of recommending appropriate articles to the users.

As a very simple cleaning step, we get rid of non-contextual components in the text, e.g. <code>\n</code> and etc. For each article, we gather the whole summary of the article provided by the Wikipedia API. Such a summary includes the uppermost chunk of text that one would usually see on a Wikipedia page.

### Featurization

In order to generate an encoded feature-like representation of each article, we create __vector representations__ using the __TF-IDF approach__. Naming each article as a document and the whole set of articles as the corpus, the __TF-IDF__ approach provides a metric that captures the __Term Frequency (TF)__ in the __documents__ as well as the __Inverse Document Frequency (IDF)__ in the __corpus__. The Term Frequency (TF) is the frequency of different words that appear in a document and the Inverse Document Frequency (IDF) is the inverse of document frequency among the whole corpus of all documents. In other words, TF measures how often a word appears within a specific document while IDF measures how rare a word appears within the corpus. TF-IDF in particular suppresses the dominance (overinflucence) of high-frequency words when it comes to determining their importance. Moreover, in simple words, a word is considered important in a document if, it occurs a lot in that document, but rarely in other documents within the corpus. For further reading about TF-IDF please refer to Refs. [[2](https://dl.acm.org/doi/abs/10.1145/1361684.1361686),[3](https://ieeexplore.ieee.org/abstract/document/7754750/)].

Accordingly, we create a set of feature vectors corresponding to each wikipedia article using the TF-IDF approach. TF-IDF vectorization package could be accessed from the scikit-learn Python package through this [link](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html). We convert all the characters to lowercase before tokenizing in TF-IDF, consider features at the word level but not character n-grams, and consider the top <code>num_tfidf_features=100</code> words ordered by term frequency across the corpus as the TF-IDF features.

### Similarity Measure

In order to identify the recommended articles within the gathered corpus of Wikipedia articles that are similar to an input article, we use the __Cosine Similarity__ as the similarity measure between the TF-IDF vectors that characterize the features attributed to each Wikipedia article. Comparing the Cosine Similarity between the TF-IDF vector of an __input article__ by the user and the TF-IDF vectors of all the articles in the corpus, we then recommend the top <code>num_recommended_articles=10</code> similar articles to the user.


