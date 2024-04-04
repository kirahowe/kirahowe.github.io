Title: The Current State of ML in Clojure
Date: 2024-04-04
Tags: clojure, machine learning, scicloj

I had a really enlightening talk with [Daniel Slutsky](https://www.linkedin.com/in/daniel-slutsky-42122b4/) this week (who is an exceptional data scientist, software engineer, and community oragnizer I highly recommended meeting if you haven't already) about the current state of the machine learning landscape in Clojure. This post is my attempt to distill it into a summary for the community's benefit, so more people can understand where things are at and what the active developers in this space are working on.

It's no secret I love Clojure and especially working with data in Clojure, but it's fair to say that the Clojure for data science ecosystem is not anywhere near as easy to use or understand as reasonable potential users might expect. This is the main problem I'm focusing on this year, and there is significant effort being put into refining our tools to make them more accessible to a wider audience.

There are already people doing "real" machine learning work in Clojure, though, and below is an overview of what the current state of our libraries and tools are in that area, as of April 2024.

## Java ML Libraries

There are two (sort of three) popular Java libraries that implement many of the main algorithms and tools used in machine learning (e.g. classification, regression, clustering, model development, etc.): [Tribuo](https://tribuo.org/) and [Smile](https://haifengl.github.io/). We count Smile as two libraries because Smile 2.x is LGPL-licensed, and Smile 3.x is GPL-licensed, which poses some potential conflicts for some end users. The community consensus is converging around moving away from Smile due to the GPL-relicensing issue, focusing instead on Tribuo and hand-rolled solutions.

## Clojure wrappers

There are two main "families" of libraries that wrap these Java ML libraries in Clojure.

[Fastmath](https://github.com/generateme/fastmath) includes statistical as well as machine learning tools for Clojure. Fastmath 2.4.0+ depends on Smile 2, and the forthcoming fastmath 3.x will have no Smile dependency at all. The clustering functionality in fastmath 2.x that depended on smile has been moved to the [fastmath-clustering](https://github.com/generateme/fastmath-clustering) library, which will have a Smile 2.x dependency going forward. There is a strong preference in the community to avoid introducing GPL-licensed libraries into the ecosystem.

[scicloj.ml.smile](https://github.com/scicloj/scicloj.ml.smile), as you might expect, wraps Smile. It wraps much more of Smile than fastmath did (does). [scicloj.ml.tribuo](https://github.com/scicloj/scicloj.ml.tribuo) wraps the Tribuo library and is likely to become the main source of ML algorithms for the ecosystem.

[tech.ml.dataset](https://github.com/techascent/tech.ml.dataset) (the core dataframe/dataset library underlying tablecloth) also [incorporates some of the functionality of tribuo](https://github.com/techascent/tech.ml.dataset/blob/master/src/tech/v3/libs/tribuo.clj), with the API centered around individual datasets. Which leads me to the next group of libraries. 

It's worth also mentioning [tech.ml](https://github.com/techascent/tech.ml), which implements some machine learning tools, but has been deprecated in favour of the various libraries discussed above.

## Clojure ML Pipelines

[Metamorph](https://github.com/scicloj/metamorph) is a library that implements a function composition mechanism for composing ML pipelines. It arises from the common ML practice of repeatedly running the same set of functions with varied parameters. You might, for example, try many different test/train splits to see how that affects your results, or fit the same data using many different algorithms, or try training your model using different sets of features. This leads to an explosion of pipeline permuatations, so it's useful to have machinery to encapsulate the variable components of your ML pipeline into a single function. This is where metamorph.ml comes in. 

[Metamorph.ml](https://github.com/scicloj/metamorph.ml) is based on this concept of meta-functions and pipelines. It is currently the central library for orchestrating ML pipelines in Clojure. It's API is still evolving, and there are currently [many ways (10+)](https://clojurians.zulipchat.com/#narrow/stream/321125-noj-dev/topic/ols.20interaction.20tutorial/near/422408507) to acheive the same outcomes, with no clear community consensus yet on a preferred approach.

## Collections/Frameworks

The community is well aware that it is difficult to know where to get started and several efforts have been made in an attempt to make the path more clear for people who want tools that Just Work. [scicloj.ml](https://github.com/scicloj/scicloj.ml) is one such project. It's a collection of libraries (mostly the ones mentioned above) with some lightweight wrappers and efforts in creating documentation.

The community is heading toward deprecating this library, though, in favour of [noj](https://github.com/scicloj/noj), which we are hoping to stabilize in the near future. The goal is to have a single entry-point into the Clojure data science stack with all the tools one would need to do work with data consolidated in one place, with seamless interoperability akin to R's tidyverse of libraries.
## More updates

These discussions all happen in the open, on the [Clojurian's Zulip instance](https://clojurians.zulipchat.com), which has become the main gathering place of the Clojure-for-data-science community. The `#data-science` and `#noj-dev` streams are the most active on these topics at the time of this writing. You can follow along with developments in the trenches over there, or follow the key libraries on github for updates ([scicloj.smile.ml](https://github.com/scicloj/scicloj.ml.smile), [scicloj.ml.tribuo](https://github.com/scicloj/scicloj.ml.tribuo), [metamorph.ml](https://github.com/scicloj/metamorph.ml), [noj](https://github.com/scicloj/noj)). I will also post periodic updates here and all the other corners of the internet where I lurk.