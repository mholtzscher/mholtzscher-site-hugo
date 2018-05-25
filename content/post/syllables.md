---
title: "Counting Syllables In English Words"
date: 2018-05-24T18:00:00-05:00
---

### The Beginning

When I started looking into how I could explore the Yelp dataset on [Kaggle](https://www.kaggle.com/yelp-dataset/yelp-dataset) I was really curious about the language complexity of the reviews. Did writing quality suffer when people wrote reviews? How complex did users make their sentences in their reviews? What kind of vocabulary did they use? Therefore, I knew my first course of action was to find ways to objectively calculate and evaluate these questions. 

### Text Readability

There are a lot of different ways to calculate the complexity of a corpus of text. So I started Googling and started my research with text readability scores. These scores and indexes have been around for decades with millions of dollars in rsearch having been invested by the U.S. government and various universities. The government, more specifically the millitary, wanted a system to score their manuals so that anyone serving could read the instructions for a piece of equipment. The Flesch-Kincaid reading grade level was created during a contract to the U.S. Navy in 1975. Similar formulas have been developed for other areas of industry such as: insurance policies and health care documentation.

### The Formulas

I started implementing these formulas on top of the amazing NLP platform [SpaCy](https://spacy.io/). I used SpaCy to perform the heavy lifting of splitting a sample of text into sentences and words. The Flesch-Kincaid formula uses words per sentence and syllables per word along with some calculated constants to determine the score and grade level. 
![svg](/syllables/flesch-kincaid.svg)
SpaCy makes the words per sentence calculation very simple and straight forward. However, calculating syllables is quite a challenging and complex problem. Many readability formulas use syllables in some fashion. (list some forumulas here)

### Syllables By Hand

