Title: OSS Updates November and December 2024
Date: 2024-12-31
Tags: open source, clojure, clojurists together, oss updates

This is a summary of the open source work I spent my time on throughout November and December 2024. This was the last period of my ongoing funding from Clojurists together. It's been such a magical year in many ways and I'm so grateful to have had the opportunity to spend so much time focusing on open source this year. It was a fantastic experience and I hope to be able to take another professional hiatus at some point in the future to do it again.

I've really enjoyed writing these summaries, too, and I find knowing they're coming helps motivate me to stay more organized and keep better track of things, so I'll probably continue to publish them even though I'll be spending less time on side projects as my focus shifts to other priorities this year.

## Sponsors

I always start these posts with a sincere thank you to the generous ongoing support of my sponsors that make this work possible. I can't say how much I appreciate all of the support the community has given to my work and would like to give a special thanks to Clojurists Together and Nubank for providing incredibly generous grants that allowed me reduce my client work significantly and afford to spend more time on projects for the Clojure ecosystem for nearly a year.

If you find my work valuable, please share it with others and consider supporting it financially. There are details about how to do that on my [GitHub sponsors page](https://github.com/sponsors/kirahowe). On to the updates!

## BOBKonf 2025

One exciting update is that [a workshop I proposed got accepted to BOBKonf](https://bobkonf.de/2025/howe.html), which will be in Berlin next March. It'll be similar to the types of talks and workshops I've been doing over the last couple of years at e.g. the Conj and London Clojurians, of course updated to show off the latest and greatest developments in the Clojure-for-data ecosystem. I spent some time in December beginning work on the content and now I'm in full conference-driven-development mode, figuring out what's realistic to finish in time to demo at the event and what we should consider stable "enough" for now and just include. This preliminary work also sparked a couple of minor conversations, one about [quarto theming of Clay notebooks](https://github.com/scicloj/clay/issues/192) and another about [parsing dates from Excel workbooks](https://clojurians.zulipchat.com/#narrow/channel/236259-tech.2Eml.2Edataset.2Edev/topic/excel.20dates/near/486781881).

Anyway there are still a couple of months to work on it, which on one hand feels like a long time but on the other hand is also no time at all. Before I know it I'll be landing in Berlin ready to share the wonders of Clojure with a new eager audience.

## Clojure Data Cookbook

This has been a very long-running, very ongoing project of mine. The high level goal was always (and still is) to create resources that would allow people to figure out how to be productive with Clojure's data stack. In reality what this particular project morphed into was a process for discovering the gaps in the ecosystem and guiding development of new tools, uncovering missing features to implement or new libraries to write every time I'd start work on a new chapter.

We've come a long way over the past couple of years and there's still work to do but the ecosystem is reasonably mature now. The [Noj book](https://scicloj.github.io/noj/) has taken on covering a lot of the topics I wanted to document thanks to Daniel Slutsky's incredible efforts at coordinating the community to produce this amazing content. The list of draft articles demonstrates many of the areas where work is still very ongoing in the development of the various libraries. Tutorials are mostly not left unfinished because the authors haven't gotten around to finishing them, but more because the question of what exactly to write about is yet to be answered.

On the Clojure Data Cookbook itself, the current work in progress is [available here](https://scicloj.github.io/clojure-data-cookbook/) and includes only sections that document stable and established functionality. The goal of making Clojure's data stack accessible and easy to work with is still at the top of my priority list but conversations are underway about what the best way to do that is in the context of the current ecosystem.

## ggclj

Another project I've been poking away at the last couple of months is my implementation of the [grammar of graphics in Clojure](https://github.com/kirahowe/ggclj). Most of my effort here is spent learning more and more about the core concepts of data visualization and how graphics can be represented using a grammar, and then how that grammar could be implemented in an existing programming language. This along with exploring prior implementations in other languages. I have a very rudimentary build process working for transforming an arbitrary dataset into a standardized, graph-able dataset, but nothing yet on the actual graphic rendering. It's very interesting and satisfying but I'm not sure how useful. But, in the spirit of heeding Rich's advice from the last Conj about doing projects for fun, I haven't let it go completely. It's still something I'd love to get working someday.

## Reflecting on a year of open source

As I mentioned above I really enjoyed having the time this year to work on so many interesting projects for the Clojure community. It's so rewarding to see how far we've come. Even though it feels like there is still so much to do, I think it's important to reflect on the progress we have made and think about how the problems we encountered along the way shaped the path we took.

When I first started working on the Clojure Data Cookbook, there wasn't even a way to publish a book made out of Clojure files. Clerk was brand new and Clay barely existed. Now we can render a pile of Clojure files as a Quarto book! And the need for better documentation has spurred tons of amazing development in this space. The literate programming story in Clojure is better than in any other language.

We've also made huge strides in connecting the various libraries of the ecosystem together. At the beginning of the year there were many amazing but disconnected libraries. I've been really inspired by the ideas behind the tidyverse and have been trying to communicate the idea of sharing common idioms and data structures. An ecosystem is starting to emerge in Noj that offers a coherent, standardized, shared paradigm for using all of the amazing tools of the Clojure data ecosystem together. The default stack has been chosen, and serious efforts are now underway toward making these libraries feature complete and interoperable. And I plan to continue working on tutorials, guides, and workshops as much as I can to help promote it all. 

I'm grateful for all the changes in my life that have taken my time away from working on side projects as much as I used to, like marriage and a great new job, but in many ways I miss doing more of this work and I sincerely hope I find myself in a position to veer off of this "standard" life track in the future to take a period to focus on this kind of stuff full time again. Even better would be figuring out a way to make it sustainable so that I could continue to do it full time. If you have any idea  how to make that work, let me know :)

It turns out I am not the kind of market-oriented, entrepreneurially-minded person who can turn coding skills into a business that generates steady income for my family. I like contracting and the slow-and-steady community building type of work that constitutes a career in open source, but unfortunately continuing down this road is just not in the cards for me this year. Though I'll never be able to completely resist working on it whenever I can :) Thanks so much for reading this far, and hope to see you around the Clojureverse!