Title: OSS Updates July and August 2024
Date: 2024-08-31
Tags: open source, clojure, clojurists together, oss updates

This is a summary of the open source work I've spent my time on throughout July and August, 2024. There was a blog post, some library updates, and lots of community work.

## Sponsors

This work is made possible by the generous ongoing support of my sponsors. I appreciate all of the support the community has given to my work and would like to give a special thanks to Clojurists Together and Nubank for providing me with lucrative enough grants that I can reduce my client work significantly and afford to spend more time on these projects.

If you find my work valuable, please share it with others and consider supporting it financially. There are details about how to do that on my [GitHub sponsors page](https://github.com/sponsors/kiramclean). On to the updates!

## Blog post

At the beginning of the summer Daniel Slutsky and I were feeling very ambitious and thought we might be able to put together a course for data scientists coming to Clojure from other languages. For many reasons, this hasn't materialized yet, but in service of these plans I wrote a [blog post comparing tablecloth](https://codewithkira.com/2024-07-18-tablecloth-dplyr-pandas-polars.html) to other common data processing tools, like `dplyr`, `pandas`, and `polars`. My goal was to put tablecloth in perspective, illustrating some of the key differences between it and other standard, more popular, data processing tools.

## `tcutils`

I added a few more helpers to `tcutils`, like `between` and `duplicate-rows`, and also made a [docs website](https://scicloj.github.io/tcutils/) for the project. I also had many interesting conversations with people in the community about how Clojure's data processing tools "feel" to work with, and how we might adopt APIs and conventions that are familiar to data scientists in the interest of making their transition to Clojure's ecosystem as smooth as possible.

## Clojure Data Cookbook

This month I added a chapter about working with data from databases, starting with SQL, and also continued to work on the end-to-end example for the introductory section. Working with real data is very difficult and interesting, and it's a fun challenge to try to figure out the right balance between getting into the weeds and compromising on the final result. So much of data science is just cleaning up messy data from the world, but surprisingly often you have to make some assumptions about how you're going to use the data in order to make decisions about how to do the cleaning. And there are tons of different ways to "clean" data, but the strategies you use depend on what information you're after.

In the particular example of the housing dataset I'm working with there are many missing values to handle, and some questionable rows that look like duplicates but aren't _exactly_ duplicates. There are also lots of illogical data points, like house sales from the future or multiple sales for the same property on a given date. Deciding how to handle these cases to build up a "clean" dataset to actually work with is a very interesting exercise in domain modelling and goal setting.

## Scicloj mentoring program

This one is really mostly Daniel Slutsky's amazing work, but we collaborated on launching it and it's definitely worth mentioning. We put together a [structured way for people to get involved](https://scicloj.github.io/docs/community/groups/open-source-mentoring/) in contributing to Clojure's open source data science ecosystem, and got an overwhelmingly positive response. Over 25 people reached out to express an interest in contributing their time to Scicloj projects. The structured parts of the program include having some help choosing a meaningful and impactful project to work on, and up to an hour per week of one-on-one time with a mentor to help things progress. Daniel is doing all the heavy lifting coordinating the mentors, but it's been great so far participating as one and meeting some very keen and smart people who are willing to help us move things forward.

Another big part of this is thinking of the projects to work on. We came up with a list of projects that would deliver high value to the community but remain small enough to tackle by a single developer. We also tried to come up with ones that would require a wide range of skills and interests to try to accommodate as many people as possible. I am super excited to see how things go over the next few months with all of these projects.

## Other community connections

I'm still doing my weekly data-science drop-in streaming with Clojure Camp. I really enjoy connecting with other people who are interested in Clojure for data science, and I often get great suggestions and tips, too.

I also met with a couple of groups of people who are presenting at the Conj this year to help brainstorm some ideas for how to make the most of the talks. Daniel has amazing vision for the community and organized these calls that I was lucky enough to join. The goal is to connect all of the people who are giving data-related talks to optimize the overall messaging, like minimizing duplication across talks or drawing examples from each other's presentations. I love conference speaking and hope to do more of it in future years when my personal commitments allow for it, but in the meantime it's really amazing getting to connect with such cool people in the community to learn about their talks and brainstorm ideas for making them the best they can be. I'm hoping to attend the conference this year to see some of these great talks in person.


## Personal Updates

This has been a really amazing year professionally, having had the opportunity to spend much more time than in the past on open source and public work for the Clojure community. I've been trying to make the most of it and it's been really rewarding. Over the next couple of months, there are some other parts of my life that will be taking precedence, however.

The main one is my relationship. I'm getting married in a couple of weeks and will be taking almost a month off between getting ready for the wedding, wrapping up all the loose ends afterward, and a nearly 3-week-long honeymoon. I've never taken this long off of work in my life, so I'm both excited and curious to see what it's like. For over a decade now my career has been taking up most of my time and energy. It's been well worth it and I'm really happy with my work now, but I'm also excited to be stepping into a new chapter of life where things can be more balanced.

Related to this, the other major update I have to share is that I've accepted a full time job with a company called BroadPeak which I will be starting as soon as I'm back from my honeymoon. It's a small fintech company built primarily with Clojure that handles trade data management, commodities transaction surveillance, regulatory compliance, and other things related to the behind-the-scenes of commodities trading. I think it's a perfect fit for my skills and interests, and I'm hoping to have a chance to build some bridges between a really exciting, growing company that uses Clojure for real-world financial data processing and the Clojure open source community. Initial conversations about how the engineering team there feels about open source and community involvement have been really promising, so I'm optimistic that it will work out well for everyone. I'm not sure yet what exactly my open source work will look like once this job starts, but at a minimum I will still be working on the various side projects, like I always was before I tried giving it a go full time.

No matter how things go, I'll be back in two more months with another update. Thanks for reading. As always, feel free to reach out, and hopefully see some of you at the Conj! :)
