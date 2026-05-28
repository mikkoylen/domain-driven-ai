# Domain-Driven AI

A repository for essays, notes, examples, and experiments around **Domain-Driven Design in the age of AI-assisted software development**.

The central idea is simple:

> AI can help us produce code faster, but it does not remove the need for clear language, boundaries, ownership, and domain understanding.

In fact, it may make those things more important.

## What this repository is

This repository is the companion workspace for the **Domain-Driven AI** blog series.

It contains:

* blog posts in Markdown format
* notes and drafts behind the posts
* possible future code examples
* AI agent instructions and prompts
* small experiments related to DDD, bounded contexts, documentation, and AI-assisted development

The material here is not meant to be a polished framework from day one. It is a place to develop the thinking in public and keep the ideas versioned, reviewable, and reusable.

## Working thesis

AI changes the economics of software creation.

It can generate code, tests, documentation, APIs, diagrams, and refactorings faster than teams used to produce them manually. That is useful. But speed also changes the failure mode.

When implementation becomes cheaper, unclear thinking can spread faster.

Domain-Driven Design gives us practical tools for resisting that drift:

* clearer language
* explicit boundaries
* owned models
* bounded context
* better-scoped knowledge
* stronger links between software and the domain it serves

This repository explores how those ideas apply when humans and AI work together on software systems.

## Repository structure

The structure may evolve, but the initial shape is:

```text
.
├── posts/
│   └── published and draft blog posts in Markdown
│
├── notes/
│   └── rough ideas, outlines, and working thoughts
│
├── agents/
│   └── AI agent instructions, prompts
│
├── examples/
│   └── possible code or architecture examples
│
└── README.md
```

## Main themes

The repository will likely focus on these topics:

### DDD and AI-assisted development

How DDD changes when AI can help generate large amounts of software quickly.

### Bounded context as an AI design constraint

Why AI does not only need more context, but better-scoped context.

### Domain models as learning tools

Treating a domain model as a working theory about how part of the world behaves, not as a static design artifact.

### Documentation as shared context

Using Markdown, structured facts, contracts, and summaries to make knowledge easier for both humans and AI to use.

### Events, ownership, and meaning

Exploring how event-driven systems, bounded contexts, and published language help preserve meaning across system boundaries.

### AI agents inside bounded contexts

Thinking about agents as tools that should operate within clear ownership, contracts, and domain boundaries.

## Blog series

The related blog series is called:

**Domain-Driven AI**

It explores why DDD may become more important, not less, as AI becomes more common in software development: https://blog.unplugit.fi/series/domain-driven-ai

The posts are written as essays, not formal documentation. They are meant to reason through architectural concerns in a practical way.

## Status

This repository is early.

Some ideas will change. Some posts may be rewritten. Some examples may remain experimental.

That is intentional.

The goal is not to present a finished doctrine, but to build a clearer way of thinking about AI, software design, and domain understanding.

## License

License to be decided.

Until a license is added, assume the content is published for reading and reference, but not for unrestricted reuse.
