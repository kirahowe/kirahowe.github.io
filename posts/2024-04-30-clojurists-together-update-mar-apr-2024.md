Title: OSS Updates March and April 2024
Date: 2024-04-30
Tags: open source, clojure, clojurists together, oss updates

This is a summary of the open source work I've spent my time on throughout March and April, 2024. Overall it was a really insightful couple of months for me, with lots of productive discussions and meetings happening among key contributors to Clojure's data science ecosystem and great progress toward some of our most ambitious goals.

## Sponsors
This work is made possible by the generous ongoing support of my sponsors. I appreciate all of the support the community has given to my work and would like to give a special thanks to Clojurists Together and Nubank for providing me with lucrative enough grants that I can reduce my client work significantly and afford to spend more time on these projects.

If you find my work valuable, please share it with others and consider supporting it financially. There are details about how to do that on my [GitHub sponsors page](https://github.com/sponsors/kiramclean/). On to the updates!

## Grammar of graphics in Clojure
With help from Daniel Slutsky and others in the community, I started some concrete work on implementing a grammar of graphics in Clojure. I'm convinced this is the correct long-term solution for dataviz in Clojure, but it is a big project that will take time, including a lot of [hammock time](https://www.youtube.com/watch?v=f84n5oFoZBc). It's still useful to play around with proofs of concept whilst thinking through problems, though, and in the interest of transparency I'm making all of [those experiments public](https://github.com/kiramclean/ggclj).

The discussions around this development are all also happening in public. There were two visual tools meetups focused on this over the last two months ([link 1](https://www.youtube.com/watch?v=MxjzaOtcdcY), [link 2](https://www.youtube.com/watch?v=d3iRGmbJmes)). And at the London Clojurians talk I just gave today I demonstrated an example of one proposed implementation of a [grammar-of-graphics-like API](https://github.com/kiramclean/workshops/blob/main/london_clojurians_april_2024/src/utils/hana.clj) on top of hanami implemented by Daniel.

There are more meetups planned for the coming months and work in this area for the foreseeable future will look like researching and understanding the fundamentals of the grammar of graphics in order to design a simple implementation in Clojure.

## Clojure's ML and statistics tools
I spent a lot of time these last couple of months documenting and testing out Clojure's current ML tools, leading to many great conversations and one [blog post](https://codewithkira.com/2024-04-04-state-of-clojure-ml.html) that generated many more interesting discussions. The takeaway is that the tools themselves in this area are all quite mature and stable, but there are still ongoing discussions around how to best accommodate the different ways that people want to work with them. The overall goal in this area of my work is to stabilize the solutions so we can start advocating for specific ways of using them.

Below are some key takeaways from my research into all this stuff. Note none of these are my decisions to make alone, but represent my current opinions and what I will be advocating for within the community:
- Smile will be slowly sunsetted from the ecosystem. The switch to GPL licensing was made in bad faith and many of the common models don't work on Apple chips. Given the abundance of suitable alternatives, the easiest option is to move away from depending on it.
- A greater distinction between statistical modelling and machine learning workflows will be helpful. Right now there are many uses of the various models that are available in Clojure, and the wrappers and tools surrounding them are usually designed with a specific type of user in mind. For example machine learning people almost always have separate training and testing datasets, whereas statisticians "train" their models on an entire dataset. The highest-level APIs for these different usages (among others) look quite different, and we would benefit from having APIs that are ergonomic and familiar to our target users of various backgrounds.
- We should agree on standards for accomplishing certain very common and basic tasks and propose a recommended usage for users. For example, there are almost a dozen ways to do linear regression in Clojure and it's not obvious which is "the best" way to someone not deeply familiar with the ecosystem.
- Everything should work with tablecloth datasets and expect them as inputs. This is mostly the case already, but there is still some progress to be made.

## Foundations of Clojure's data science stack
I continue to work on guides and tutorials for the parts of Clojure's data science stack that I feel are ready for prime time, mainly tablecloth and all of the amazing underlying libraries it leverages. Every once in a while this turns up surprises, for example this month I was surprised at how column header processing is handled for nippy files specifically. I also [fixed one bug](https://github.com/scicloj/tablecloth/pull/143) in tablecloth itself, which I discovered in the process of writing a tutorial earlier in March. I have a pile of in-progress guides focusing on some more in-depth topics from developing the London Clojurians talk that I'm going to tidy up and publish in the coming months.

The overarching goal in this area is to create a unified data science stack with libraries for processing, modelling, and visualization that all interoperate seamlessly and work with tablecloth datasets, like the tidyverse in R. Part of achieving that is making sure that tablecloth is rock solid, which just takes a lot of poking and prodding.

## London Clojurians talk
This talk was a big inspiration for diving deep into Clojure's data science ecosystem. I experimented with a ton of different datasets for the workshop and discovered tons of potential areas for future development. Trying to put together a polished data workflow really exposed many of the key areas I think we should be focusing on and gave me a lot of inspiration for future work. I spent a ton of time exploring all of the possible ways to demonstrate a broad sample of data science tools and learned a lot along the way.

The resources from the talk are all available [in this repo](https://github.com/kiramclean/workshops/tree/main/london_clojurians_april_2024) and the video will be posted soon.

## Summary of future work
I mentioned a few areas of focus above, below is a summary of the ongoing work as I see it. A framework for organizing this work is starting to emerge, and I've been thinking about in terms of four key areas:

### Visualisation
- Priority here is to release a stable dataviz API using the tools and wrappers we currently have so that we can start releasing guides and tutorials that follow a consistent style.
- The long-term goal is to develop a robust, flexible, and stable data visualization library in Clojure itself based on the grammar of graphics.

### Machine learning
- Priority is to decide which APIs we will commit to supporting in the long term and stabilize the "glue" libraries that provide the high-level APIs for data-first users.
- Long term goal is to support the full spectrum of libraries and models that are in everyday use by data science professionals.

### Statistics
- Priority is to document the current options for accomplishing basic statistical modelling tasks, including Clojure libraries we do have, Java libs, and Python interop.
- Long term goal is to have tablecloth-compatible stats libraries implemented in pure Clojure.

### Foundations
- Priority is to build a tidyverse for Clojure. This includes battle-testing tablecloth, fully documenting its capabilities, and fixing remaining, small, sharp edges.

## Going forward

My overarching goal (personally) is still to write a canonical resource for working with Clojure's data science stack (the Clojure Data Cookbook), and I'm still working on finding the right balance of documenting "work-in-progress" tools and libraries vs. delaying progress until I feel they are more "ready". Until now I've let the absence of stable or ideal APIs in certain areas hinder development of this book, but I'm starting to feel very confident in my understanding of the current direction of the ecosystem, enough so that I would feel good about releasing something a little bit more formal than a tutorial or guide and recommending usages with the caveat that development is ongoing in some areas. And while it will take a while to get where we want to go, I feel like I can finally see the path to getting there. It just takes a lot of work and lot of collaboration, but with your support we'll make it happen! Thanks for reading.