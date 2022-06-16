## 6. Open Source Projects

Open source recommendation systems are useful for putting together basic
systems, getting an intuition for the performance of various algorithms
on a particular dataset, and sanity checks. Depending on the maturity of
the packages, some can be deployed in production. There are many in the
wild; we focus on the handful that are recent, complete, and well
documented.

### Surprise

Surprise^[http://surpriselib.com/>] is a Python scikit for recommendation systems built by
Nicolas Hug. Out of the box, the package provides many popular
recommendation algorithms as well as
the MovieLens dataset and a dataset of anonymous ratings from Jester, an
online joke recommender system. Custom datasets can be loaded from a
file or from a Pandas DataFrame. Installing Surprise is straightforward;
the only dependency is NumPy. While useful for smaller datasets, we ran
into tractability issues for neighborhood algorithms on large datasets.

### LightFM

LightFM^[http://lyst.github.io/lightfm/docs/home.html] (built by Maciej Kula while at Lyst) is a Python package that
provides a hybrid recommendation model by incorporating metadata at both
the user and the item level. Both implicit and explicit models (see
[Collaborative Filtering](#collaborative-filtering)) are included. Implicit models are trained through
negative sampling, where items are randomly sampled to act as negatives
(similar to the negative sampling used in the embedding models). The
model reduces to traditional matrix factorization when no metadata is
provided. The package has the MovieLens dataset built in, and external
datasets can be accommodated by transforming them into matrix form.^[Input data is a sparse matrix where rows represent users, and columns represent items.]
LightFM is written in Cython and can run on multiple cores. Installation
for multicore functionality is trickier on macOS but can be done via
Docker.

### Spotlight

Spotlight^[https://maciejkula.github.io/spotlight/] is a Python package built using PyTorch that enables users
to build traditional and neural network-based recommendation systems. It
was also written by Maciej Kula. Both implicit and explicit models are
available. Similar to the LightFM package, implicit models are trained
through negative sampling. The package provides the MovieLens and
Goodbooks datasets.^[The Goodbooks dataset contains six million ratings for ten thousand of the most popular books.] It also has a module for generating synthetic sequential data with known properties; this type of data is useful when
testing deep recommendation models. External datasets can be
accommodated by transforming them into the internal Spotlight
representation with a few lines of code. Installation is relatively
straightforward; PyTorch is an obvious dependency.

### Implicit

Implicit,^[http://implicit.readthedocs.io/en/latest/] built by Ben Frederickson, is a Python package that
provides a collaborative filtering model for implicit datasets based on
observations of user actions. To handle implicit data, rather than using
negative sampling the package implements a specific matrix
factorization-based model to infer ratings from, for example, the number
of times a user fully watched a show. Implicit does not have built-in
datasets; input data needs to be in a matrix form.^[Specifically, a compressed sparse row (CSR) matrix, where rows represent items and columns represent users.] Installation is straightforward. Running with macOS requires an OpenMP compiler.

### Apple Turi Create

Apple very recently made its Turi Create engine available on GitHub,
including a recommender.^[See https://github.com/apple/turicreate/tree/cce55aa5311300e3ce6af93cb45ba791fd1bdf49] Apple did not develop this engine itself,
but acquired it along with Turi. Given the timing of this release, we
were not able to test it. Installation instructions are straightforward,
but Python 3.5+ is not yet supported.

Turi Create uses SFrames as the primary data structure for extracting
data from CSV, JSON, and SQL formats. It supports both explicit and
implicit feedback and provides matrix factorization and
neighborhood-based recommendation algorithms. The quickest way to get a
recommender up and running is to let the system automatically choose a
model based on the properties of the data. For example, if the input
data only has user and movie pairs, a ranking model based on item
similarity (neighborhood approach) will be chosen. One can also specify
a model explicitly. Turi Create partially addresses the cold start
problem for new items by supporting neighborhood models for item
content.

### Apache Spark

Apache Spark is an open source cluster computing framework. Its machine
learning library (MLlib) has a matrix factorization-based recommendation
algorithm trained using an alternating least squares (ALS) method. It
supports both explicit and implicit feedback. For neighborhood methods,
LSH is also included in the library. Spark’s recommendation engine is
scalable, distributed, and can be deployed easily into a web
application.

### Our Recommendations

<table style="width:100%;">
<caption>Open source systems at a glance.</caption>
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;"></td>
<td style="text-align: left;"><p>Surprise</p></td>
<td style="text-align: left;"><p>LightFM</p></td>
<td style="text-align: left;"><p>Spotlight</p></td>
<td style="text-align: left;"><p>Implicit</p></td>
<td style="text-align: left;"><p>Turi</p></td>
<td style="text-align: left;"><p>Spark</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Neighborhood</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Matrix factorization</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Hybrid matrix factorization</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Sequence models (LSTM, pooling, CNN)</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✖️</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Implicit models</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Custom dataset</p></td>
<td style="text-align: left;"><p>custom object via file, Pandas</p></td>
<td style="text-align: left;"><p>matrices</p></td>
<td style="text-align: left;"><p>custom object</p></td>
<td style="text-align: left;"><p>matrices</p></td>
<td style="text-align: left;"><p>CSV, JSON, SQL</p></td>
<td style="text-align: left;"><p>RDD</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>Scalability</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>Production ready?</p></td>
<td style="text-align: left;"><p>✖️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
<td style="text-align: left;"><p>✔️</p></td>
</tr>
</tbody>
</table>

*Open source systems at a glance.*

Of these packages, Surprise is most useful for learning about basic
recommendation systems when you have smaller datasets (thousands of
users and thousands of products) with explicit data. Spotlight, on the
other hand, allows you to experiment with deep learning techniques and
compare the results with those of traditional matrix factorization
recommenders. In addition, Spotlight scales to larger datasets (tens of
thousands of users and tens of thousands of products) and can handle
both implicit and explicit data.

LightFM and Implicit are specialized packages. If you have metadata in
addition to just user-product ratings, LightFM can be used to build a
hybrid recommender where the cold start problem is alleviated. If you
only have implicit data (for example, the length of time users spend on
a particular show), Implicit allows you to build a recommendation system
based on ratings inferred from the available data.

In terms of speed, Turi Create seems to provide the quickest path to a
working recommendation system. Spark’s matrix factorization-based
recommender also provides an easy way to build a basic scalable and
deployable system using data in existing clusters.