---
layout:     post
title:      "Perl to Go (Golang)"
subtitle:   "A transition that's required"
date:       2015-10-13 12:00:00
header-img: "img/bg.jpg"
---

Let me be frank, Perl is great! It's the first programming language that I used in
bioinformatics and I never had to look back. That said, I feel that it's time
for a change. It's not that Perl is not a good fit anymore but it kind of
isn't.

Let me explain. In the last years there has been a paradigm shift in
biological research. High Throughput Sequencing (HTS) has dominated the field.
Almost everyone nowdays is, one way or the other, working with HTS datasets.
Trying to cope with the huge amount of data is a serious bottleneck. In our
lab we can still use Perl to address the biological questions at hand. We do
this by delegating computationally intensive tasks to tools specifically
designed for big data, databases. We had to sacrifice a little bit of
flexibility so we could still keep using Perl. The question is why did we do
that? Why were we reluctant to change or programming environment from a
interpreted language to a compiled one?

The answer is simple and manifests itself in every article in favor of
interpreted languages - it is so easy to build stuff. Bioinformatics is a
field at the frontend of science. Analyses are rarely well defined a priori.
Most often we do not know beforehand what we are looking for in the data. We
have to improvise all the time and follow every single lead that we get. It is
evident that lots of prototyping is needed, and this is where interpreted
languages shine. We do not need to declare types. We just import the data,
in whatever format they are, integers, floats strings, etc and process them to
extract information.

So why is a change needed?

There are two main reasons for that. The first one has to do with the massive
datasets as it gets harder and harder to pretend that interpreted languages
can cope with the amounts of data. The second, is very practical. It's all
about money. It has to do with the actual cost of running our programs in the
cloud.

Lets see the first reason first. As I already mentioned interpreted languages
are great for prototyping but if there was an alternative I would be happy to
investigate it. So lets see what are our requirements. We want an alternative
that is maybe not as expressive as Perl but at least close to it and that
gives a significant performance boost. I believe that such an alternative can
be Go or Golang if you prefer. 

I have been very reluctant to switch from Perl to a new language mainly
because of all the libraries that I already had. So I started investigating Go
in small steps. First I read a few tutorials, then I wrote a Hello World
program and then I investigated if there are any bioinformatics code available
in Go. I found biogo, a very interesting library that encapsulates a lot of
concepts required in bioinformatics. I though that this was a great
opportunity to further dive into Go. I submitted my first pull request into
biogo. It was a single package that is meant to provide types and methods for
handling gene model annotations. Since then I've built a few more libraries
and some executables. To be honest still when I need something urgent, Perl is
my language of choice but I think that getting accustomed to Go will very soon
make me comfortable enough to switch to Go as my main programming language.

Without having any concrete data to support the following claim I find myself
writing 50% more lines of code in Go than an equal program in Perl but I
usually get 10x better performance.

So this brings us to the second reason why the language transition is
necessary. In his excellent post [Hardware is Cheap, Programmers are
Expensive](http://blog.codinghorror.com/hardware-is-cheap-programmers-are-expensive)
Jeff Atwood argues that seeking the most optimized algorithmic solution can
often be more expensive than actually buying more harware to compensate for
the algorithmic inefficiencies. The reason is that the developers compensation
far outlays the hardware cost.

> Clearly, hardware is cheap, and programmers are expensive. Whenever you're
> provided an opportunity to leverage that imbalance, it would be incredibly
> foolish not to.

But what happens when this concept is applied to choosing the right language
for the job.  What is the sweetspot of language expressiveness and therefore
speed of code production versus the computational cost of running your
programs in the cloud. Evidently using an interpreted language will save
development time but then the cost of running the program on the cloud will be
higher. I personnaly tend to think that investing 50% more effort during
development is worth the outcome of approximatelly 10x performance improvement
that is translated in lower running cost.

And there is another thing that just came into my mind. Improvisation requires
immediate feedback. I personnally want to run a program see the output and
rerun using different parameters. It would greatly help me if things run 10x
times faster. It keeps my brain focused.
