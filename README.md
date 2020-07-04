# SMS Spam Classifier: Binary Classification of SMS Spam Messages
SMS messages are ubiquitous and a major form of communication for billions of people. The spam messages that affect other communication channels, like email, similarly affect SMS. Filtering SMS messages and marking them as spam, and possibly removing them, can not only give the end user a better experience, but it can also free up network bandwidth, as hundreds of millions, if not billions, of messages could prevented from being sent every year.

This issue will not go away soon, with the upcoming adoption of the rich communication services protocol, end user devices will only become more advanced and have increasing ability to receive and respond to highly interactive and content full messages. This will be exploited both by corporate actors seeking profit and malicious actors.

This analysis aims to classify SMS spam messages using a variety of techniques, so that spam messages can be filtered. 

## Techniques Employed
A variety of techniques were employed in an attempt to find the data set's most important attributes, and in turn, attempt to use these specifying features to classify the messages. The following techniques were used: 

1. Word Cloud Analysis
2. Bag of Words - Frequency Vectorizer
2. Principal Component Analysis
3. Multinomial Naive Bayes
4. Logistic Regression
5. Support Vector Machine
6. Filtering for key words, punctuation

## Data Set
The data set we are working with is the *SMS Spam Collection Data Set*, which is available at the following link: ```https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection```
The data set has 5,572 unique records, and each record contains two different fields, the message field itself and a binary classification of either being spam or legitimate, where 'ham' denotes the latter.

#### The First 10 Rows of the Data Set:
| Classification | Text                                              |
|----------------|---------------------------------------------------|
| ham            | go until jurong point, crazy.. available only ... |
| ham            | ok lar... joking wif u oni...                     |
| spam           | free entry in 2 a wkly comp to win fa cup fina... |
| ham            | u dun say so early hor... u c already then say... |
| ham            | nah i don't think he goes to usf, he lives aro... |
| spam           | freemsg hey there darling it's been 3 week's n... |
| ham            | even my brother is not like to speak with me. ... |
| ham            | as per your request 'melle melle (oru minnamin... |
| spam           | winner!! as a valued network customer you have... |
| spam           | had your mobile 11 months or more? u r entitle... |

## Findings and Results
### Word Cloud
The first step we did was to generate a word cloud, this allowed us to simply and trivially see the most common words, as they will be the largest in the graphic.  
![Word Cloud](https://github.com/samsuri100/SMS_Spam_Classifier/blob/master/pictures/wordCloud.png)

From the Word Cloud above, the most common words included:
- now 
- will
- got 
- it
- love 
- ur
- ok  

### Principal Component Analysis
Without filtering any of the words and using all the words present in the initial data set, we did a Principal Component Analysis on the two most common attributes, which were essentially the two most common words, and got PC1 and PC2, though the variance of both only account for **0.304% and 0.293%**, respectively. For the two most common features, they have little influence over whether the message is spam or not.  
![All Words PCA](https://github.com/samsuri100/SMS_Spam_Classifier/blob/master/pictures/allWordsPCA.png)

Filtering the data set for specific words and symbols, we added additional fields for each record in the data set depending on whether each word or symbol was present in that record's text, and ignored the original fields that corresponded to the original set of words, unless they were one of the specific words that we added an additional field for. The specific words and symbols we looked for were:
- free
- win
- interested
- buy
- $
- offer
- !
- ?  

Our new data frame looked like:
|   | Free | Win | Interested | Buy | DollarSign | Offer | Exclamation | Question |
|---|------|-----|------------|-----|------------|-------|-------------|----------|
| 0 | 0    | 0   | 0          | 0   | 0          | 0     | 0           | 0        |
| 1 | 0    | 0   | 0          | 0   | 0          | 0     | 0           | 0        |
| 2 | 1    | 1   | 0          | 0   | 0          | 0     | 0           | 0        |
| 3 | 0    | 0   | 0          | 0   | 0          | 0     | 0           | 0        |
| 4 | 0    | 0   | 0          | 0   | 0          | 0     | 0           | 0        |

With these additional fields in place, and the original fields being ignored, we again did a Principal Component Analysis on the two most common attributes, and got PC1 and PC2, though the variance of both now accounted for **15.412% and 14.176%**, respectively. This is an improvement, and means that the fields we added were more significant than the words alone at determining whether a given SMS message was spam or not. This is not surprising because the most common words, like 'the' and 'hi' may be present in both spam and legitimate messages.  
![Filtered Words PCA](https://github.com/samsuri100/SMS_Spam_Classifier/blob/master/pictures/keyWordsPCA.png)

### Machine Learning Algorithms
PCA provided useful clues on what is important in the messages, not the most common words as much as specific words. However, PCA is only a tool to reduce dimensionality and provide some useful insights on the most common attributes in the process. Seeing how well certain machine learning classification algorithms fit the data also provides insight. 

Looking at three machine learning algorithms:
1. Multinomial Naive Bayes
2. Logistic Regression
3. Support Vector Machine

We saw how well each of the algorithms fit and predicted the data. The results are as follows:
1. Multinomial Naive Bayes --> **98.15**
2. Logistic Regression -- > **98.27**
3. Support Vector Machine --> **87.14**

Clearly, Multinomial Naive Bayes and Logistic Regression did the best, whereas Support Vector Machine did the worst. This is most likely because though the data is in a very high dimension, given that the number of columns is established over the entire vocabulary of the 'text' column, it is not easily separable, regardless of the kernel used. Also, because an optimum soft margin had to be found, Support Vector Machine was the slowest of the methods. Multinomial Naive Bayes did very well considering its speed and simplicity. It was particularly well suited for the bag-of-words technique on the text data.

Results of over 98% are quite good, however, since many words sent over SMS text messages are slang, not spelled correctly, or abbreviated, these altered words can dilute the amount of valid English words that we can attempt to derive meaning from. To investigate this, we filtered out all non-valid English words, but kept two attribute fields, looking at whether exclamation and questions marks were present. It is worth noting that some of the words present still are not valid English, such as 'dun', but they are present in the dictionary that we checked against. Our data frame, for our X values, before the text has been turned into a frequency vector, now look like this:

|   | Text                                              | ExclamationMark | QuestionMark |
|---|---------------------------------------------------|-----------------|--------------|
| 0 | go until point crazy.. available only in great... | 0               | 0            |
| 1 | ... joking ...                                    | 0               | 0            |
| 2 | free entry in a wkly comp to win fa cup final ... | 0               | 0            |
| 3 | dun say so early ... already then say ...         | 0               | 0            |
| 4 | nah i do think he goes to he lives around here... | 0               | 0            |
| 5 | hey there darling it been week now and no word... | 1               | 1            |
| 6 | even my brother is not like to speak with me t... | 0               | 0            |
| 7 | as per your request has been set as your for a... | 0               | 0            |
| 8 | winner as a valued network customer you have b... | 1               | 0            |
| 9 | had your mobile 11 months or more entitled to ... | 1               | 1            |

Re-running out tests, though excluding Support Vector Machine, we now get:
1. Multinomial Naive Bayes --> **97.67**
2. Logistic Regression -- > **98.03**

Our accuracy actually went down, for Multinomial Naive Bayes, by **.478%**, and for Logistic Regression, by **.24%**. That means we eliminated some key columns when filtering out non-valid English words. In fact, this makes sense, certain expressions and abbreviations, such as <3 or (:, might be very indicative of personal messages over more formal messages, whose purpose can be more ambiguous. 

Going back to the second PCA analysis that we performed, where we only looked at certain key words and symbols:
- free
- win
- interested
- buy
- $
- offer
- !
- ?  

Lets see the accuracy of our machine learning algorithms when only performed on these specific attributes. Instead of using Multinomial Naive Bayes, we will be using Bernoulli Naive Bayes, as that is better suited for binary features.
Our results are:
1. Bernoulli Naive Bayes --> **90.13**
2. Logistic Regression -- > **89.71**

It seems that though the key words and punctuation we included do account for more variability than many other words, they still do not account for enough variability to make provide higher accuracy than when training on the full data set, and this makes sense given the limited scope of the attributes.
