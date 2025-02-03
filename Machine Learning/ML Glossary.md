## Epoch
A complete run of every single training example in the training set through the model.

## Iteration
A complete run of every training example in the current batch through the model.

## Parameter
is:
1. A trainable variable within a network
2. A value to be passed to a function

## Hyper-parameter
Is a parameter who's value is used to tune the learning process. A really good example is the learning rate. Basically any custom value you set at the start of training is a hyper-parameter

## Encoding
A simplistic and human-readable representation of an input. Most common is **One-Hot Encoding**

## Embedding
A high dimensional **dense** continuous vector representation of some input

## One-Hot Encoding
A binary array consisting entirely of 0s and 1s, with an instance of a label being 1 and all others being 0. This encoding is defined as having only a single instance of a label per datapoint, ie, only one 1 per row. This makes it a **sparse** representation

The instance where multiple labels exist per datapoint is kind of considered **multi-label one-hot encoding**, but realistically people can't decide on a name.

#machine-learning 
#glossary