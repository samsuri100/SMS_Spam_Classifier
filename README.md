# SMS Spam Classifier: Binary Classification of SMS Spam Messages
SMS messages are ubiquitous and a major form of communication for billions of people. The spam messages that affect other communication channels, like email, similarly affect SMS. Filtering SMS messages and marking them as spam and possibly removing them can not only give the end user a better experience, but it can also free up network bandwitdth, as hudreds of millions, if not billions, of messages could prevented from being sent ever year.

This issue will not go away soon, with the upcoming adoption of the rich communication services protocol, end user devices will only become more advanced and have increasing ability to recieve and respond to highly interactive and content full messages. This will be exploited both by corporate actors seeking profit and malicious actors.

This analysis aims to classify SMS spam messages using a variety of techniques, so that spam messages can be filtered. 

## Techniques Empoyed
A variety of techniques were employed in an attempt to find the data set's most important attributes, and in turn, attempt to use these specifying features to classify the messages. The following techniques were used: 

1. Word Cloud Analysis
2. Frequency Vectorizer
2. Principal Component Analysis
3. Multinomial Naive Bayes
4. Logistic Regression
5. Support Vector Machine
6. Filtering

## Data Set
The data set we are working with is the *SMS Spam Collection Data Set*, which is available at the following link: ```https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection```
The data set has 5,572 unique records, and each record contains two different fields, the message field itself and a binary classification of either being spam or legitimate

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
#### Word Cloud
The first step we did was to generate a word cloud, this allowed us to simply and trivialy see the most common words.
[WORD CLOUD IMAGE]
From the word cloud above, the most common words included:
- now 
- will
- got 
- it
- love 
- ur
- ok  

#### Principal Component Analysis
Without filtering any of the words and using all the words present in the initial data set, we did a Principal Component Analysis on the two most common attributes, which were essentially the two most common words, and got PC1 and PC2, though the varience of both only account for 0.304% and 0.293%, respectively. For the two most common features, they have little influence over whether the message is spam or not.
[ALL WORDS PCA]

Filtering the data set for certain, specific words, we added additional fields for each record in the data set depending on whether each word was present in that record's text. The specific words and symbols we looked for were:
- free
- win
- interested
- buy
- $
- offer
- !
- ?  

With these additional fields in place, we again did a Principal Component Analysis on the two most common attributes, though this was now between all the words present and the additional fields added, and got PC1 and PC2, though the varience of both now accounted for 15.412% and 14.176%, respectively. This is an improvement, and means that the fields we added were more significant than the words alone at determining whether a given SMS message was span or not. This is not surprising because the most common words, like 'the' and 'hi' may be present in both spam and legitimate messages
[FILTERED WORDS PCA]

#### Machine Learning Algorithms
PCA provided useful clues on what is important in the messages, not the most common words as much as specific words. However, PCA is only a tool to reduce dimensionality and provide some useful insights on the most common attributes in the process. Seeing how well certain machine learning classifications algorithms fit the data also provide insight. 

Looking at three machine learning algorithms:
1. Multinomial Naive Bayes
2. Logistic Regression
3. Support Vector Machine

We saw how well each of the algorithms fit and predicted the data. The results are as follows:
1. Multinomial Naive Bayes --> 98.15
2. Logistic Regression -- > 98.27
3. Support Vector Machine --> 
