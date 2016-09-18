A few initial questions and answers
###################################

:date: 2016-03-16 23:00
:category: faq
:slug: initial-faq
:summary: Answers to some basic questions asked after the announcement on the LCA discussion list

There have been a number of good questions asked since we announced the Ocelot project on the `LCA discussion list <https://www.pre-sustainability.com/lca-discussion-list>`__. As most of these questions were asked by private email, we are making an initial FAQ post, to be updated over time.

Is Ocelot LCA software?
=======================

No.

What is the output of a linking algorithm?
==========================================

The input to a linking algorithm is set of datasets that are not yet finalized. For example, dataset A could need peanut butter as an input, and dataset B could produce peanut butter, but there would be no explicit link between A and B. Each transformation function will produce a new version of the set of datasets with one small change. In our example, the output of the transformation function would be a link from process B in process A. The output of a system model as a whole is a set of datasets that are completely linked and which produce a square technosphere matrix, i.e. a set of files you could import into LCA software.

Can you give an actual example of a transformation function in Ocelot?
======================================================================

That will be the next development blog post :)

Will a system model created with ocelot require ocelot to interpret?
====================================================================

No, the outcome of the system model will be a set of `ecospold2 XML files <http://www.ecoinvent.org/data-provider/data-provider-toolkit/ecospold2/ecospold2.html>`__, just like you can download right now for the three different system model versions of ecoinvent. These files will then be imported by other software such as `Brightway2 <https://brightwaylca.org/>`__.
