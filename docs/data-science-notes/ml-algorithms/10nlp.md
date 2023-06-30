---
layout: default
title: NLP
parent: Machine learning algorithms
grand_parent: Data science notes
permalink: /docs/data-science-notes/ml-algorithms/10nlp/
nav_order: 10
---

# NLP

## Vocabulary

* **Corpus** = list of documents, i.e. list of strings
* **Documents** = strings
* **Tokens** = A token is a string of contiguous characters between two spaces or between a space and punctuation marks.
* 
* **TF-IDF** = short for **term frequency–inverse document frequency**, is a numerical statistic that is intended to reflect how important a word is to a document in a corpus. In practice, matrix with columns as count of words for a documents and rows as words.
* **Context words** : words within a defined window around a center word (e.g. the two words before and after a word)
* **Word embedding**: a mapping of a discrete — categorical — variable (representing a word) to a vector of continuous numbers. Neural network embeddings are useful because they can *reduce the dimensionality* of categorical variables and *meaningfully represent* categories in the transformed space. Better for NLP than one hot encoding as similar words or words from similar contexts will remain similar once embedded. With one hot encoding, all vectors are orthogonal to each other.

## Dimensionality reduction

* t-SNE is more appropriate to text as reduced to 2 or max 3 dimensions
