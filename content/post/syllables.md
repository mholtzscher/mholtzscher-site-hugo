---
title: "Counting Syllables In English Words"
date: 2018-05-29T6:00:00-06:00
---

### The Beginning

When I started looking into how I could explore the Yelp data set on [Kaggle](https://www.kaggle.com/yelp-dataset/yelp-dataset) I was really curious about the language complexity of the reviews. Did writing quality suffer when people wrote negative reviews? How complex did users make their sentences in their reviews? What kind of vocabulary did they use? Therefore, I knew my first course of action was to find ways to objectively calculate and evaluate these questions. 

### Text Readability

There are a lot of different ways to calculate the complexity of a corpus of text. So I started Googling and started my research with text readability scores. These scores and indexes have been around for decades with millions of dollars in research having been invested into them by the U.S. government and universities. The Flesch-Kincaid reading grade level was created during a contract to the U.S. Navy in 1975. The military wanted a system to score their manuals so that anyone serving could read the instructions for a piece of equipment.  Similar formulas have been developed to evaluate text for other areas of industry such as: insurance policies and health care documentation.

### The Formulas

I started implementing these formulas on top of the amazing NLP platform [SpaCy](https://spacy.io/). I used SpaCy to perform the heavy lifting of splitting a sample of text into sentences and words. The Flesch-Kincaid formula, for example, uses words per sentence and syllables per word along with some calculated constants to determine the score and grade level. 
![svg](/syllables/flesch-kincaid.svg)
SpaCy makes the words per sentence calculation very simple and straight forward. However, calculating syllables is quite a challenging and complex problem. Many readability formulas use syllables in some portion of their equation. Flesch-Kincaid grade level/reading ease and SMOG index are just a few that rely on syllable counts in some manner.

### Syllables By Hand

As a human, counting syllables is a simple but time consuming task that can be performed using a few different methods as seen below. [Source](https://www.howmanysyllables.com/howtocountsyllables)

1. The "Listen Method" Rules
    - Say the word.
    - How many times do you hear A, E, I, O, or U as a separate sound?
    - This is the number of syllables.
1. The "Chin Method" Rules
    - Put your hand under your chin.
    - Say the word.
    - How many times does your chin touch your hand?
    - This is the number of syllables.
1. The "Clap Method" Rules
    - Clapping may help you find syllables.
    - Say the word.
    - Clap each time you hear A, E, I, O, or U as a separate sound.
    - The number of claps is the number of syllables.
1. The "Robot Speak Method" Rules
    - Make believe you are a robot from the year 2000.
    - Say a word as this robot.
    - Pay attention to the pauses you make.
    - How many parts did you break your word into?
    - This is the number of syllables.
1. The "Written Method" Rules
    - Count the number of vowels (A, E, I, O, U) in the word.
    - Add 1 every time the letter 'y' makes the sound of a vowel (A, E, I, O, U).
    - Subtract 1 for each silent vowel (like the silent 'e' at the end of a word).
    - Subtract 1 for each diphthong or triphthong in the word.
    - Does the word end with "le" or "les?" Add 1 only if the letter before the "le" is a consonant.
    - The number you get is the number of syllables in your word.

These methods are very effective if you're a human but incredibly complex to implement programmatically since they rely on detecting sounds while speaking a word.

### Maybe Someone Has Already Done This?

I started my search by looking for a list of words and their syllable counts hoping someone had compiled a database for me. The closest thing I found was the [CMU Pronouncing Dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict). However, this database shows the phonemes for the words rather than syllables. In some cases the phonemes align with syllables but this is not always the case. I then moved on to looking for an algorithm to programmatically count syllables and had mixed luck. I found a few very primitive implementations on StackOverflow but most were missing handling for many of English's edge cases.  

### Syllapy

So I started piecing all these different algorithms together and this is where my package [SyllaPy](https://pypi.org/project/syllapy/) began. My current implementation can correctly handle a large subset of English words with its rules but there still quite a few cases where they fail. 

<script src="https://gist.github.com/mholtzscher/f753f9b893856eb15b26387f7b4a27cb.js"></script>

The English language is an amalgamation of words from other languages, so this means for every 10 words that fit my "rules" there will be at least one word that doesn't fit. While my current algorithm is fairly accurate, the perfectionist in me always wants MORE! To increase accuracy I have started compiling a database of known words and syllable counts. SyllaPy will lookup a word in the database before processing to see if I can get the correct answer immediately. The database has been built by scraping the web and gathering mostly small lists of syllable counts from education websites. Many universities/schools have small lists of words (less than 100) that are used in curriculum for elementary and middle school students so I have to hand check them before I merge them into the primary database. I will continue to use my algorithm as a backup to handle words that are not currently in the database in the meantime.

The source code for Syllapy can be found [here.](https://github.com/mholtzscher/syllapy)