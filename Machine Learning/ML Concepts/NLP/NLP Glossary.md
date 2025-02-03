## Word Embedding Model
A model that take a word and converts it to a dense continuous vector

## Static Word Embedding Model
A model that maps a word in its vocabulary directly to a vector, regardless of context.

This has the upside of simplicity, and speed. However, it struggles with semantics and cannot handle words outside its vocabulary.
###### **Word2Vec** is and example of this type of model.
## Contextual Word Embedding Model
A model that considers a sequence of words as a whole to determine a words contextualized embedding. 
###### **BERT** is an example of this type of model.
## Sub-word Embedding Model
A model that breaks words into sub-word vectors. They are largely designed to handle words OOV (out of vocabulary). 
###### **FastText** is an example of this type of model.