Title: OSS Updates January and February 2024
Date: 2024-02-29
Tags: open source, clojure, clojurists together, oss updates

I was lucky enough to get funding this year from Clojurists together to work on some open source projects for the Clojure community. It's been a really fun couple of months getting more involved in the ecosystem and having the time to work on some projects that I've long thought would be valuable for the community. This post is a summary of the things I've been working on over the past two months.

## Sponsors

First of all, I want to thank the sponsors that make this work possible. We're living through the worst tech job market since I started working as a software engineer, and I'm lucky to have a little bit of time and runway to work on things I find interesting thanks to the generous sponsors who find my work worthwhile.

Right now my work is primarily funded by [Clojurists Together](https://www.clojuriststogether.org) and [Cognitect/Nubank](https://www.cognitect.com). Thank you to these major sponsors, and to everyone who contributes to my continued work in the Clojure open source ecosystem.

If you find the work I do valuable, please share it with others or consider supporting it financially. I would love to be able to turn working on this kind of stuff into a sustainable career in the long term.

## Clojure Tidy Tuesdays

The main thing I spent my time working on over the past couple of months was a collection of tutorials and guides for working with data in Clojure. The R for Data Science online learning community publishes toy datasets every week for "Tidy Tuesdays" with a question to answer or example article to reproduce. I've been going through them in Clojure, and it's proven a great tool for uncovering areas for future development in the Clojure data science ecosystem.

## Other Work

The explorations with the Tidy Tuesday data have been revealing areas where I think we could benefit from more ergonomic ways to work with tablecloth datasets. I started two little projects each with a couple of little wrappers around existing functions to make them easier to use with tablecloth datasets. So far I'm calling them `tcstats` (for statistical operations on datasets) and `tcutils` (with miscellaneous dataset manipulation tools that aren't built-in to tablecloth directly).

I am also still working on the Clojure Data Cookbook. I nudged it forward ever so slightly these last couple of months, and I plan to finish it despite the remaining holes in Clojure's data science stack. I would love to also fill these in eventually, but the Cookbook will be a living document that can easily evolve and be updated as new tools and libraries are developed.

Lastly, one of the main missing pieces I'm discovering we really need to work on in Clojure's data science ecosystem is a robust yet flexible graphics library. There are a few great solutions that already exist, but they take different approaches to graphing that can make them a bit clumsy to work with when it comes time to build more complex visualisations. My dream is to implement a proper [grammar of graphics](https://ggplot2-book.org) in Clojure, giving the Clojure data ecosystem a "profressional quality" graphics library, so to speak. Anyway.. there is still tons of work to do here so I'm grateful for the ongoing funding that will allow me to continue to focus a large amount of time on it for the foreseeable future.




