The matrix math of constrained markets
######################################

:date: 2016-09-18 22:30
:category: consequential
:slug: matrix-math-constrained-markets
:summary: Translating abstract ideas into actual numbers can be a bit challenging
:status: draft

Constrained markets
===================

There is a `graphical summary <http://www.ecoinvent.org/support/faqs/methodology-of-ecoinvent-3/what-is-a-constrained-market-how-is-it-different-from-the-normal-market-how-does-it-behave-during-the-linking.html>`__ of a constrained market on the ecoinvent website. Constrained markets are simply that - a market for a product or service that can't be produced more than it currently is. Consuming more of this product or service will cause other sectors of the economy to consumer less. These constraints are only considered in the consequential system model.

Implementation in EcoSpold 2
----------------------------

Market constraints are specified in the EcoSpold 2 data format as a conditional exchange. This exchange has the following characteristics:

* It is an input to a market activity
* It has the type ``byproduct``
* It has a negative amount
* It has a direct link (also known as an "activity link")

Direct links are not resolved by the normal linking algorithm (on which more in a future blog post), but instead give a direct indication of the activity that produces the relevant input flow.

Case study: Cryolite production
===============================

`Cryolite <https://en.wikipedia.org/wiki/Cryolite>`__ is used in aluminium production. It can be produced from `fluosilicic acid <https://en.wikipedia.org/wiki/Hexafluorosilicic_acid>`__ or `hydrogen flouride <https://en.wikipedia.org/wiki/Hydrogen_fluoride>`__. In ecoinvent, fluosilicic acid production is considered completely constrained.

In our case study, we will consider the following activities:

* Market for fluosilicic acid (hereafter Market Ⓕ), with reference product fluosilicic acid (hereafter Ⓕ). This market has a conditional exchange of fluosilicic acid with a direct link to Cryolite Ⓕ.
* Cryolite production from fluosilicic acid (hereafter Cryolite Ⓕ), with reference product cryolite from fluosilicic acid (hereafter Cryolite Ⓕ). This activity consumes Ⓕ.
* Cryolite production from hydrogen flouride (hereafter Cryolite Ⓗ), with reference product cryolite from hydrogen flouride (hereafter Cryolite Ⓗ).
* Market for cryolite (hereafter Market Cryolite), with reference product cryolite (hereafter Cryolite). This activity aggregates production of cryolite, both Cryolite Ⓕ and Cryolite Ⓗ, in proportion with their respective production volumes. For simplicity in this case study, we assume both activities produce equally.

Matrix math in attributional system models
------------------------------------------

In attributional system models, conditional exchanges are ignored. Therefore, our matrix can be easily constructed. Note that, instead of actual numbers, we use simple numbers:

.. figure:: /images/constrained-attributional.png
    :alt: Matrix with no costrained exchanges.
    :align: center

    Figure 1: Techosphere matrix for cryolite production in attributional system models.

Matrix math in consequential system models
------------------------------------------

The matrix looks a bit different is we don't get to ignore the conditional exchange. No build up of suspense, here it is:

.. figure:: /images/constrained-consequential.png
    :alt: Matrix with costrained exchanges.
    :align: center

    Figure 2: Techosphere matrix for cryolite production in consequential system models.

The algorithm to build this matrix is a bit tricky. Our conditional exchange is a direct link for the flow "fluosilicic acid" (Ⓕ) from the activity Cryolite Ⓕ. But Cryolite Ⓕ doesn't produce Ⓕ! Instead, our direct link is changed to the flow Cryolite Ⓕ (which is now also not produced by the activity Cryolite Ⓕ...). The amount in the matrix is the amount of the direct link - there is no trickery due to the sign of the exchange.

And what is happening with Cryolite Ⓕ, anyway? It looks, and is, totally different. The most striking difference is that the reference product of the activity "cryolite production from fluosilicic acid" is fluosilicic acid! I thought that fluosilicic acid was an input, but as my teenage daughter likes to remind me, I grew up in the "Steinzeit." As the activity Cryolite Ⓕ no longer produces Cryolite Ⓕ, it can't contribute to Market Cryolite; but we do now have an *input* of Cryolite from Market Cryolite. Strange times, etc.

Let's look at the math for some demand vectors. What happens when we want some fluosilicic acid?

.. code-block:: python

    In [1]: import numpy as np

    In [2]: A = np.array((( 1, -1,  0,  0),
       ...:               (-1,  0,  0,  0),
       ...:               ( 0,  0,  1, -1),
       ...:               ( 0,  1,  0,  1)))

    In [3]: b = np.array((1, 0, 0, 0))

    In [4]: np.linalg.solve(A, b)
    Out[4]: array([ 0., -1.,  1.,  1.])

So, requesting one unit of Ⓕ causes no change in the activity producing Ⓕ, but does decrease the activity producing Cryolite Ⓕ, and increase the activity producing Cryolite Ⓗ. And this is exactly what we want - our initial supposition is that the market for Ⓕ is constrained and can't produce more, so that consuming Ⓕ will reduce cryolite production from Ⓕ, which will in turn require increased production of cryolite from Ⓗ, as the total demand for cryolite didn't change.

Some might wonder about the amount of the activity Market Cryolite. Markets often include transportation, which will meant that there is some additional environmental burden from transport even though there is no net different in cryolite production. I am not sure if this effect is intended or not.

What about a demand of Cryolite Ⓕ? Well, this would require more Ⓕ, but we know that this isn't possible. Instead, we would expect that asking for Cryolite Ⓕ would just get us Cryolite Ⓗ, and this is indeed what happens:

.. code-block:: python

    In [5]: b = np.array((0, 1, 0, 0))

    In [6]: np.linalg.solve(A, b)
    Out[6]: array([-1., -1.,  1.,  1.])

Another interesting result that requires some unpacking. In this case, it may be difficult to understand why there is -1 of the activity Market Ⓕ - it certainly took me some time. The environmental damage caused by an increase in demand for Cryolite Ⓕ is actually the *marginal difference* in environmental performance of the production of Cryolite Ⓗ *minus* the production of Cryolite Ⓕ (each including their respective supply chains, e.g. the market for fluosilicic acid). You will have to decide for yourself if this is in concordance with your mental model of how this is supposed to work; but it is what the math says.

Finally, let's make sure that drawing from the market for Cryolite only gets us Cryolite Ⓗ:

.. code-block:: python

    In [7]: b = np.array((0, 0, 0, 1))

    In [8]: np.linalg.solve(A, b)
    Out[8]: array([ 0., 0.,  1.,  1.])
