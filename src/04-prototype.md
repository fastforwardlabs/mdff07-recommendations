## 4. Prototype

### Data

For our prototype, we decided to use the Amazon book recommendations
dataset.^[See http://jmcauley.ucsd.edu/data/amazon/] This dataset contains 41.13 million reviews, where every
book/user has at least 5 reviews associated with it. After heavy
filtering, we reduced the set to 254,932 books reviewed by 603,668
users. This filtering was necessary because of the abysmal data quality:
most users only give 5-star ratings and the number of ratings users
provide follows a power law (meaning there are half as many 4-star
ratings as 5 stars, half as many 3-star ratings as 4 stars, and so on).

![Figure 4.1](figures/04-01.png)
*Figure 4.1 The values for the ratings are heavily skewed towards 5 stars.*

![Figure 4.2](figures/04-02.png)
*Figure 4.2 A small group of users writes the majority of the reviews.* 

Another pain point with the dataset is the amount of noise in the book
summaries. Many of the books have incredibly short and noninformative
summaries. Another large portion of the books have very long summaries
that consist mainly of quotes from critics or author biographies. In
addition, a majority of the books' summaries have encoding errors which
result in meaningless characters being spread throughout the text. Our
favorite example of these errors is one book summary which is quite
descriptive, but doesn’t contain any spaces or punctuation!

![Figure 4.3](figures/04-03.png)
*Figure 4.3 Many books have short and uninformative summaries.*

As a result, our model is an example of working in an extreme situation
where there is no quality control over the data. In most applications,
application designers can remedy this lack of quality because of their
control over the environment. In this case, however, that was not
possible.

### Model

One of the hardships in creating a model to deal with user interactions
through book summaries is the number of sequences we need to deal with.
First, there is the sequence of words within the summary of one book. We
wanted to model this as a sequence since the summaries have variable
length (we initially hoped to avoid truncating them, but in the end this
was unavoidable, as we discuss in [Failures](#failures)).
Furthermore, we wanted to take advantage of the word *order* in the
summaries. In addition to this sequence, there is also the user history,
which is represented as a sequence of books that a user has interacted
with; this again is a variable-length sequence where order could
potentially be relevant. Since we represent each book with a sequence of
words, this means that the user history is a sequence of sequences.

Luckily, we can use recurrent neural networks to
learn from this sequential data without losing potentially valuable
temporal information such as word or book order. In addition, we’re able
to use the modular nature of neural networks to reuse and share
information between parts of the model. That is to say, the segment of
the model that learns how to understand a particular book summary can
also be reused to understand the books within a user’s history.

The first example of this modular structure is with the **attention
mechanism** we use to focus the model. Whenever we input a book
description, represented as a sequence of word2vec vectors, we first
filter that data through a set of layers that reweights the input so
that the model can learn to ignore certain concepts or focus on others.
This attention model is not a global feature — which is to say, it
doesn’t learn to *always* downweight a particular word. Instead, it
learns to weight words based on their context.

![Figure 4.4](figures/04-04.png)
*Figure 4.4 The structure of the attention mechanism in the model.*

This attention mechanism has two benefits. First, it focuses the neural
network so that it can concentrate on the data that is important for the
particular input it is considering, effectively reducing noise. Second,
it helps the users understand *why* a certain prediction is made. By
introspecting inside the model when being shown a particular example, we
can see which words it chooses to look at and which it ignores, giving
us information that can be shown to the user. These sorts of attention
mechanisms are used in many places where understanding why a neural
network makes a decision is important, and this is seen as the leading
method in making neural networks interpretable.^[See our report on[Interpretability](https://ff06-2020.fastforwardlabs.com/)]

Having now a sense of which parts of the book description were
important, we wanted to reduce this large sequence into something more
manageable. For this, we created another submodule for our larger model
that takes this reweighted sequence, feeds it through several layers of
recurrent networks, and outputs a fixed-length vector or embedding
(which was chosen to be 256 elements in length after some
experimentation).

![Figure 4.5](figures/04-05.png)
*Figure 4.5 The description is transformed into a manageable fixed-length vector.*

At this stage, we had a way of turning a book summary of arbitrary
length into a fixed-sized vector that encodes its content. This would
have been enough for a unimodal model that simply looked at how books
are related to other books, but we wanted to incorporate user histories,
or sequences of books with ratings associated with them. Our solution
was to take the embedding for a user (which consists of multiple book
embeddings, one for each book they’ve reviewed), multiply it with the
ratings that user has given the books they’ve read, and feed it into
another recurrent network that is meant to find a single vector
representing the user. This resulting vector is the same size as the
vector for a single book.

![Figure 4.6](figures/04-06.png)
*Figure 4.6 The full multi-modal model.* 

We finally have all the working pieces we need. We have a 256 element
vector embedding for a book, and we also can create an embedding of a
user’s history into a vector embedding with the same size. All told,
this model takes 1,610,652 parameters, which is fairly modest given the
complexity of the operations being performed. The model is now trained
so that the dot product between these two embeddings is as close to that
user’s rating for the book as possible.

This procedure not only gives us a way to train the model using the
available book ratings from the dataset, but also forces the vector
embeddings of similar books/users to be close to each other. As a
result, when we want to perform recommendations on books/users in the
future, we simply need to do a *k*-nearest neighbor search on the
book/user embeddings, the values of which can be precalculated. This is
similar to what is done for hybrid collaborative filtering, but we are using a much more robust model than
matrix factorization.

#### Training

We chose to use a skip-gram-like approach, described in our [Summarization](https://summarization.fastforwardlabs.com/) report, to train
our model. This means we randomly sample a book and rating from the
user’s history to use as the target of the model and then take a random
number of the remaining books to represent the history. This is done
many times for each user, so we are able to augment each user’s history
with different permutations. Furthermore, we randomly sample books that
the user has never interacted with in order to generate negative
samples. As described in [Missing Data and Evaluation](#missing-data-and-evaluation), this sort of approach
can be problematic, but in the case where we have no access to the users
generating the data it’s the only approach possible.

In order to deal with performance issues, all of the summaries in all of
the skip-grams, in their word2vec format, were cached and saved to an
hdf5 file.^[Using the h5py package; see http://h5py.org] The resulting two files are 821 GB (2,257,588 samples) for
training and 35 GB (98,414 samples) for validation. Even with this
precaching, each epoch took 1.5 hours (with an average of 58 epochs
required) to converge. However, without caching it would have taken ~6
hours per epoch, and without a GPU (but with caching) it would have
taken 117 hours per epoch!^[Our tests were done with an NVIDIA Tesla Titanium and an Intel Xeon CPU E5-2620v3.]

These long training times show the necessity of having multiple GPUs
when developing these models. The model exploration and hyperparameter
tuning phases both require training many models and seeing which perform
best on the data. Each one of these models can be trained independently
of the others and training can thus be parallelized simply by launching
more training operations. So, having eight GPUs means that the process
can happen eight times faster. One of the major limitations to our model
was the time constraint in this exploration phase, which could have been
remedied by having more GPUs available. AWS’s `p2.*xlarge` instances,
Paperspace’s dedicated GPU instances,^[Paperspace is a cheap by-the-hour cloud provider that provides the newest NVIDIA GPU, the Volta.] and other cloud computing services help tremendously in this area.

#### Evaluation

In order to evaluate our model, we chose to look at the
root-mean-squared error (RMSE) between our predicted rating that a user
would give a book and the actual rating. Furthermore, we made sure that
there was no leakage between our training set and our testing set: no
users or books are shared between the two sets of data. While this is
the correct way to separate out the training and testing data, it also
greatly reduces the sizes of each dataset. Results from our model
comparison between our bi-modal model and other classic approaches to
recommendations are shown in the following table:

<table>
<caption>Evaluation Results. Scores are normalized between 0 and 1.</caption>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;"><p>Algorithm</p></td>
<td style="text-align: left;"><p>RMSE</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Random</p></td>
<td style="text-align: left;"><p>0.38</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>NMF (ID)</p></td>
<td style="text-align: left;"><p>0.84</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>NMF (Subject)</p></td>
<td style="text-align: left;"><p>0.46</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Spotlight MF (ID)</p></td>
<td style="text-align: left;"><p>0.62</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Spotlight MF (Subject)</p></td>
<td style="text-align: left;"><p>0.27</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Bi-Modal</p></td>
<td style="text-align: left;"><p>0.09</p></td>
</tr>
</tbody>
</table>

Evaluation Results. Scores are normalized between 0 and 1.

In comparison with classic methods, our Bi-Modal model performed quite
well! An RMSE of 0.09 means that, on average, our prediction for a
rating is off by 0.04 stars (where books are rated from 1-5 stars). On
the other hand, because of class imbalances, a random prediction of
ratings would be off by 0.722 stars.

One feature we see in the results is how much better the recommendation
algorithms that make use item properties are, as opposed to
those that use just an ID. For example, using Spotlight’s matrix
factorization on only book IDs results in a worse-than-random result;
however, factoring in the subjects of the books gives us the best
results from the classic methods.

This also gives us an indication as to why the Bi-Modal model has such a
jump in performance compared to the other models. The number of books
being considered in the catalogue is quite large and the number of
interactions quite small in comparison. With only the book IDs, there
simply is not enough interaction information to find common trends. By
augmenting this data with the book subject, we are able to leverage
information we know about genres in order to make inferences about books
that may not have been interacted with that much. In the extreme case,
our Bi-Modal model makes use of the summary, a much richer set of
metadata than just the subject, allowing us to extract nuances that
genre simply can’t capture (for example, the similarity between computer
books and certain types of sci-fi).

#### Failures

While the final model we created for the prototype did well in
comparison to the classic methods (as shown in the table above),
there were many things that didn’t go as expected. One of the problems
stemmed from the results for user embeddings.

In our description of the overall model structure, we planned to take
advantage of the sequence of books a user reviewed to create an
embedding for users. As explained before, the benefit would be that both
users and books would be embedded into a similar space and distances
between them would be meaningful. However, while book-to-book distances
and user-to-user comparisons performed very well, user-to-book
comparisons did not. The distances between books and users were always
large, indicating that the model learned to create structure for books
separately from the structure for users (i.e., books were all in one
cohesive cluster and users were in another).

The most compelling explanation for this is that the model simply did
not converge fully. Neural networks tend to converge from the bottom up
(layers near the input layer converge sooner), and the book-to-book as
well as the user-to-user embeddings show signs of individually having
converged in the final model (this was seen by observing the distances
between the final embedding values; user-user embeddings had close
proximity as did item-item, however user-item embeddings remained far
away from each other). More data could help with this, or simply a
variation on the model structure we chose. However, because of the time
necessary to train each new variation, we weren’t able to continue our
model search.

Another general limitation of our model is that we had to truncate the
summaries of books to 64 words, a value chosen for the resource
implications of the training procedure and the observation that many
summaries devolve into quotes and awards after this many words. This
truncation happens after we do some filtering, but it still requires us
to throw away a lot of potentially useful information (further
contributing to our problems with convergence). This filtering was a
necessary way to speed up model training because of our limited time
frame. Generally, filtering tricks like this are used to accelerate
training for a model survey, and then the full dataset is used to train
the final model. However, in our case training the final model with the
full dataset would have been too slow to be useful. By truncating the
data we were able to precache all of the word-vector inputs to the model
and store them to an hdf5 file, a format which Keras can read quickly;
moving to the full dataset would have required us to compute these word
vectors on the fly, further slowing down the model.

### Product: Deep Bargain Book Shop

The simplest interface for a recommendation system is a list of
recommendations. For our prototype, we started with a list, but quickly
realized we needed to provide the user more context about how our
recommendation system worked. To do that we turned to a dimensionality
reduction algorithm to help us visualize the system.

#### Visualizing the System

We used a technique called called **t-distributed Stochastic Neighbor
Embedding** (t-SNE) to create a visualization of our recommendation
system. T-SNE diagrams have become popular with data scientists that
work with neural networks as a tool for understanding how a model is
making its decisions. Using t-SNE, you can take the multi-dimensional
relationships encoded in a model and reduce them down to a
two-dimensional plot.

The t-SNE visualization for our prototype represents each book in the
recommendation system as an `x,y` coordinate. Similar books (as
determined by the model) are near one another. This representation
dovetails with our natural human spatial reasoning abilities, providing
an intuitive way to explore a system.

![Figure 4.7](figures/04-product-01.jpg)
*Figure 4.7 An early version of the prototype with the t-SNE diagram on the left.*

Anytime you use t-SNE to explore a system, it is important to remember
that it is necessarily a simplified representation of that system. Our
model is comparing book descriptions across 256 dimensions. The t-SNE
technique tries to find the best way to reduce those relationships down
to just two dimensions.^[The imprecision of the method can be seen in the final prototype where the recommended book rankings do not *exactly* correspond to their distance from the selectecd book in the t-SNE. The discrepancy is a consequence of compromises made among lost dimensions.] As long as you keep that limitation in mind,
a t-SNE visualization can be a great general guide to a system. We used
it both to help debug the system as we built it and, in the final
prototype, to help explain the system to users.

Making the t-SNE visualization was challenging on several levels.
Generally what you hope to see in a t-SNE plot is some type of
meaningful clustering, like books of the same genre being near each
other. Whether you see clustering or not could be an indication about
whether your model is working, or it could be an indication you haven’t
found the right t-SNE parameters.^[The interactive article [*How to Use t-SNE Effectively*](https://distill.pub/2016/misread-tsne/) provides a good overview of how different parameters affect the visualization.] Often, as in our case, t-SNEs take
quite a long time to run, so the parameter search can involve a lot of
waiting. We tend do a lot of small quick interation on our prototypes
and t-SNE generation was not a natural fit for that process.

On the front-end side, interactively displaying all the points in the
t-SNE was a technical challenge. More conventional browser visualization
techniques, like using an SVG or canvas element, bogged down when we
threw more than 5,000 points at them. We turned to the Three.js
Javascript library which is focused on 3D graphics. Its use of WebGL
(which uses your computer’s GPU) let us pack in over 200,000 points
without strain.^[Using a 3D focused library for a 2D visualization was not without friction. You can read about some of the challenges in our [blog post](http://blog.fastforwardlabs.com/2017/10/04/using-three-js-for-2d-data-visualization.html)] In our final version, we reduced the number of points
down to 10,000 to make the file size of the data manageable.

![Figure 4.8](figures/04-product-02.jpg)
*Figure 4.8 An early version of the prototype showing how a book the system has not seen before* (Obama: An Intimate Portrait) *is placed in relation to books already in the system.*

Was the hard work worth it? We belive it was. The t-SNE visualization
brought a layer of context to the prototype, showing how recommendations
based on one item fit into the larger system. It also provided a nice
illustration of our system’s star feature – its ability to make
recommendations for an item it has never encountered before. As shown in
the visualization, it is able to do this by placing the new item,
through an embedding of its text description, in relation to the items
already in the system. The recommendations for that item are then simply
the items "nearest" its position.

#### Deep Bargains

The t-SNE helped explain the technology underlying the prototype, but
our prototypes are not solely technical demonstrations. They are also
designed to show the product possibilities that tech creates. We knew
that our system’s ability to make recommendations for new items,
bypassing the cold-start problem that many recommendation systems have,
was a big deal, but we needed to do more to show that usefulness in the
prototype.

We were also confronting an expectations problem. The Amazon reviews
dataset we used for our prototype contained a limited number of books
from a limited time period. If not framed properly, this could have a
negative effect on how users viewed the recommendation results. While
the system can make recommendations based on any arbitrary book, it can
only draw those recommendations from books in its dataset. This might
cause users to judge the recommendations overly harshly, especially if
they were comparing them to Amazon’s system, which has a much larger
range of recommendation candidates to choose from.

![Figure 4.9](figures/04-product-03.jpg)
*Figure 4.9 Deep Bargain Book Shop, an imaginary online book shop that helped us explain the strengths of the recommendation system.* 

We came up with a story for the prototype that explained the limited
selection and also highlighted the model’s ability to make
recommendations on new items. The prototype would be an imaginary online
book shop, "Deep Bargain Books", whose eccentric owner had both a
limited selection of bargain priced books and machine learning
expertise. This scenario helped set expectations for the limited
selection and provided the backdrop for us to demonstrate some of the
business opportunities semantic recommendations can unlock.

The most obvious advantage of a semantic recommendation system to a
business owner is the ability to add a new item to inventory and
immediately integrate that item into the relevant recommendations.
Systems, like collaborative filtering, that rely solely on user and item
interaction are unable to do that. The product possibilities go beyond
expanding inventory, however. In our prototype the recommendation system
is the method for the user to explore the selection of books. To find
books they are interested in, the user can search for a book they like
and, provided there is a description for that book in the Google Books
API, immediately view relevent recommendations for it. The front page
features recommendations based on current New York Times bestsellers.
The recommendation interface becomes a method of navigating through the
catalog, highlighting relevant in-stock books for the customer.

![Figure 4.10](figures/04-product-04.jpg)
*Figure 4.10 The final prototype, featuring the customer view on the left and the admin view on the right.* 

We finished the story of the book shop by moving the t-SNE visualization
and further information about the recommended books into an "Admin"
view. This was an acknowledgement that while this information was useful
in understanding the system, it could be overwhelming for the average
customer just looking for a book. The admin section also features the
most extreme version of the model’s cold-start capability — the option
for the user to enter a freeform text description and see
recommendations based on that description.

#### Further Product Possibilities

##### Marketing Tools

The ability to add your own custom description hints at more tools that
could be built with semantic recommendation systems. A product directed
at book publishers could provide feedback for choosing a description for
a book. It could show the similarity between the chosen description and
other already existing books. If you wanted a book to be recommended
alongside *Harry Potter* you could tailor the description to try and do
that. If you had user embeddings alongside the books, you could even see
a prediction of the book’s audience based on the entered description.

##### t-SNE as Interface

So far t-SNEs are primarily being used as tools for data scientists. We
are optimistic about the opportunities for using visualizations as part
of an interface for end users. They could help users better understand
the recommendations they are receiving and provide an intuitive way to
make changes to their own taste profile. They can also help us
understand how we explore a topic, as in our Wikipedia mapping project,
Encartopedia.^[http://encartopedia.fastforwardlabs.com>. Created by Sepand Ansari.] Making t-SNE interfaces approachable will require smart
design work, and making them interactive in the browser will require
heavily-optomized front-end code, but the payoff could be tremendous. As
recommendation systems mediate more and more of our interactions, the
desire of customers to understand just why they’re being shown what
they’re being shown will grow. Thoughtful visualizations could step in
to fill that need.

### General Engineering Considerations

There are several engineering considerations in deploying a model like
the one we implemented. First, the full model is quite big — while the
model itself is only ~50 MB,^[Note that this 50 MB will reside in the GPU memory.] we must also have our word2vec model
loaded, which takes up 2.6 GB. In addition, we must have access to all
of the book summaries, which comprise another data structure of
considerable size that must be stored in memory. These considerations,
however, can be mitigated by good engineering practices: the word2vec
model can be trimmed down and stored in an on-disk database, as can the
book summaries.

Still, the model very much wants to be run on a GPU. When encapsulated
in an HTTP API, the total time to embed a book description using the GPU
is 0.082s, while the same operation on a CPU takes 0.189s. This 2.3x
slowdown may be justifiable in comparison to the cost of a GPU, but it
also may make the difference between whether an application is feasible
or not (for example, if you have 4 hours every night to precompute
recommendations for your users, a 2.3x slowdown may simply make the
process take longer than the allotted time).

Finally, model management can become an issue with these sorts of
methods. During the hyperparameter tuning and model exploration phases,
many dozens of models are trained, evaluated, and compared. Even after a
final model is found, it is possible that this experimentation will
continue as new data is created and the user feedback of the deployed
models is considered. Having accountability as to what data the models
were trained on is critical for this, since we don’t want to leak
information from the training to the validation steps.

One method to help alleviate this is to have all parameters, links to
data, and random seeds in a particular model be hardcoded into the model
source and to pin a particular trained model to a commit hash in Git.
This means that for any trained model, the model code at the proper
commit hash can be checked out and rerun to produce the same results.
