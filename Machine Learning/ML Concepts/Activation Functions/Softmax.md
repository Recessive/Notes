# Softmax
Softmax is an activation function used to convert numbers into a probability. It's equation is as follows:

$$\huge\sigma(x_i)=\frac{e^{x_i}}{\sum^n_{j=1}e^{x_j}}$$
where $x$ is simply the entire input. To get the softmax of an entire vector, you would run $\sigma(x_i)$ for all $i$. Example below:
![[Softmax 2022-04-13 19.45.55.excalidraw]]

Python code for softmax:
`from math import exp`
`s = lambda x, xi: exp(x[xi])/sum([exp(j) for j in x])`



#machine-learning
#activation-function
#non-linear