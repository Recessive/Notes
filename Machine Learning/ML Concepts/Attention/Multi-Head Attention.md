# Multi-Head Attention

Multi-Head Attention is simply splitting the **value** inputs into multiple streams then feeding each stream through it's own [[Attention]], then rejoining the streams at the end. This allows the network to learn different "levels" of meaning in the data. Example below:

![[Multi-Head Attention 2022-04-13 20.09.37.excalidraw]]
If you consider that the first step in [[Attention]] is to multiply the **key, query** and **value** by their respective [[ML Glossary#Parameter|parameters]], what is happening here is the input vector is being multiplied by $w_1$ and $w_2$, then multiplied by $W_{V1}$ and $W_{V2}$ , which means that you can just combine these [[ML Glossary#Parameter|parameters]] into one vector! Removing the entire $w_1, w_2$... stream and simplifying down to:

![[Multi-Head Attention Simplified 2022-04-13 20.09.37.excalidraw]]
Consider the example [here](https://youtu.be/KmAISyVvE1Y?t=1134) for an example of using multi-headed attention to give a network differing levels of view.


#machine-learning
#concept 
#attention