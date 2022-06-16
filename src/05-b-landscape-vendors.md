## 5. Recommendation Vendors

Recommendation services fall into two broad categories: general purpose
and purpose built. Some vendors will sell you an API that returns
recommendations/predictions, and some vendors will tailor a solution to
your business needs. A tailored solution (one would hope) ought to be
more accurate and better suited to a specific domain. Note also that a
state-of-the-art recommender can take advantage of semantic information
in the features that prior, less advanced, recommenders would ignore.

### General-Purpose APIs

General-purpose APIs offer one-size-fits-all recommendations or
predictions with no (or little) adaptation to a given problem. This
makes them easier to deploy, but there’s a slight performance cost in
that a less-tailored configuration will not take advantage of
domain-specific information. The larger APIs have no problems with scale
and generally perform well.

#### Microsoft Cognitive Services Recommendation Module

The Azure recommendations module^[See
https://azure.microsoft.com/en-us/services/cognitive-services/recommendations/] is in preview at the time of
writing. It’s not fully integrated into Microsoft’s Cognitive Services
ML platform yet, but it functions well independently.

Azure’s recommendation engine views the world from a product
recommendation standpoint. The inputs to the model are a catalog file
and a usage file. The catalog file contains the master list of products
available and some basic information about the products, like name,
category (e.g., "Software," "Gaming," or "Services"), a description, and
any additional features that are useful to your application. The usage
file simply describes interactions that have taken place, including the
user, product, time, and (optionally) event type, such as purchase,
click, or add to cart. Azure offers several options for training the
model, allowing some control over model features, and provides insight
into post-training model evaluations.

Azure has a convenient API explorer;^[See https://westus.dev.cognitive.microsoft.com/docs/services] it includes specifications for
data formats and each operation (e.g., uploading data, training a model,
and getting recommendations), as well as a testing console for each
method that generates request code in several languages and shows you
the responses from the API in real time. Microsoft also includes sample
data to guide you through the process of using the API. This explorer
makes it simple to get started with recommendations.

Overall, we found Azure’s API simple to set up and use, and it produced
reasonable results fairly quickly.

#### Amazon Machine Learning

Amazon’s Machine Learning (ML) platform^[https://aws.amazon.com/aml/] puts recommendations under the rubric of predictions, a reasonable view given that a recommendation is in effect a prediction of what a user would like to see, buy, or otherwise interact with. The service is less focused on products than
Azure’s, which is somewhat surprising in light of Amazon’s origins as a
retailer.

Amazon’s recommendation engine is not as configurable as Azure’s when it
comes to training — for example, it trains all models using regression.
Its data is somewhat more flexible, though. Input is from one master
file uploaded to Amazon’s cloud storage service, S3. It takes only a
list of user-product interactions, but each interaction can include an
arbitrary number of features used to train its models. The lack of a
catalog file implicitly means that Amazon ML’s "catalog" is gleaned from
the usage file itself, recognizing no products that have not been
interacted with.^[This has implications for cold starting new products or users:
specifically, that they must be handled by comparisons of category alone.]

Amazon ML can be configured through the AWS web console for setup and
testing. It is not as simple to use as Azure’s offering, but it provides
reasonable guidance and statistics on model quality. Once set up, it can
be accessed through provided SDKs in several languages, including Java,
Python, and JavaScript through Node.js.^[See https://aws.amazon.com/aml/getting-started/] Android and iOS SDKs are also
offered. Amazon ML currently runs only on machines in Amazon’s US East
(Northern VA) and EU (Ireland) data centers, which could diminish
request performance in other regions.

We found that Amazon ML was more difficult to set up than Azure, but was
reasonably straightforward and had similar performance.

#### Google Cloud Prediction

For now, Google offers its Cloud Prediction API using Spark. We did not
evaluate this product in depth, though, since Google has declared that
it will no longer support its prediction service after April 2018.
Google refers Prediction API users to its Cloud Machine Learning Engine
using TensorFlow,^[See https://cloud.google.com/architecture/recommendations-using-machine-learning-on-compute-engine] and has admitted that Cloud Prediction was unmaintained and had few users.^[See https://news.ycombinator.com/item?id=14343389] This suggests that Google has little
interest in serving recommendation customers.

### Smaller Vendors

In addition to the Goliaths of web services above, there are numerous
smaller vendors that provide recommendation APIs. Some are more
specialized than others. We list some of these vendors below.

#### Domain-Focused API Vendors

Certain APIs are targeted at specific types of recommendations. Fashion
is one special case, because the recommendations must be adaptable to
temporal constraints (fashion products expire much more quickly than,
say, books, movies, or tools). Offerings in this area include:

-   Vue.ai^[https://vue.ai/] - Vue mainly targets the fashion industry, but also
    handles other retail outlets (furniture, for example).

-   Apptus^[https://www.apptus.com/customer-success/customers] - Similar to Vue, Apptus’s clients include several in the
    fashion industry and a lot of well-known retailers.

#### Other Vendors

The vendors below offer recommendation services that are less directed
to a specific domain:

-   Rich Relevance^[https://www.richrelevance.com/] - Rich Relevance is a personalization vendor
    whose platform includes a recommendation engine. They have a number
    of high-profile clients across a broad range of industries,
    including fashion, food, and electronics.

-   YUSP^[http://www.yusp.com/solutions/] - Like Rich Relevance, YUSP offers a personalization engine
    with a recommender included. Its engine is adaptable for product
    recommendations, email campaigns, coupons, and in-store
    interactions.

-   Strands Retail^[http://retail.strands.com/] - Strands Retail offers a recommendation API in
    the form of a JavaScript library. The library can be used to add
    recommendations to websites and emails, adapted for user needs.

-   4-Tell^[https://get4tell.com/] - 4-Tell offers business-to-consumer and
    business-to-business platforms. Its offering includes inline search
    completion recommendations and email content recommendations.

-   Recombee^[https://www.recombee.com/] - Recombee offers a general-purpose recommendation
    engine through its API. Packages are provided for most popular
    languages, including Python, Ruby, Java, and Node.js.

-   Tamber^[https://tamber.com/] - Tamber claims that its API uses state-of-the-art
    algorithms, tuned for maximum performance, and avoids feedback loops
    and addresses the cold start problem. The API libraries are offered
    in many popular programming languages.

-   Barilliance^[https://www.barilliance.com/] - Barilliance’s recommendation engine uses online
    interactions, but can also combine them with point-of-sale data from
    brick-and-mortar stores for a given customer to improve
    recommendations. It offers a configurable API that allows some
    customization with API users' business rules.

-   Trouvus^[http://trouvus.com/] - Trouvus provides a retail recommendation engine and an
    engine specialized for video-on-demand applications.
