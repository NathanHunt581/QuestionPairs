# QuestionPairs

## Overview

Question and answer websites (Quora, StackOverflow, Yahoo! Answers, to name a few) are faced with the same question, with slightly different wording, being asked multiple times.  If the site can automatically determine that a question has already been asked (and possibly answered), the user experience can be improved by directing the user to the existing answers.  If a question is determined to be novel, a new question - answer thread can be initiated.

In this project, I look at a dataset of 400000 question pairs provided by Quora.  These pairs of questions are labelled as duplicate or not duplicate.  I apply Natural Language Processing (NLP) approaches implemented with a neural network to predict whether pairs are duplicates or not.

## Data

I am not including the data on GitHub.  It is available at

https://www.kaggle.com/c/quora-question-pairs/data

Note that the notebook needs Python 3.6 - trying 3.7 gives an error when using TensorFlow.

## Methods

I used SpaCy and Gensim for preprocessing, lemmatization and named entity recognition.  The Glove 300-dimensional embeddings were used initially, but then trained further.  A Siamese neural network architecture was used, feeding each question into a separate, identical copy consisting of an embedding layer followed by a Gated Recurrent Unit (GRU) layer.  The outputs of the two GRU's (one for each question) were compared by taking the L1 norm of the differences.  This difference signal was fed into a final sigmoidal layer to make the duplicate / not duplicate call.

I used regularization, dropout, and early stopping to avoid over-fitting.

## Conclusions

The accuracy achieved,  77.5%, is 15% better than one would get by just guessing "not duplicate" for all the questions.  I was pleased to get the Siamese network architecture working; the over-all accuracy is several percent higher than one can get with simple approaches just using SpaCy's similarity method.

To improve results further, I would like to try using ELMO, which provides a context-dependent representation as opposed to what the simple embeddings provide.  Another more sophisticated approach would be to use an attention-based method such as BERT, although BERT seems to require significant computational resources.

A note for anyone wanting to work on this data set - it is from a Kaggle contest.  There are a lot of complaints from the contest participants because it turns out that one can boost one's score significantly by looking at non-NLP factors such as the number of times a question appears in the data set. 
