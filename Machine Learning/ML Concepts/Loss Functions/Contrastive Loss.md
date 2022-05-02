# Contrastive Loss

**Contrastive Loss** is any loss function that takes two inputs and compares them. There are a few different loss functions that fit this criteria. These are outline below:

## Simple form (superseded by [[Triplet Loss]])

Measures the euclidean distance between a pair of training samples. Defined as:
$$\huge L(a, b, Y) = (1-Y)\times||a-b||^2+Y\times\text{max}(0,m-||a-b||^2)$$
Where $a, b$ are both training samples, $Y$ is the label and $m$ is a [[ML Glossary#Hyper-parameter|hyper-parameter]] defining the upper-bound distance between dissimilar samples 

$Y=0$ if similar, $Y=1$ if dissimilar

|| means the norm of a vector

Can also be written as:
$$
\huge \begin{equation}
    L(a,b,Y) = \begin{cases}
      ||a-b||^2 & Y=0\\ 
      \text{max}(0,m-||a-b||^2) & Y=1
    \end{cases}\
\end{equation}

$$
### Detail
Analysing the function, essentially:
- $Y=0$: When the samples are meant to be similar, their loss is their euclidean distance
- $Y=1$: When they're meant to be dissimilar, their loss is 0 if they exceed the threshold $m$, otherwise it's their distance from $m$.

#### Why need an upper-bound?
$m$ is included so the model can actually converge. Think about what happens if it's not there - the network will continuously push everything further apart, which causes samples that are similar to have to learn more to catch up to the pushed away sample, and they keep tugging on each other unto infinity.

## Other forms:
- [[Normalized Temperature-scaled Cross Entropy Loss]]



#loss-function