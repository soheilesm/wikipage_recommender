# wikipage_recommender
This repo presents a content-based recommendation framework for recommending users wikipedia pages

## Text Recommendation Framework

### Recommendation Approach
Here I build a recommender system based on the very basic principles of Content-Based Recommendation apporach [[1](https://link.springer.com/chapter/10.1007/978-3-540-72079-9_10)]. In summary, a content-based recommender system utilizes the features of product(s) in order to recommend other product(s) similar to what a user has liked, or purchased, or used.

### Dataset Gathering and Cleaning
Downloading all Wikipedia articles from the webstie as you have suggested comes to 78 GB of data when unzipped. So, as the source data I gather a series of articles from Wikipedia. In order to gather such articles I use __wikipedia API__ for Python that can be accessed through [this link](https://link.springer.com/chapter/10.1007/978-3-540-72079-9_10). This API could be installed using the 'pip', as the standard package-management system for Python, using the command <code>pip install wikipedia</code>. In this work I gather a random set of __ONLY__ <code>num_random_articles=25000</code> wikipedia articles. Alghout, if I had more time and resources, I could have gathered, more and more articles to further saturate the corpus pool with multiple articles.

As a very simple cleaning I get read of non-contextual components in the text, e.g. '\n' and etc. For each article I gather the whole summary of the article provided by the wikipedia API. Such a summary usually includes the top most chunk of text that you observe generally when you open a wikipedia page.

### Featurization

In order to generate an encoded feature-like representation of each article, I create __vector__ representations using the __TF-IDF approach__. Naming each article as 'document' and the whole set of articles as 'corpus', the TF-IDF approach provides a metric which captures the Term Frequency (TF) in the documents as well as the Inverse Document Frequency (IDF) in the corpus. The Term Frequency (TF) is the frequency of different words that appear in a document and the Inverse Document Frequency (IDF) is the inverse of document frequecy among the whole corpus of all documents. In other words, TF measures how often a word appears within a specific document while IDF measures how rare a word appears within the corpus. TFIDF in particular suppresses the dominance (overinflucence) of high freqeuncy words when it comes to determining their importance. Moreover, in simple words, a word is considered important in a document if, it occurs a lot in that document, but rarely in other documents within the corpus. For further reading about TF-IDF please refer to Refs. [[2](https://dl.acm.org/doi/abs/10.1145/1361684.1361686),[3](https://ieeexplore.ieee.org/abstract/document/7754750/)].

Accordingly, I create a set of feature vectors corresponding to each wikipedia article using the TF-IDF approach. TF-IDF vectorization package could be accessed from the scikit-learn Python package through [this link](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html). I convert all the characters to lowercase before tokenizing in TF-IDF, consider features at the word level not character n-grams, and consider the top <code>num_tfidf_features=100</code> words ordered by term frequency across the corpus as the TF-IDF features.

### Measure of Similarity

In order to identify the recommended articles within the gathered corpus of wikipedia articles that are similar to an input article I use the 'Cosine Similarity' as the similarity measure between the TF-IDF vectors that characterize the features attributed to each wikipedia article. Comparing the Cosine Similary between the TF-IDF vector of the input article and the TF-IDF vectors all the articles in the corpus, I then recommend the top <code>num_recommended_articles=10</code> articles to user.
