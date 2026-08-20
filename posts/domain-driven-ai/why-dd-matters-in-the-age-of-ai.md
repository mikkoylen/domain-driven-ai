# Why DDD Matters More in the Age of AI-Generated Code

Much of the discussion I notice around AI-assisted development quickly turns to productivity. Generated code, completed tasks and agent autonomy are relatively easy things to demonstrate.

I am not sure they tell us much about productivity.

If the software solves the wrong problem, producing it faster does not help. At best, those measures tell us that implementation got faster. Whether the work was useful is much harder to see.

Still, AI has changed something for me personally.

For the first time in a while, software feels more like creating again. I can start with an idea and get to something tangible quickly. I can try one implementation, throw it away, explore another direction and spend more of my time shaping the thing I am trying to build.

That has brought back some of the spark that made software interesting to me in the first place.

I like that change.

It has also made me more interested in Domain-Driven Design again.

## Faster implementation can hide weaker thinking

When implementation was slower, there was some natural friction in turning an idea into software.

That friction was hardly a virtue. I do not miss spending hours writing repetitive code or debugging problems that a tool can now solve in seconds. Removing that work is one of the reasons I enjoy using AI.

But friction had a side effect. Turning an unclear idea into a large amount of working software took time.

AI reduces that delay.

I can give an AI assistant an idea that is still poorly formed and get something surprisingly complete back. It can produce code, tests and supporting documentation that agree with each other. The result can look coherent very quickly.

That coherence can be misleading.

A misunderstood business rule can still be implemented cleanly. A vague concept can have a good API. Tests can prove that the implementation behaves exactly as specified while the specification itself is based on the wrong understanding.

The uncomfortable part for me is that AI can make a weak assumption look established much faster than before.

This does not make AI the problem. The unclear thinking was already there. AI simply gives it more reach.

## The bottleneck moves towards understanding

In the systems I work with, the difficult problems are rarely caused by an inability to write enough code.

They tend to appear around meaning.

A concept means one thing to one part of the business and something slightly different somewhere else. Ownership is unclear. A rule that looked simple turns out to have an exception nobody mentioned. Software gradually stops matching how the business actually works.

Those problems existed long before generative AI.

They also do not disappear when implementation becomes cheaper.

If anything, I think they become more visible as the limiting factor. Once I can produce a reasonable implementation quickly, I spend proportionally more time deciding what the implementation should actually mean.

That is where my thinking has shifted.

I used to think of the expensive part of software largely in terms of implementation effort. Increasingly, the scarce part seems to be a sufficiently clear understanding of the domain to know what should be implemented.

I cannot measure that shift neatly, and I am wary of pretending that I can. It is an observation from using these tools in real development work.

The code has become easier to produce. The difficult conversations have not.

## DDD gives the model somewhere to come from

For a long time, I thought about DDD mainly as a way to design better software models.

I still think that is true, but it feels incomplete now.

A domain model is also a record of what we currently understand about part of the business. It gives names to concepts, makes rules explicit and forces decisions about where something belongs.

That understanding is never finished.

Real domains keep exposing things the model did not account for. An edge case appears. Someone from the business uses a word differently than the development team. A production incident reveals an assumption that was never really true.

The model changes as we learn.

I find this side of DDD more interesting in the context of AI than the usual discussion about tactical patterns. Aggregates, entities and value objects still have their place, but they are downstream of something more important: learning enough about the domain to create a useful model of it.

AI can make that learning loop faster as well.

I already use it to explore ideas, challenge an implementation and make connections across information that would take longer to work through manually. Sometimes that exposes a gap in my thinking before I have written much code.

Other times it confidently reinforces the assumption I started with.

That difference matters.

The useful loop is not simply idea to generated code. It is understanding to model to software, followed by whatever reality teaches us next.

DDD gives that loop some structure.

## Boundaries matter when AI has access to everything

There is another effect I have noticed when working with AI. My first instinct when it lacks context is usually to give it more.

More code. More documentation. More instructions.

Sometimes that works.

Eventually, though, more material starts mixing things that should probably remain separate. Different parts of a system have different models, different language and sometimes different meanings for the same word.

This is where bounded contexts have started to look useful to me in a slightly different way.

A bounded context already tells us that a model and its language are valid within a particular boundary. The meaning does not need to apply everywhere.

That is useful for people designing software, and I suspect it is equally useful when deciding what information an AI assistant should reason over.

The value is quite practical. Words, rules and ownership are no longer floating around the whole system. They have a place, a context.

I do not yet know whether a bounded context maps neatly to the ideal context for an AI assistant. It probably does not in every case. But giving an AI a smaller, coherent part of the domain seems more promising than continually adding material and hoping it can work out which parts belong together.

There is more to unpack there, especially around ubiquitous language, contracts and sources of truth. I want to leave that for later rather than squeeze the whole argument into this post.

## Cheaper code makes domain clarity more valuable

I do not think AI reduces the need for software design.

My experience so far points in the other direction.

As implementation becomes easier, more of my attention moves towards the assumptions behind it. I care more about whether the concept is understood, whether the language is shared and whether the model belongs inside the boundary where I am changing it.

DDD already gives us ways to work with those problems.

That does not mean DDD somehow solves AI-assisted development. I am not convinced there is a single method that does. Some DDD practices will probably turn out to be more useful in this setting than others.

What feels increasingly clear to me is that generating more software is not the interesting problem.

The interesting part is keeping that software connected to an understanding of the domain that can still change as we learn.

AI has made creating software enjoyable for me again. I would rather use that new speed to shorten the path between an idea and learning whether the idea was any good than simply increase the amount of software I can produce.

That is the direction I want to explore with Domain-Driven AI.

I am still unsure what the right boundaries for AI-assisted development will look like in practice. A bounded context designed for a software system may not automatically be the right unit of context for an AI assistant.

But as implementation gets cheaper, I find myself caring more about the quality of the domain model and the boundaries around it.

The next part of the problem starts there: how much context actually helps, and when does more material simply become noise?
