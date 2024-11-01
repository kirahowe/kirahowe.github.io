Title: OSS Updates September and October 2024
Date: 2024-10-31
Tags: open source, clojure, clojurists together, oss updates

This is a summary of the open source work I spent my time on throughout September and October 2024. This was a very busy period in my personal life and I didn't make much progress on my projects, but I did have more time than usual to think about things, which prompted many further thoughts. Keep reading for details :)

## Sponsors

I always start these posts with a sincere thank you to the generous ongoing support of my sponsors that make this work possible. I can't say how much I appreciate all of the support the community has given to my work and would like to give a special thanks to Clojurists Together and Nubank for providing incredibly generous grants that allowed me reduce my client work significantly and afford to spend more time on projects for the Clojure ecosystem for nearly a year.

If you find my work valuable, please share it with others and consider supporting it financially. There are details about how to do that on my [GitHub sponsors page](https://github.com/sponsors/kirahowe). On to the updates!

## Personal update

I'll save the long version for the end but there is one important personal update that's worth mentioning up front: I go by Kira Howe now. I used be known as Kira McLean, and all of my talks, writing, and commits up to this point use Kira McLean, but I'm still the same person! Just with a new name. I even updated my [GitHub handle](https://github.com/kirahowe), which went remarkably smoothly.

## Conj 2024

The main Clojure-related thing I did during this period was attend the Conj. It's always cool to meet people in person who you've only ever worked with online, and I finally got to meet so many of the wonderful people from Clojure Camp and Scicloj who I've had the pleasure of working with virtually. I also had the chance to meet some of my new co-workers, which was great. There were tons of amazing talks and as always insightful and inspiring conversations. I always leave conferences with tons of energy and ideas. Then get back to reality and realize there's no time to implement them all :) But still, below are some of the main ideas I'm working through after a wonderful conference.

## SVGs for visualizing graphics

Tim Pratley and Chris Houser gave a fun talk about SVGs, among other things, that made me realize using SVGs might be the perfect way to implement the "graphics" side of a grammar of graphics.

Some of you may be following the development of [tableplot](https://github.com/scicloj/tableplot) (formerly hanamicloth), in which Daniel Slutsky has been implementing an elegant, layered, grammar-of-graphics-inspired way to describe graphics in Clojure. This library takes this description of a graphic and translates it into a specification for one of the supported underlying Javascript visualization libraries (currently vega-lite or plotly, via hanami). Another way to think about it is as the "grammar" part of a grammar of graphics; a way to declaratively transform an arbitrary dataset into a standardized set of instructions that a generic visualization library can turn into a graphic. This is the first half of what we need for a pure Clojure implementation of a grammar of graphics.

The second key piece we need is a Clojure implementation of the actual graphics rendering. Whether we adopt a similar underlying representation for the data as vega-lite, plotly, or whatever else is less consequential at this stage. Currently we just "translate" our Clojure code into vega-lite or plotly specs and call it a day. What I want to implement is a Clojure library that can take some data and turn it into a visualization. There are many ways to implement such a thing, all with different trade-offs, but Tim and Chouser's talk made me realize SVGs might be a great tool for the job. They're fast, efficient, simple to style and edit, plus they offer potentially the most promising avenues toward making graphics accessible and interactive since they're really just XML, which is semantic, supports ARIA labels, and is easy to work with in JS.

Humble UI also came up in a few conversations, which is a totally tangential concern, but it was interesting to start thinking about how all of this could come together into a really elegant, fully Clojure-based data visualization tool for people who don't write code.

## A Clojurey way of working with data 

I also had a super interesting conversation on my last night in Alexandria about Clojure's position in the broader data science ecosystem. It's fair to say that we have more or less achieved feature parity now for all the essential things a person working with data would need to do. Work is ongoing organizing these tools into a coherent and accessible stack (see [noj](https://scicloj.github.io/noj/)), but the pieces are all there.

The main insight I left with, though, was that we shouldn't be aiming for mere feature parity. It's important, but if you're a working data scientist doing everything you already do just with Clojure is only a very marginal improvement and presents a very high switching cost for potentially not enough payoff. In short, it's a tough sell to someone who's doesn't already have some prior reason to prefer Clojure.

What we should do is leverage Clojure's strengths to build tools that could leapfrog the existing solutions, rather than just providing better implementations of them. I.e. think about new ways to solve the fundamental problems in data science, rather than just offering better tools to work within the current dominant paradigm.

For example, a fundamental problem in science is reproducibility. The current ways data is prepared and managed in most data (and regular) science workflows is madness, and versioning is virtually non-existent. If you pick up any given scientific paper that does some sort of data analysis, the chances that you will be able to reproduce the results are near zero, let alone using the same tools the author used. If you do manage to, you will have had to use a different implementation than the authors, re-inventing wheels and reverse-engineering their thought process. The problem isn't that scientists are bad at working with data, it's the fundamental chaos of the underlying ecosystem that's impossible to fight.

If you've ever worked with Python code, you know that dependency management is a nightmare, never mind state management within a single program. Stateful objects are just a bad mental model for computing because they require us to hold more information in our heads in order to reason about a system than our brains can handle. And when your mental model for a small amount of local data is a stateful, mutable thing, the natural inclination is to scale that mental model to your entire system. Tracking data provenance, versions, and lineage at scale is impossible when you're thinking about your problem as one giant, mutable, interdependent pile of unorganized information.

Clojure allows for some really interesting ways of thinking about data that could offer novel solutions to problems like these, because we think of data as immutable and have the tools to make working with such data efficient. None of this is new. Somehow at this Conj between some really interesting talks focused on ways of working with immutable data and subsequent conversations it clicked for me, though. If we apply the same ways we think about data in the small, like in a given program, more broadly to an entire system or workflow, I think the benefits could be huge. It's basically implementing the ideas from [Rich Hickey's "Value of values"](https://www.youtube.com/watch?v=-6BsiVyC1kM) talk over 10 years ago to a modern data science workflow. 

Other problems that Clojure is well-placed to support are:
- Scalability -- Current dominant data science tools are slow and inefficient. People try to work around it by implementing libraries in C, Rust, Java, etc. and using them from e.g. Python, but this can only get you so far and adds even more brittleness and dependency management problems to the mix.
- Tracking data and model drift -- This problem has a very similar underlying cause as the reproducibility issue, also fundamentally caused by a faulty mental model of data models as mutation machines.
- Testing and validation -- Software engineering practices have not really permeated the data science community and as such most pipelines are fragile. Bringing a values-first and data-driven paradigm to pipeline development could make them much more robust and reliable.

Anyway I'm not exactly sure what any of this will look like as software yet, but I know it will be written in Clojure and I know it will be super cool. It's what I'm thinking about and experimenting with now. And I think the key point that thinking about higher-level problems and how Clojure can be applied to them is the right path toward introducing Clojure into the broader data science ecosystem.

## Software engineers as designers

Alex Miller's keynote was all about designing software and how they applied a process similar to the one described in [Rich Hickey's keynote from last year's conj](https://www.youtube.com/watch?v=c5QF2HjHLSE) to Clojure 1.12 (among other things). The main thing I took away from it was that the best use of an experienced software engineer's time is not programming. I've had the good fortune of working with a lot of really productive teams over the years, and this talk made me realize that one thing the best ones all had in common is that at least a couple of people with a lot of experience were not in the weeds writing code all the time. Conversely a common thread between all of the worst teams I've been a part of is that team leads and managers were way too in the weeds, worrying too much about implementation details and not enough about what was being implemented.

I've come to believe that it's not possible to reason about systems at both levels simultaneously. My brain at least just can't handle both the intense attention to detail and very concrete, specific steps required to write software that actually works and the abstract, general conceptual type of thinking that's required to build systems that work. The same person can do both things at different times, but not at the same time, and the cost of switching between both contexts is high.

Following the process described by Rich and then Alex is a really great way to add structure and coherence to what can otherwise come across as just "thinking", but it requires that we admit that writing code is not always the best use of our time, which is a hard sell. I think if we let experienced software engineers spend more time thinking and less time coding we'd end up with much better software, but this requires the industry to find better ways to measure productivity.


## Long version of personal updates

As most of you know or will have inferred by now, I got married in September! It was the best day ever and the subsequent vacation was wonderful, but it did more or less cancel my productivity for over a month. If you're into weddings or just want a glimpse into my personal life, we had a reel made of our wedding day that's available [here on instagram via our wedding coordinator](https://www.instagram.com/reel/C__5_r9pAea/).

Immediately after I got back from my honeymoon I also started a new job at BroadPeak, which is going great so far, but also means I have far less time than I used for open source and community work. I'm back to strictly evening and weekend availability, and sadly (or happily, depending how you see it) I'm at a stage of my life where not all of that is free time I can spend programming anymore.

I appreciate everyone's patience and understanding as I took these last couple of months to focus on life priorities outside of this work. I'm working on figuring out what my involvement in the community will look like going forward, but there are definitely tons of interesting things I want to work on. I'm looking forward to rounding out this year with some progress on at least some of them, but no doubt the end of December will come before I know it and there will be an infinite list of things left to do.

Thanks for reading all of this. As always, feel free to reach out anytime, and hope to see you around the Clojureverse :)
