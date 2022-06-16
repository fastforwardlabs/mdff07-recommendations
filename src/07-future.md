## 8. The Future of Recommendations

Semantic recommendation systems offer us the ability to produce highly
influential results with greater precision than many of the current “do
what’s been most popular” personalization tools underlying many apps and
search engines. The growth of these emerging techniques still faces
several limitations in the immediate future, but once these limitations
are overcome, the tools will also lead to spectacular and nonintuitive
new capabilities.

We want to clarify that a recommendation is only a prediction (built
upon past feedback), serving as a proxy for what a user might actually
want; we (as the data scientists behind the recommender) hope the
prediction relates to that user’s preferences closely enough within a
certain window of time as to be useful. As user interfaces improve and
collect more precise user preference information, we expect model
development to improve as well. Because the design of nonintrusive and
easy-to-use interfaces can be very challenging, we expect that the
adoption of some advanced recommendation techniques may hinge on users'
comfort with sharing more personal requests and specific preferences
with the systems. For example, it may be easier to tell even a friend
(let alone an app) that you’re looking for a story “like *Harry Potter*”
than to spell out that you want to read something featuring centaurs,
dragons, mass murderers, and a coming of age love story. Further, most
people simply aren’t used to being able to ask for more from search and
recommendation systems. Most of us take it for granted that website and
app search suggestions are regularly inaccurate, to the point where we
are offered hundreds (if not thousands) of options to scroll through on
a results page. It’s also quite common for people to simply feel that
they won’t know specifically what they are looking for until they see
it. However, the payoff for us (as users) in opening up with greater
specificity about what it is we are really looking for will be measured
in both immense time savings and much greater user satisfaction.

We do see some progress being made in overcoming this tailored data
collection challenge, as we’re seeing more (and better) integration of
lifestyle aids in our consumer experiences. For many, the debate over
how invasive such technologies are now and how much data should be
permitted to be collected on our lives is quite active, and (as noted in
[Ethical Considerations](#7.-ethical-considerations)) the imbalance in personalization for the "haves" vs.
"the have-nots" will continue to be a substantive concern. However,
these data-collection devices are becoming more and more welcomed and
integrated into our lives, in forms such as mobile phones with GPS
tracking, Fitbit-like lifestyle trackers, and "personal assistants" like
Alexa, Siri, and Cortana. All of these devices facilitate the gathering
of data from an increasingly broader range of user actions, which will
be key to the growth of more capable and more personalized recommenders.

Similar to the challenges facing many other emerging machine learning
capabilities, such as those for video analysis in fields like medical
robotics, we see that the lack of topical labeled training data can
really limit effective recommendation algorithm development. Even within
available scored datasets, the information may not be as valuable as we
would hope in developing accurate recommendation models. For example, in
preparing for this report and building the accompanying prototype, we
found many of the datasets we explored used 5-star ranking systems, but
the vast majority of the user scores were simply 1, 4, and 5 stars. So,
although technically the datasets had a large number of ratings, we
estimate that much of the true sentiment of the users is buried in other
characteristics of the works which were not measured in the general
scale. Netflix analysts/engineers may have noted a similar feature,
leading to their switch in spring 2017 to a thumbs-up/thumbs-down
scoring system.

![Figure 8.1](figures/08-02.png)
*Figure 8.1 Multimodal embeddings could allow us to recommend books based on a user’s music taste.* 

Data limitations notwithstanding, we’re looking forward to growth and
development in the neural network space applicable to training
multimodal embeddings. We expect better understanding of embeddings will
help spur the growth of recommendation systems and, in turn, new and
exciting recommendation capabilities. For example, clearer
understandings of bimodal and multimodal embeddings could lead to
recommendations of music or other art forms based on a user’s book
preferences (and vice versa). We may even see personal action
recommendations, such as receiving a suggestion to eat before you even
realize you’re a bit peckish, a water heater warming water in the tank
in anticipation of you wanting to take a bath, or a coffee maker brewing
a pot of coffee in anticipation of when friends at a dinner party may
want a cup. A specific area we’re watching is the development of
attention models, which we feel will support increasing interpretability
of neural network systems and further benefit understanding of
embeddings — how seemingly unrelated objects and actions are each
represented in a network.

We’re also watching the growth of video analytics capabilities, and how
the media industry addresses expected interest with tailored content
generation. We know that developers across industries are aware of the
data availability challenge and are working to grow larger and more
complete datasets, such as more completely labeled videos with new tags
and features, as well as joined datasets. This growth will help drive
neural network advancement, which will in turn help boost the low
signal-to-noise ratio in recommendation offerings. However, we also
recognize that the media industry in particular may shortsightedly and
excessively buy in to recommending content tailored solely to user
preferences (we’re not fans; see the [Filter Bubbles and Echo Chambers](#filter-bubbles-and-echo-chambers)). While there are
interesting problems in the tailored content space, too much emphasis
may delay the advent of truly revolutionary (recommendations-based)
capabilities, which will change the ways we interact with the world and
connect our ideas and actions.

Finally, in terms of raw processing capability, by summer 2018 we’re
expecting to see new hardware become available with which data
scientists will be better able to train larger models designed to
uncover semantic relevancy. In particular, the increasing integration of
ASICs (application-specific integrated circuits) into chipsets, as with
the NVIDIA Volta, is providing a boost to machine learning processing
capabilities. The growing competition in this space between NVIDIA,
Intel (currently developing its Nervana chipset tailored for ML), and
other hybrid and ML-focused processing units will only serve to deliver
greater capability at better prices going forward.

### Customers Who Haven’t Read Kafka Also Like

![](figures/08-01.png)

*A short story written by Kent Szlauderbach,^[http://kentszlauderbach.com] inspired by Franz
Kafka’s parable "An Imperial Message".^[https://www.kafka-online.info/an-imperial-message.html]*

Given that Kafka’s famous parable, “An Imperial Message,” never
happened, neither would this parable, as our model suggests, though they
are very similar. Say the most powerful computer in the nation sends a
message, in a fatal error, containing the story’s true meaning to you, a
modest user, a forensic trace represented by mere underscores bookended
by two periods, a crushed smiley at the bottom of the remotest silo in
the most isolated piece of crumbling land that can still be accessed
from the office that houses the tower, which is gleaming and made all of
windows and plants, as you can see. To its subjects the office suits the
computer’s good policy, clear thinking, and calm understanding—unlike
yours, the message’s subject. We all feel this way now and then. Better
for you, the subject, that the computer’s message, its production of
meaning after reading the famous parable, which the subject has never
read, according to the model, urgently needs to be delivered to this
subject alone, which is so rare as to be impossible, we modeled, we
thought, given that the computer was not designed to send messages so…
personal, so clearly made out to a reader. What it says is not our
business; the successful delivery of this message is: so the computer
called a little messenger over, said good boy, because you can imagine a
black dog better than a black box, and began to whisper the message that
you’ve been wanting to hear, something so urgent, perhaps for the
perfect product—no, service—the one that may have been your very idea,
the one you need now more than ever. What could it say? The computer, in
its towering form and brilliant interface, standing at the center of its
office made of windows and plants, made a command to the messenger,
which had never happened before and should never have happened, we
thought, we modeled, but still, the computer set delivery to the subject
who shall be delivered of the need to hear that message, that message
that the computer had delivered to a messenger, who we can only say is a
black box, a negative, nothing we can say, that is neither black nor a
box. The dog carried its black box in its mouth along the path below but
still in range of sight of the office made of windows and plants, then
down through an office park, where in its visible spectrum it could see
the thousands of others like him itself who had come to the office of
windows and plants to witness the fatal error of the computer, dogs
whose messages were totally unlike yours, mutually unintelligible as
they were, but had gathered there anyway to witness the message as if it
were the final move in a game, this final stroke of the task, but
instead demanded that this message go out to a single subject in the
remotest place in the country’s network, a black box through a black
box, first through the innermost room of an office park that was neither
black nor a box, to the inner city whose office park buildings were
themselves black boxes atop black boxes, but again, were neither black
nor boxes, but blue and reflective, where our messenger looked—yes this
dog vessel could look and learn itself, as we modeled, we thought. Even
if these buildings, as they appear to the messenger, are not black nor
boxes, they give the messenger, looking at itself, the feeling of a
black box, of unknowing, since he it has no idea what is contained in
this message, or how he’ll it will ever get to you and if he it ever
will, but yet he it presses on as the computer commanded, for even if
the messenger reached the edge of the city, it would still have to make
it through the suburb offices of black boxes, buildings that had grown
so tall and wide as to become indistinguishable from the inner city.
Still not lost, it would take even more power and time to deliver the
message to its recipient. We have worked very hard to understand the
impossibility of this. We’ve thought about how one could be receiving
these kinds of messages before, but only in their lack of possibility.
Pure imagination. This would never happen. And you think, waiting for
the messenger who had been given the rarest possible message ever
created: What would it say? What is the meaning of the famous parable?
You’ve been selected to read this because you are the last of a group of
people who speak this language, perhaps, or only, the last who has not
read this parable. This language is English: the subject’s recommended
language, not the original. Do you know it? Click yes or no, you think,
we thought, when it would arrive, when you would hear the ring on your
phone. Thank you. That’s what it might sound like, a sound you’d never
imagine you would hear. We’re not in the business of recommending
content, it might say. We recommend memories, that of the computer
sending you such a perfect message on your birthday. Is that all it
says? Was that the day? But still the message is far away, still walking
through the office park, even as you wait and look vaguely out the
window toward the office made of windows and plants.
