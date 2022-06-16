## 2. What Are Recommendations?

Recommendation algorithms may seem to inhabit only content platforms and
e-commerce websites, but their application is actually quite broad.
Whenever we have two types of things that need to be paired
together — moviegoers and movies, customers and customer support
representatives — we have a recommendation problem. The task is, given
historical information about how these pairings have gone in the past,
to predict new pairings for the future.

![Figure 2.1 Recommendation systems have many uses beyond recommending products.](figures/02-01.png)
*Figure 2.1 Recommendation systems have many uses beyond recommending products.* 

For example, we can easily imagine a system that predicts the best
customer sales representative to help a person based on the current
question as well as past interactions. Or we could choose to make a
system that predicts the outcome of the interaction between X and Y.
Because of the symmetry of the system, it could also be used to
recommend X given Y or Y given X.

The massive scope of applicability is why recommendation problems
constantly show up in different fields, and also why they are so hard to
solve (as we’ll discuss in [Complexity of the Problem](#complexity-of-the-problem)).
It is because of their complexity that in most places where
recommendation algorithms would make sense, heuristics are used instead.
For example, an online video game will usually group users together
based on experience. What if instead the grouping also took into
consideration play style and social dynamics? Both systems would be
recommending users to users for matching in a game; however, the former
ignores all but the simplest information. On the other hand, extracting
the relevant information to make the more informed decision is
incredibly difficult; we don’t have information regarding most of the
possible pairings, and we can’t know while building the system whether
user A and B will actually play well together.

![Figure 2.2 Collaborative filtering and matrix factorization make use of past interactions (shown in orange) to make recommendations.](figures/02-02.png) 
*Figure 2.2 Collaborative filtering and matrix factorization make use of past interactions (shown in orange) to make recommendations.* 

There have been many attempts to try to uncover some of this underlying
information in the data. Collaborative filtering and matrix
factorization are two methods that have generally been the frontrunners
for solving these problems by using previous interactions to try and
understand how objects interact.

Deep learning has also recently come into the recommendations game and
shifted things quite a lot. The main benefit from deep learning (as
we’ll see in [Neural Network Approaches](#neural-network-approaches)) is that we can use more than just
interaction data, and start learning from the actual raw data describing
the objects we are recommending. Because of this, the models can more
easily uncover the **semantic** information that contributes to why the
interactions go well or poorly. This is done using a new and promising
method called **multimodal embedding** (MME).

We discussed embeddings in our report on ["Summarization"](https://summarization.fastforwardlabs.com/), where we examined
using models such as **word2vec**^[See http://mubaris.com/2017/12/14/word2vec/] and **skip-thoughts** as a way to
turn language into a numerical representation that a computer can
understand. Unlike classic representations (for example, bag of words),
this representation distills an understanding of the text that can be
used to do complex and insightful calculations — these models are
powerful enough to solve analogies and summarize documents!


![Figure 2.3 Embeddings can use text or image data from an item to group them for recommendations.](figures/02-03.png)
*Figure 2.3 Embeddings can use text or image data from an item to group them for recommendations.*

More importantly, multi-modal systems learn fundamental characteristics
about the items or users and create a model that understands how these
characteristics (which can come from intrinsic data, such as an image or
description of an item) relate to each other. By having our
recommendation system extract semantic meaning from that raw data we can
form recommendations for items and users we’ve never seen before,
avoiding the so-called "cold start problem" that plagues other methods.

While this extension to the field is still in its infancy, more work is
being done constantly to expand the utility of deep learning in
recommendations. We’ll explore some of the shortcomings of the current
approaches in [Failures](#failures), but this field is ramping up and soon
will open up the possibility to solve most of the problems that **are**
recommendation problems **as** recommendation problems.

#### The Role of Representation

The term "representation" will come up a lot in this report, and it is
important enough that we should spend some time discussing what it
means. All algorithms operating on real-world objects need to come up
with some way of representing those objects in a quantitative way. One
way of representing categorical data, or data that can be divided into
groups, is with a **one-hot vector**. That is to say, you have a long
list of all the possible values a sample can be represented by, and for
each individual sample, you put a 1 for each value that’s present and a
0 for all the others. For example, a bookseller who wanted to encode the
topics of the books in their inventory could use a one-hot vector for
the topics in *Harry Potter and the Sorcerer’s Stone* that looked like the figure below. 

![Figure 2.4 An example 1hot vector for Harry Potter and the Sorcerer’s Stone.](figures/02-04.png)
*Figure 2.4 An example 1hot vector for Harry Potter and the Sorcerer’s Stone.* 

This can be done for all sorts of fields that may be present. So, we
could encode a book title by having a list of all the possible words
that could be in the title, and putting a 1 next to the words that exist
in the title of the book we’re encoding (this is the aforementioned "bag
of words" approach). Similarly, for interactions, we could have a list
of all our users, and for a given book we could put a 1 next to the
users who have interacted with that book and a 0 next to the users who
have not.^[If we accumulate this vector for all books we create an adjacency
matrix, which will be discussed in [Collaborative Filtering](#collaborative-filtering)]

The major problem with these methods, however, is that they don’t
represent any meaningful semantic information about the content that we
can use for comparison. For example, consider the bag of words
representations for *The Buffalo Book: The Full Saga of the American
Animal* and *High Hopes: The Rise and Decline of Buffalo, New York*.
Ignoring stopwords like "of" and "the," they are linked by the word
"Buffalo." On the other hand, those titles share no words in common with
that of a third book, *Of Bison and Man*. As humans, we can see the
relationships between the books and understand that *The Buffalo Book*
and *Of Bison and Man* are related, while *High Hopes* is about a
completely different topic — but using this representation, a computer
cannot.

![Figure 2.5 Simply checking for the presence of common words ignores the context those words are used in.](figures/02-05.png)
*Figure 2.5 Simply checking for the presence of common words ignores the context those words are used in.*

Throughout this report, we’ll talk about three main ways of picking
representations for recommendations that address this issue:
collaborative filtering, matrix factorization, and multimodal
embeddings. We will dive into the full technical details in
[History of Recommendation Systems](#history-of-recommendation-systems), but thinking about the various types of
representations used in the algorithms is a good way to distinguish and
understand them.

In collaborative filtering (see Figure 2.6), we ignore all the
problems with having rich representations of objects in our
recommendation system and simply focus on the user-item interactions,
hoping that the interactions themselves contain meaningful information.
That is to say, some people are just very into buffalo, and thus their
interactions will mainly be with books on that topic… a feature that
will propagate into our recommendation system (even if we don’t know
what a buffalo is!).

![Figure 2.6 Collaborative filtering finds recommendations using common likes between users.](figures/02-06.png)
*Figure 2.6 Collaborative filtering finds recommendations using common likes between users.* 

In matrix factorization, described in [Matrix Factorization](#matrix-factorization), we take the
interaction data or the metadata and try to distill it into a smaller,
more compact form (see Figure 2.7). So, the bag-of-words
representation for a book title, which could consider tens of thousands
of words, would be distilled down to 10 or so values (called
**factors**), which would encode higher-level features. This can be
thought of as a type of topic extraction, in the sense that sets of
features (like the presence of the word "buffalo" or "bison") contribute
to one factor, while the presence of other sets of features (like the
words "New York") will contribute to another factor. However, matrix
factorization still starts with one-hot representations before creating
a new one, and thus carries with it many of the associated problems.

![Figure 2.7 Matrix factorization abstracts factors out of the items and uses those factors to make recommendations.](figures/02-07.png)
*Figure 2.7 Matrix factorization abstracts factors out of the items and uses those factors to make recommendations.* 

Embeddings, on the other hand, never attempt to look at the bag-of-words
representation and instead look at the raw text (Figure 2.8). In doing so, they are able to
learn from word order and from word context. In the end, this gives us a
representation where titles about buffalo, regardless of the exact
terminology being used, will be deemed similar to each other. Moreover,
the structure that the embedding learns contains within it a general
understanding of how all the words relate to each other. For example,
with word2vec it is possible to add and subtract words such that, for
example, `vec("USA") - vec("Washington DC") + vec("France")` is close to `vec("Paris")`. This
sort of deep understanding about the content of the data being fed into
the system is unparalleled, and lets us use information that is hidden
in text — such as topic, tone, style, and content — in our algorithms.

![Figure 2.8 Embeddings use raw text to place items and users in an embedding space.](figures/02-08.png)
*Figure 2.8 Embeddings use raw text to place items and users in an embedding space.* 

For more information about the importance of representation and how they
are created in the context of language models, please read section 4.2,
["Language Models with RNNs" from *FF04: Summarization*](https://summarization.fastforwardlabs.com/#language-models-with-rnns).


### Why Recommendations Are Hard

#### Complexity of the Problem

Like A/B testing and reinforcement learning, recommendations are part of
a class of problems called Markov decision processes (MDPs). MDPs are
problems where there is a finite state describing the world and a finite
number of actions that can be taken, and each action has an unknown
reward associated with it. Returning to the initial e-commerce example,
the state would be a user’s history on the site and the interaction
history for all the items in the online catalogue. The action would be a
choice of which item to show to the user, and the reward would be
whether the user purchases the item. Importantly, the effects of the
action and the final user interaction go on to affect the state of the
system and change future actions. This way of thinking about
recommendations is a useful way to account for both the generality of
these systems (beyond just e-commerce) and complications in building a
robust system.

![Figure 2.9 A recommendation system modeled as a Markov decision process.](figures/02-09.png)
*Figure 2.9 A recommendation system modeled as a Markov decision process.* 

As a point of comparison, let’s look at a solution to an MDP that has
gained popularity — playing Atari with reinforcement learning^[See https://arxiv.org/abs/1312.5602]. A good
way to do this is by looking at the number of parameters involved in
defining the problem; it can serve as a proxy for how complex our
solution must be. An Atari screen has a resolution of 160x192 pixels and
a maximum of 128 colors. This corresponds to a state that can be
represented by 3,932,160 numbers. The action state can be encoded by
three numbers, two for the left/right and up/down position of the
joystick and one for the button. Finally, the reward is simply how much
a user’s score has gone up.

In contrast, the Netflix recommendations dataset^[See https://www.kaggle.com/netflix-inc/netflix-prize-data] contains 17,770
movies, rated from 1 to 5 by 480,189 users. This creates a state
representable by 42,664,792,650 numbers (and this is if we ignore the
order in which a user watches movies!). The number of actions we can
take is equal to the number of items we have multiplied by the number of
recommendations we want to show the user. Even if we only recommend one
movie, that’s an action state representable by 17,770 numbers!
Furthermore, the reward can become quite tricky to calculate. We can
look at whether a user watches the movie, but we might also consider
what rating that user gives it, whether they watch the whole thing, or
whether that movie recommendation influences their decision to watch
another movie (for example, if we recommended *2 Fast 2 Furious* but the
user decides to watch the original *Fast and Furious* first).

In order to deal with this complexity, various algorithms have been
created that take advantage of some structure we can find in the data.
This structure can be used to reduce the number of parameters needed to
model the problem and make a model tractable. As an example, classical
collaborative filtering algorithms (described in detail in
[Collaborative Filtering](#collaborative-filtering)) rely on the assumption that people can be considered
similar if they interact with similar items, and that people want to see
items that similar people like. What this does is reduce the number of
possible recommended items to those our friends have interacted with, or
those that people who’ve bought similar things to items we’ve purchased
have interacted with. This may seem like a fairly benign assumption to
make, but what happens, for example, with the news media if most people
see the same 90% of articles from the major headlines, and a user’s
personal taste is only evident from the last 10% of articles they look
at? We discuss some of the possible social effects of this in
[Filter Bubbles and Echo Chambers](#filter-bubbles-and-echo-chambers).

Multimodal embeddings try to approach this problem by avoiding it
entirely. Instead of operating on every item and every user separately,
we only consider the properties that make up the objects. With books,
for example, this means that we only need to consider the words that
make up a book’s summary. This may seem like making the problem much
harder, but it’s a method that also comes with a lot more data, since
each item has a wealth of data associated with it. Furthermore, since we
are training our system simply to find good representations of the
books, as opposed to directly creating recommendations, we simplify the
action phase of the algorithm as well. We now only have to put books in
the neighborhood of books other people have liked, and far away from
books other people haven’t liked.

#### New Data

Another difficulty is how to deal with new items. For example, if we
have a new article that no one has interacted with, how do we know what
types of users might enjoy it if we are relying on the interactions of
"similar users" for our recommendations? Recommendation systems such as
these on the internet generally also come with explanations in the form
of: "You read article A, and so did Alice. Alice also read article B, so
you should read it, too." In the absence of a user interacting with the
article, however, we don’t know how it fits in with user preferences.
This is the **cold start problem** mentioned earlier.

![Figure 2.10](figures/02-10.png)
*Figure 2.10 Collaborative filtering is unable to make recommendations for a new item because it does not have interaction data for that item.*

Many methods attempt to solve it by using metadata about the objects.
So, if you generally read political articles, we will recommend to you
our new political article, regardless of who has interacted with it.
This, however, can lead to very bad recommendations, because we have to
make strong assumptions about what properties to extract out of an
article so that our system will have a general understanding of it. What
if you don’t actually care about politics in general, and just want to
read about a certain event or figure? In that case, our system would
have to have a special tag that takes that into account. Alternatively,
you may simply like long-form political editorials and nothing else; how
can a metadata algorithm know this, unless we have already introduced
"long-form politics" as a tag in our system?

Multimodal embeddings are able to deal with this by looking directly at
the content that is being recommended and learning their own way of
understanding the relevant features. This stems from the trend in
machine learning of using deep learning to automatically find what
aspects of the data are interesting.^[This is called feature engineering, and it is generally something that must be done with some domain expertise. Deep learning, however, has shown that it is able to do automatic feature engineering, often finding better features than experts would!] 
The thought is: why should I
tell the system that I think "long-form politics" will be important,
when it can learn that by itself? In fact, this is a much more robust
way of doing things, since the machine is able to find a much wider
variety of ways of representing the items than a human could.

MMEs rely only on the content of the items being recommended and the
items that users have interacted with in the past. This helps us
completely avoid the cold start problem, because we don’t need to know
who is interacting with a new piece of media or what tags are associated
with it; we simply need to know its content. The algorithm is able to
use that information to infer how the content aligns with a person’s
interests. We can recommend a new article to you because the
topic/tone/style aligns with *your* preferences, not because of other
users' interactions.

#### Missing Data and Evaluation

Finally, we come to the biggest difficulty with recommendations:
evaluation. When first creating a recommendation system, you have to
decide what you are actually trying to do. Are you trying to predict
whether a user will interact with a new item, or predict the user’s
preference order for various items? How do you know when you’ve failed?
How do you know when you’ve succeeded? How do you deal with the fact
that your model is fundamentally altering the state of the world it is
trying to model?

Generally, when a recommendation system is being created, data is
truncated in time, and we try to predict the newest data using the
oldest data. This is to simulate the fact that when training a model, we
cannot ask the user whether they would like a recommendation or not.
This changes the problem into one of predicting whether a user did, in
the newest data, interact with a recommended item (essentially removing
any notion of feedback). It is important to realize, though, that this
is fundamentally a different problem, and comes with its own additional
problems.

For example, if I predicted that you would like *Introduction to
Algorithms* but you didn’t interact with it in the latest data, then was
I wrong? Should I penalize my algorithm in favor of it predicting that
you wouldn’t like the book? If you as a user have only given me a very
small sample of data regarding your preferences, it is impossible to
tell. This is why current recommendation methods are very quick to put
users into very small groups of recommendations; they assume that
anything you haven’t interacted with is something you don’t like,
instead of simply being something you haven’t had the chance to interact
with yet. This is particularly problematic because most users only
interact with a small percentage of a vast number of possible items.
(How many movies from the Netflix catalogue have you looked at? How many
items from Amazon’s full listing have you bought?)

As a result, the only motivated way to train a recommendation system is
online, using A/B testing, multi-armed bandits,^[Multi-armed bandits is a generalization of A/B testing where multiple different results are possible. It uses a much more statistically motivated way of deciding when to pick one result over another when compared to classic A/B testing.] 
or similar algorithms from the reinforcement learning community. 
These algorithms explore how
you may react to different items, and they operate on user feedback, as
opposed to historical data. This removes the assumption that not
interacting is the same as not liking, as well as intrinsically
incorporating that feedback.

However, doing this is hard — and it is hard for reasons that no
algorithm can fix. We must show results to users and get their feedback.
This requires having a large enough and active enough user base to be
able to test and refine the system. Furthermore, it requires a good
enough data pipeline to take this interaction data in and refine the
model on the fly, as more interactions occur. Consequently, it generally
takes much longer to train a model that has satisfactory results.

### What Are the Solutions?

We can see that recommendations is a very complex problem. There are
many decisions to be made about algorithm, representation, and data
quality. In addition, it may not always be obvious what is being
optimized for or how to best capture this as a machine learning task.
Because of these subtleties, it is important to have a good
understanding of the variety of algorithms that support these
recommendation systems. By understanding the algorithms, choices can be
made to mitigate many of the complications introduced here.

