Title: OSS Updates May and June 2024
Date: 2024-06-30
Tags: open source, clojure, clojurists together, oss updates

This is a summary of the open source work I've spent my time on throughout May and June, 2024. There were lots of small bug fixes and reports, driven by work on the Clojure Data Cookbook. This work was also the impetus for my initial release of [`tcutils`](https://github.com/scicloj/tcutils), a library of utility functions for working with tablecloth datasets. I also had the wonderful opportunity to attend PyData London in June and found it really insightful and inspiring. Read on for more details.

## Sponsors
This work is made possible by the generous ongoing support of my sponsors. I appreciate all of the support the community has given to my work and would like to give a special thanks to Clojurists Together and Nubank for providing me with lucrative enough grants that I can reduce my client work significantly and afford to spend more time on these projects.

If you find my work valuable, please share it with others and consider supporting it financially. There are details about how to do that on my [GitHub sponsors page](https://github.com/sponsors/kiramclean). On to the updates!

## Ecosystem issue reports and bug fixes
Working on the cookbook these last couple of months turned up a few small issues in ecosystem libraries. The other developers of Clojure's data science tools are such a pleasure to work with, it's so rare and nice to have a distributed team of people capable of getting cool things built asynchronously. Here are some details of a few particular issues that came up:
- Small problem loading .xls/.xlsx files as datasets if they had a number as a column name: [discussed here](https://clojurians.zulipchat.com/#narrow/stream/236259-tech.2Eml.2Edataset.2Edev/topic/xlsx.20column.20parsing/near/437313810), [reported here](https://github.com/techascent/tech.ml.dataset/issues/408), and graciously [fixed by Chris Nuernberger](https://github.com/techascent/tech.ml.dataset/commit/24c0e646f289210aa95c1ac9998cb2ddd5c9f836). 
- Unexpected behaviour when comparing certain numeric types in `dtype-next`: [discussed here](https://clojurians.zulipchat.com/#narrow/stream/236259-tech.2Eml.2Edataset.2Edev/topic/numeric.20datatypes/near/438617694%5D(https://clojurians.zulipchat.com/%23narrow/stream/236259-tech.2Eml.2Edataset.2Edev/topic/numeric.20datatypes/near/438617694), [reported here](https://github.com/cnuernber/dtype-next/issues/99), and again [fixed by Chris](https://github.com/cnuernber/dtype-next/commit/563fe9c13797feb206391cd951655942e3e6cf0f). This one sadly had some unintended consequences that [generateme found and reported here](https://github.com/cnuernber/dtype-next/issues/103).
- [Many improvements to Clay](https://github.com/scicloj/clay/blob/b299d060c3edbce789a55fee3efedce42fbd2ab4/CHANGELOG.md) by Daniel Slutsky, especially a couple of ones that make the quarto publications it produces much nicer: [fixing too-wide tables in quarto pages](https://github.com/scicloj/clay/pull/102) and [supporting limiting the number of table rows that get displayed](https://clojurians.zulipchat.com/#narrow/stream/321125-noj-dev/topic/kindly.20options/near/440663980).
- Some good discussions about how best to incorporate the myriad of dependencies required to use Java machine learning libraries in Clojure libs, including sorting out what to do about [transitive dependencies in our tribuo wrapper](https://github.com/scicloj/scicloj.ml.tribuo/issues/1), led by Carsten Behring.

## Initial release of tcutils
In my explorations of other languages' tools for working data I often come across nice utility functions that are super simple but have a big impact on the ergonomics of using the tools. I wanted to start bringing some of these convenience utilities to Clojure, so for now I'm putting them in [`tcutils`](https://github.com/scicloj/tcutils). So far only a handful of helpers are implemented (`lag`, `lead`, `cumsum`, and `clean-column-names`). The goal is to eventually fill out more utilities that save people from having to dig into the documentation of half a dozen different libraries to figure out how to implement things like these. The goal is not to achieve feature parity or to exactly copy similar libraries, like pandas or dplyr, but rather to take inspiration from them and make our tools easier to use for people who are used to these conveniences.

## Progress on Clojure Data Cookbook
I spent a lot of time on the Clojure Data Cookbook over these last two months. Notable progress includes:
- The introductory chapters bear some resemblance now to the final form they'll take.
- The overall structure of the book is much more clear now.
- I started the example analysis that will serve as the high-level introductory section of the book.
- The publishing and deployment process is finally working.

It's still very much in progress, but in the interest of transparency the work-in-progress version is [available online now](https://github.com/scicloj/clojure-data-cookbook). It will continue to evolve and change as I fill out more and more of the chapters, but there's enough of it available now to hopefully give a sense of the style and tone I'm going for. I also finally have the publishing workflow set up and it's generating a nice-looking Quarto book, thanks to all of Daniel Slutsky's amazing work on Clay and Quarto integration recently.

## Progress on high-level goals
The high-level goal of my work in general remains to steward Clojure's data science ecosystem to a state of maturity and flourishing so that data practitioners can use it to get real work done. Toward this end, I set up a [project board](https://github.com/users/kiramclean/projects/4) to track progress toward what I see as the main components of this project. 

Over the last couple of months, beginning with a prototype demoed at my [London Clojurians talk in April](https://www.youtube.com/watch?v=eUFf3-og_-Y), Daniel Slutsky has made tremendous progress on our goal of implementing a grammar of graphics in Clojure in the new [hanamicloth library](https://github.com/scicloj/hanamicloth). The near-term goal is to stabilize the API of this library enough that it can be used to provide a user-friendly way to accomplish all of the simple data visualization tasks that are currently possible with our other tools. The long term goal is to take the lessons we learn from this library and build a JVM-only grammar of graphics library for doing data visualization "right" in Clojure.

The development and surrounding discussions of hanamicloth have also made me realize it would be useful to write an overview of the current state of dataviz options for Clojure and why we're working on building something new. That's on my list for the coming months, but lower priority than actual development work.

## Impressions from PyData London
I got to attend PyData London this year thanks to a client of mine who was sponsoring the conference. I learned a lot and found the talks very interesting. My overall impression is that data science is maturing as a discipline, with more polished methods and robust theory backing up different approaches to data-related problems. With this maturation, though, comes higher expectations for production-ready, professional quality results. Most of the talks focused on high-level concerns like observability, scalability, and long-term stewardship of large open-source projects.

There are a lot of reasons why Python is just not ideal for building highly available, high-performance systems, and I really believe this is a good time to be building alternative tools for data science. Python is obviously entrenched as the current default language for working with data, but it is difficult and slow to write code that can take full advantage of modern hardware (because of the infamous global interpreter lock, reference counting, slow I/O, among other reasons). And to be fair, the Python community knows this. It's why virtually all of the libraries that do the heavy lifting for data science in Python are actually implemented in C (numpy,  pandas) or Rust (Polars, Pydantic), or are wrappers around C++ (PyTorch, TensorFlow, matplotlib) or Java (PySpark, Pydoop, confluent-kafka) libraries. 

I think this provides a lot of insights into what data practitioners want. It's clear that users _want_ approachable, simple, human-readable interfaces for all of these tools, and that any new tool needs to interoperate with the rest of the ones currently in use. People are also [tired of churn](https://news.ycombinator.com/item?id=40815097) and are craving stability. I think Clojure has a lot to offer in all of these areas and is well placed to become more widely adopted for data science.

## Ongoing work
My focus over the next two months will remain on the cookbook. My main goal is to finish the introductory chapter with the housing price analysis and to continue putting together the data import section with instructions and examples for all file formats that can reasonably be supported easily at this time.

I'll continue to support and contribute to all of the ecosystem libraries I come across in my writings and analysis work in hopes of smoothing out all the rough edges I find.

Thanks for reading. I always love hearing from people who are interested in any of the things I'm working on. If that's you, don't hesitate to be in touch :)







