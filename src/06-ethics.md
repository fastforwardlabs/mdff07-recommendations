## 7. Ethical Considerations

With the increasing popularity of online retailers, websites, and social
media platforms, the amount of physical and digital goods (as well as
news and entertainment media content) available to us has increased
greatly. Whereas brick-and-mortar stores are limited by shelf space in
terms of what physical goods they can offer, online stores, unencumbered
by these limits, can propose a much wider variety. As of November 2017,
Amazon, for example, had a total of 573,374,133 products on sale.^[https://www.scrapehero.com/how-many-products-does-amazon-sell-november-2017/]

Given internet access and efficient logistics, (almost) regardless of
consumer location, the greater variety of available goods and content
means niche interests can be served. But with greater variety comes the
problem of discoverability: the sheer volume of distinct products makes
it hard for consumers to find the ones they may like or want.

![Figure 7.1](figures/07-thinking-01.png)
*Figure 7.1 The sheer amount of items available online make filtering strategies like recommendation systems necessary.* 

Search allows consumers to find items they *know* exist, while
recommendations systems introduce consumers to items they may *not* know
exist: items they may like, items they may want to buy. For subscription
platforms (e.g., Spotify, Netflix), recommenders enable subscribers to
make easy use of the platform. Recommendation systems help us navigate a
world of variety, novelty, and consumer choice.

In terms of consumption, the number of hours per day per user has
remained constant: there are only so many products we can buy, songs we
can listen to, movies we can watch, and stories we can read in a single
day. Money has always been a limiting resource, but over the past years,
*attention* has become a newly scarce one. Recommendation systems guide
our attention as we navigate this world of variety, novelty, and choice;
they are convenient, and increasingly ubiquitous.

As such, recommendation systems are powerful. For example, an informed
citizenry is a crucial component of a well-functioning democracy, and
society relies on the news media for information. Increasingly,
recommendation systems guide the news we consume. The design and
deployment of such systems requires thought to ensure that they are
actually helpful, and not harmful.

### Filter Bubbles and Echo Chambers

Internet activist Eli Pariser coined the term "filter bubble" circa
2010,^[https://www.penguinrandomhouse.com/books/309214/the-filter-bubble-by-eli-pariser/9780143121237/] to describe the personalized ecosystem of information
delivered to a user by search and recommendation systems that filter
content through the lens of past behavior. A related concept is that of
the "echo chamber," where people with similar viewpoints share and
discuss information or ideas in a self-reinforcing manner, leading to
the exclusion of other perspectives.

![Figure 7.2](figures/07-01.png)
*Figure 7.2 If they are not calibrated carefully, recommendation systems can limit users to a filter bubble based on their past behavior.* 

These concepts highlight a growing concern that algorithms could
contribute to a world in which people are exposed to less diverse
viewpoints over time. In 2013, the EU High Level Group of Media Freedom
and Pluralism noted that "Increasing filtering mechanisms makes it more
likely for people to only get news on subjects they are interested in,
and with the perspective they identify with. Such developments
undoubtedly have a potentially negative impact on democracy."^[http://ec.europa.eu/information_society/media_taskforce/doc/pluralism/hlg/hlg_final_report.pdf]

However, the jury is still out on the role of algorithms in creating
filter bubbles and echo chambers. For example, individual choices about
what content to consume, especially when it comes to news media, have
always been subject to factors such as confirmation bias (the tendency
to search for, interpret, favor, and recall information in a way that
confirms one’s preexisting beliefs or hypotheses) — and a study
conducted by Facebook in 2015 found that individual choices more
strongly determine news media consumption than the Facebook news
feed.^[ See
http://science.sciencemag.org/content/early/2015/05/06/science.aaa1160.
Given the researchers' affiliations, there may, of course, be a conflict
of interest at play.]

Regardless of how filter bubbles and echo chambers are actually created,
though, semantic recommenders may further "seal" the bubble or chamber.
Prior to semantic recommenders (due to the lack of a solution for the
cold start problem), developers had little choice but to introduce to
users new items that they did not yet know or predict the users would
like. Consequently, users could encounter items not in accordance with
past behavior, thereby increasing the diversity in the set of items they
were presented with. With semantic recommenders such chance encounters
are less likely to happen. Furthermore, since semantic recommenders can
trace the development of user preferences over time (driven, in part, by
current mood and opinion) and produce recommendations accordingly, they
can act as polarizers in times of heated discourse.

To combat the potentially harmful effects of personalized search and
recommendation systems, some suggest that developers use "exposure
diversity" as a design principle for these systems.^[http://www.tandfonline.com/doi/full/10.1080/1369118X.2016.1271900] Diversity becomes
part of what the algorithm optimizes for. Exposure diversity may help
foster our collective ability to digest diverse viewpoints for civil
discourse, for example, and even reduce confirmation bias:
preference-inconsistent recommendations are known to trigger critical
thinking patterns that can help overcome such bias.^[See https://www.sciencedirect.com/science/article/pii/S0747563212001963] We may,
furthermore, consider the development of tools to inform readers about
their news diet, providing an overview of news consumption behavior in
aggregate, and develop recommenders expressly designed to surface
articles on topics of interest, but with an opposing view, to help
inform readers about the extent of their bubble and encourage more
balanced consumption.

### Bias

Recommendation engines can encode biases and perpetuate unwanted or
harmful behavior. A study conducted in 2015 found that compared to men,
women see fewer ads for high-paying jobs on Google.^[See https://www.theguardian.com/technology/2015/jul/08/women-less-likely-ads-high-paid-jobs-google-study] Another study
found that searching for names primarily given to black babies (e.g.,
DeShawn, Darnell, and Jermaine) generated public-record ads suggestive
of an arrest record in 81 to 86 percent of searches on one website, and
92 to 95 percent on another. When searching for names primarily given to
white babies (e.g., Geoffrey, Jill, and Emma), the word "arrest"
appeared in only 23 to 29 percent of ads on the one site and 0 to 60
percent on the other.^[https://arxiv.org/abs/1301.6822]

Furthermore, recommenders may simply work less well for minority groups.
Since recommenders are evaluated with a focus on the system’s overall
effectiveness, and since larger subgroups tend to dominate overall
statistics, the satisfaction of dominant user groups is weighted more
heavily than that of minority groups,^[See http://ceur-ws.org/Vol-1905/recsys2017_poster20.pdf] which is a form of
discrimination.

Finally, recommended items tend to get higher ratings because
recommendations "anchor" user ratings — that is, users give higher
ratings to an item *because* the item has been recommended to them. This
dynamic of "the rich get richer" can prevent upstarts (like small
vendors on Etsy) from growing their businesses, perpetuating the status
quo.

Semantic recommenders, like behavior- or demographics-based
recommenders, can encode all three types of biases, while their solution
to the cold start problem may in fact exacerbate the "rich get richer"
dynamic. Unfortunately, combating bias is hard. Women and men tend to
wear different clothes, so a retailer may want to use the variable
"gender" in clothing recommendations. But it is wrong to serve
"gendered" job recommendations.

Removing protected category information from data that powers
recommenders is a start, but protected category information tends to
correlate and interact with other variables in unexpected and unknown
ways. Model introspection may help us understand how algorithms create
recommendations and spot bias. But neural recommenders (like semantic
recommenders) are, compared to their non-neural cousins, harder to
introspect. We recommend the use and development of tools to audit and
test recommendation engines for bias, both during development and once
deployed.^[An example of such a tool is AdFisher, developed by Carnegie Mellon
University; see http://possibility.cylab.cmu.edu/adfisher/] Equipped with such tools, engineers should make a point of
adding "bias tests" to their suite of unit tests, functional tests,
regression tests, etc.

To ensure that recommenders do not serve only the majority, we can
downsample majority groups (or upsample minority groups) so that all
groups are represented equally in the data used during the development
of recommendation systems. In parallel, we need to develop evaluation
methods that ensure that recommenders serve all groups equally well,
even at the expense of their overall effectiveness. Both mitigation
strategies force us to define majority and minority groups, which is
admittedly a minefield — but without such definitions, we lack
strategies to combat biases that exist today.

Finally, to break the "rich get richer" dynamic, we can use algorithmic
methods to estimate and subtract the effect of a recommendation on a
user rating, as has been done by researchers at Cornell University.^[See
http://papers.nips.cc/paper/6362-beyond-exchangeability-the-chinese-voting-process]
Combating bias is hard, but there are tools developers can use to reduce
bias in recommendation systems.

### Attacks & Gaming

Recommendation engines can be designed (or "gamed") for economic
benefit, rather than for best serving the needs, wants, and interests of
users. Retailers selling their wares online can use recommendations to
anchor consumer preference and expectation,^[See
https://arizona.pure.elsevier.com/en/publications/recommender-systems-consumer-preferences-and-anchoring-effects] nudging consumers to buy more expensive items.

Retailers and content producers selling their wares on aggregator
websites or through social media platforms (where they compete with
others) can game recommenders to recommend their products or content
over another’s. "Shilling" attacks, for example, are malicious attempts
to change recommendations by inserting fake user profiles into user-item
matrices.^[See
http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0130968]

Recommenders based on user behavior are particularly prone to shilling
attacks, while semantic recommenders, since they make use of content (in
addition to user behavior), are more robust. They are, however, prone to
another form of gaming: content expressly designed to be favored by
recommendation engines.

In his post "Something Is Wrong on the Internet,"^[https://medium.com/@jamesbridle/something-is-wrong-on-the-internet-c39c471271d2] James Bridle
provides an example of what can happen when content is designed for
recommendation engines first and foremost, not people. Children’s
entertainment on YouTube is lucrative: children enjoy watching videos of
Peppa Pig nursery rhymes, and the unwrapping of Kinder Surprise Eggs.
When a video ends, recommendation engines surface the next recommended
video, and with autoplay turned on, it will play after just a brief
interruption.

![Figure 7.3](figures/07-02.png)
*Figure 7.3 Recommendation systems are vulnerable to gaming through algorithmic content generation based on popular keywords or topics.* 

While some of the recommended videos are appropriate suggestions, some
have "word salad" titles that feel like they are not intended for human
consumption — and they aren’t: they are algorithmically designed to be
favored by the recommender. The content of some of these videos is
decidedly "odd" (and some are even disturbing or violent). With ever
faster and more inexpensive forms of content generation, from cheap 3D
animation to fully computer-generated content, these videos may
eventually crowd out more appropriate content and prove lucrative to
their creators because of their high ranking by YouTube’s recommendation
engine (content producers receive a share of ad revenue).

Recommendation engines are also gamed for ideological reasons. Terrorist
and hate groups disseminate content online (including via social media
and platforms like YouTube). They have an interest in increasing the
visibility of their message to radicalize target groups. A report by
Data & Society on Media Manipulation and Disinformation Online,
published in May 2017,^[See
https://datasociety.net/output/media-manipulation-and-disinfo-online/] outlines how internet subcultures take
advantage of the current media ecosystem (including search and
recommenders) for their benefit, with detrimental consequences to
society (e.g., spreading misinformation and eroding trust in traditional
news media organizations).

Gaming, fraud, and abuse are a cat-and-mouse game; they can never be
entirely eradicated. Still, fake user profiles tend to be dissimilar
from existing user profiles: there are metrics to determine the
probability of a profile being real, or the result of a shilling attack,
that can immunize recommenders against such attacks.^[See
http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0130968]

To combat content-based gaming, a new phenomenon, the recommendation
systems development community needs to create strategies to identify
"fake content" at scale. We could, for example, flag content with word
salad titles or block accounts that publish very similar content very
often (suggestive of content autogeneration for gaming). To date,
companies faced with the fake content challenge have struggled to
develop such strategies, because it is a very hard problem and there is
no silver bullet; YouTube recently hired thousands of (human) content
moderators to weed out inappropriate content, partly in response to
Bridle’s post.^[See https://www.ft.com/content/080d1dd4-d92c-11e7-a039-c64b1c09b482]

One of the central challenges in this debate and effort — a challenge we
face as a society — is the definition of what is (and what is not)
appropriate content (and who will be the arbiter). While there are no
definite answers to this question, there are organizations and
committees drafting possible solutions, some of them recently formed in
the light of new challenges.^[E.g., the [Partnership on AI](https://www.partnershiponai.org/)
and several initiatives by the [Ford Foundation](https://www.fordfoundation.org/)]

### What (More) Can We Do?

Recommendation engines are powerful. Development and deployment requires
thought to ensure they are helpful, not harmful. Throughout this
chapter, we’ve outlined strategies for *developers* to reduce the
harmful effects of filter bubbles and echo chambers, bias, attacks, and
malicious gaming — but there is more we can do.

As *users* of recommenders and *consumers* of recommendations, we can
challenge ourselves to overcome confirmation bias and consciously adopt
a more balanced "news diet"; we can decide to read across partisan
lines. We can use incognito browser windows and search engines like
duckduckgo.com that do not store personal information and do not track
users. As we interact with content on media platforms, we can report
fraud and abuse to companies, to help reduce malicious activity on their
sites and products (and contribute to public pressure, in case companies
fail to act on this information).

Finally, we can take part in the public debate on what is and what isn’t
appropriate content, sharing our perspectives on what is plainly a
complex issue that can only be addressed through collective effort.
