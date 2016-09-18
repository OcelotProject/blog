An outside opinion
##################

:date: 2016-04-12 21:00
:category: blog
:slug: an-outside-opinion
:summary: Disqus comments which will be removed from the front page

I have removed comments from the front page, because for silly technical reasons that got attached to whatever the latest blog post, making for a disjointed conversation. However, I didn't want to lose what had already been said, so here is are the comments so far between Brandon Kuczenski and the Ocelot project team.

Brandon Kuczenski
=================

It seems to me that if you are already starting with ecospold v2 and ending with ecospold v2, you have already foreclosed a great many design decisions / discussions.

Chris Mutel
===========

One some level, you are completely correct - ecospold 2 builds in a certain set of assumptions, both explicit and implicit, and these assumptions impose limits. I don't think any of the actual code aside from IO will include anything ecospold 2-specific, but there will be an internal data format which will mirror ecospold 2 in many ways.

One of the explicit principles in the Ocelot grant was to "not let the best be the enemy of the good", and we think it is better to get at least some system model principles in place as tested, open source computer code, than to wait for the perfect data format. There are also some practical reasons for choosing ecospold 2 - you can add arbitrary properties, so it is quite flexible, and it supports parameterization. Most importantly, ecoinvent is the only LCI database that I know of that provides "raw" master data where no system models have been applied at all, and ecoinvent is provided in ecospold 2.

Certainly our work will only be the first step on a longer conversation with the community, and I would be quite curious to hear about specific examples of what you think would be ignored or overseen.

Brandon Kuczenski
=================

In my opinion the lack of a "flow" as a standalone entity is a serious shortcoming in a linking algorithm, since flows are the things that link. But my bigger concern is the philosophical one that is implied in squeezing everything into a data format that is so very expensive to work with- and in doing so a priori, before the design principles of the task are even established (or at least known to others).

I consider "arbitrary extensibility" to be a weakness of ecospold, not a strength. I've long thought it a folly for ecoinvent to try to fold all their complex data (transformations! LCIA methods! markets! inheritance! uncertainty! parameters!) into a single data object that nobody but ecoinvent knows how to author or read. ILCD, verbose as it is, at least has some concept of separation of concerns. (ILCD also does not do many of the things ecoinvent requires-) I have a genuine fear, especially reading your response, that ocelot is just a trip further down a rabbit hole that is already (from the outside) too deep to enter.

It's also not true that ecoinvent provides "raw" master data to its users! The `file list <https://v32.ecoquery.ecoinvent.org/File/Files>`__ only has linked system models. They're also not on `nexus.openlca.org <http://nexus.openlca.org/>`__. It's true that I can *view* the unlinked data on the website, one data set at a time, via search, but that's not the same thing.

n.b. US LCI, on the other hand, is made up of "raw" unallocated multioutput processes.

Anyway, forgetting the input format- why would the output format be ecospold 2?
There are very few people who would consider a set of 11,000 XML files to be a useful thing they would want to generate. If the object of the algorithm is to create a square technology matrix, why make the output so very, very far from that? Why not deliver, say, a technology matrix? It would be 0.1% the size, it would be easy to interpret, it would be software independent.

A bit later
-----------

All this is just to say, it sounds like ocelot is mainly going to be useful for internal ecoinvent personnel. Which is not a bad thing! I think it's great that ecoinvent will be able to do new and exciting things with system models. It's just- a smaller community than I thought you meant at first... and to say it in a nasty way, because I have a chip on my shoulder.

Chris Mutel
===========

Brandon makes some interesting points, and I have invited him to make a guest blog post about what he imagines a project like Ocelot could become. So I postpone discussion of broader theory questions for now. However, I do have to disagree with a few technical points.

First, `ecospold 2 <http://www.ecoinvent.org/data-provider/data-provider-toolkit/ecospold2/ecospold2.html)>`__ definitely has "flows" distinct from activities. For example, the complete list of flows in ecoinvent 3.2 is included in the files Content/MasterData/IntermediateExchanges.xml and Content/MasterData/ElementaryExchanges.xml, both `available here <http://www.ecoinvent.org/files/documentation_on_ecospold2_format.zip)>`__.

Next, while it is true that unallocated data is not available on nexus.openlca.org (an unaffiliated website), you can get all the master data by just asking the ecoinvent team. They don't bite.

It is also a bit of an exaggeration to state that "nobody but ecoinvent knows how to author or read" ecospold 2. For example, you can find open source importers and converters for `Java <https://github.com/GreenDelta/olca-converter)>`__ and `Python <https://bitbucket.org/cmutel/brightway2-io)>`__. In my personal opinion, Ecospold 2 is sometimes a pain, but it is not such a dramatic pain. For what it's worth, I have given multiple presentations about how I really want the `JSON linked data schema <https://github.com/GreenDelta/olca-schema)>`__ to take over the world.

It is worth briefly mentioning that the US LCI is not *raw* data - it is *linked* data, resolved in time and space. System models are much more than just allocation.

Finally, here is a kitten:

.. figure:: images/kitten.jpg
    :align: center
    :alt: The internet is 90% cats

Guillaume Bourgault
===================

Chris has already said most of what I wanted to say, I'll just add a few words.

The schema for the master data files is available for these files, upon request. I'm happy to answer all questions about these files. The schema for the dataset ecospold2 format is also available.

About the output format: all the existing software who deal with the ecospold2 format have the capacity to take those files and transform them into a matrix. We also have the internal tools to transform ecospold2 files to a matrix representation, both in Python scipy.sparse matrices and in machine-readable txt format that can be reconstituted into a matrix easily.
We think it is important to use ecospold2 format as an output because it carries with it all the relevant meta information necessary to understand the datasets. We plan to add comments, where necessary, along the calculation chain, when it will be judged necessary (for example, allocation, loss, market group replacement, geography linking, etc.)
That being said, more than one output is for from impossible, and since we already have some internal tools, we could easily include them as some "end-of-pipe" scripts for user comfort. We also care about the accessibility of results. We are happy that you have suggestions about this aspect!

If I can provide the perspective of somebody answering support questions from the entire user base of ecoinvent. Brandon is right to point out that the ocelot project is "directly useful" to a very small proportion of users. The vast majority of our users do not care much about what we are discussing here. And if I can speak candidly, I am sometimes appalled by questions showing lack of understanding of vary basic LCA concepts from our users. At the other side of the spectrum, there is only a handful of people in the world who have the resources, interest and skills to actively contribute to the project. However, the entirety of the users will benefit indirectly from the fruits of this project, for example by the reassurance that system model assumptions have been thoroughly tested.

The purpose of the development blog is to allow open discussion about the kind of concerns Brandon has. The project is young enough to be steer in many directions. As somebody who usually focuses on computational aspects that most member of the community don't even want to think about, I'm quite happy to see Brandon's interest in the project. We obviously have to find a balance between the ideals we hold and what we can reasonably achieve with the resources we have for this project. As Chris pointed, not letting the best be the enemy of the good is something to keep in mind. The release of version 3.0 has been delayed because this principle was forgotten, and we are dealing with many legacy problems caused by this lack of foresight. Ocelot is the best initiative we have had so far to fix these legacy problems.
