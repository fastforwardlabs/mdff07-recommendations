## 1. Introduction

The internet has given us an avalanche of options for what to read,
watch and buy. Because of this, recommendation algorithms, which find
items that will interest a particular person, are more important than
ever.

But until recently, recommendation algorithms suffered from a critical
shortcoming: they didn’t understand the content of the items they are
recommending or the underlying preferences of their users. Using them
was like getting a book recommendation from someone who hadn’t read the
book and didn’t know you well. This limitation was down to naive
algorithms that made unsubstantiated assumptions with insufficient
datasets, and a general lack of tools and hardware to unlock meaning
within raw content. However, thanks to recent algorithmic advances in
the field of embeddings, namely *multi-modal models*, we have begun to
uncover how the semantic content of items relates to a user’s
preference.

![Figure 1.1 We can now build recommendation systems that are content-aware, addressing a weakness of traditional approaches.](figures/01-01.png)


This new capability will allow us to do several things. First, it will
improve current recommendation systems by solving the *cold start*
problem — where existing algorithms simply cannot create recommendations
for items or users they haven’t seen before. It will also improve
recommendations by allowing the algorithms to use the most important
information about each item: the item itself. Algorithms that understand
content — and the preferences of a user in relation to that
content — can make better recommendations. Using these recommendation
algorithms is like getting a book recommendation from friend who knows
you well and has actually read the book!

Another exciting aspect of these new algorithms is the ability to apply
recommendation algorithms in contexts other than e-commerce. Better
recommendations predict the outcome of an interaction, so why restrict
their use to e-commerce? Multi-modal models could become important in
many situations: pairing users to customer service representatives,
recommending which emails you should respond to first (and why), or even
recommending travel routes that are not simply the most efficient, but
rather the route you would most prefer.

In this report we discuss this new field, from the history of how it has
evolved to where it currently is and what work is being done to make the
algorithms more widely applicable. While these methods are still in
their infancy, they show incredible promise.
