# AI Should Not Become the Source of Truth

I keep running into the same uncomfortable moment when using AI in software work.

You ask it to explain part of a system, and the answer is useful. It connects code, documentation, tests, and names that were spread across different places. It gives you something clearer than what you started with.

For a while, that feels like progress.

Then you notice the problem. The explanation is easier to read than the sources it came from. It is more coherent than the system itself. And because it is coherent, it starts to feel authoritative.

That is the risk I think we need to take seriously. AI can make uncertain knowledge look settled.

## When explanations become too convenient

Most teams already have gaps in their knowledge. Some rules live in code. Some are written down. Some are remembered by people. Some are only visible through behaviour.

AI is good at smoothing that into a readable answer.

Suppose it explains that an order can be cancelled before payment is captured. That may be correct. It may also be a conclusion drawn from the current implementation, rather than a rule the business has named and agreed on. The difference matters, but it is easy to miss when the answer sounds confident.

This is how generated text quietly gains authority. Someone uses the explanation in a discussion. Later it appears in a document. Then it becomes part of the material for another piece of work. Nobody decided that AI should define the rule. The generated explanation just became the easiest thing to reuse.

That is a weak foundation for design.

The important question is not only whether the answer sounds right. It is whether we know where the authority came from.

## Authority belongs somewhere

This is where Domain-Driven Design matters, but in a very practical way.

A bounded context is a place where certain words and rules are allowed to mean something specific. If a context owns ordering, then the ordering context owns what cancellation means in that model. Other contexts may react to published facts, call an API, or keep a local projection, but they do not get to redefine the rule from the outside.

AI should be treated the same way.

It can help interpret the ordering context. It can point to where a rule seems to be implemented. It can draft a clearer explanation than the current documentation. But it should not become the thing that decides what cancellation means.

That authority has to stay with the owning context and the artifacts the team maintains deliberately. Otherwise the model starts moving into generated prose. The system may look documented, but the documentation is no longer anchored strongly enough to ownership.

## Inference should be visible

I do not think the answer is to make AI cautious to the point of being useless. Inference is valuable. A lot of the benefit comes from asking AI to connect things that are not already neatly connected.

The problem is hidden inference.

When AI makes a statement about the domain, especially one that could affect design, it should show the status of that statement. Did it find the rule in an owned artifact, or did it infer it from surrounding evidence? If no clear source exists, it should say that.

That small distinction changes the output.

Instead of only saying how the system works, the answer can say that the behaviour appears to exist, but the source of the rule is unclear. That is not a worse answer. It is a more honest one. It gives the team something useful to act on.

Sometimes the best result of an AI session is not a better summary. It is the discovery that the system depends on knowledge nobody has properly owned.

## Make the gaps part of the document

This also changes how I ask AI to generate documentation.

I try not to ask only for a clean explanation. A clean explanation is often exactly what hides the problem. I want the output to show where the explanation came from, and where the source material is weak.

The instructions can be simple:

> Require evidence for important claims. Mark unknowns as TBD instead of filling the gap. Highlight places where a decision or more clarity is needed. Ask if the source material is not enough.

Those rules change the purpose of the generated document. It is no longer just a summary of what the system seems to do. It also becomes a map of what the system has failed to make clear.

That is useful for the person reading it. While reviewing the generated documentation, they learn more about the system, but they also see where their understanding rests on weak evidence. The missing pieces become visible enough to discuss and fix.

I think this is one of the better uses of AI in documentation work. It can help produce the explanation, but it can also help expose the gaps behind the explanation.

A good generated document should contain both parts: what appears to be known, and what still needs to be clarified.

Without the missing pieces, the document may look better, but the system has not actually become better understood.

## Generated material needs somewhere to land

There is still a line we need to hold.

Generated material should remain provisional until it moves into an owned place.

An AI-generated explanation can become documentation, but only after someone decides that it is documentation. A generated schema can become a contract, but only after the owning context publishes it as one. An architecture note can become a decision record, but not while it is still just text from a chat session.

That may sound obvious, but in practice this boundary is easy to blur. AI makes draft artifacts cheap, and cheap artifacts spread. The more useful they are, the more likely they are to be reused before their status is clear.

This is not a reason to avoid generating them. It is a reason to be stricter about what they are allowed to become.

A draft can be loose. A source of truth cannot.

## The source of truth should be less impressive

There is something unglamorous about a good source of truth. It is often not the nicest explanation. It may be a contract, a test, a policy, a decision record, or the part of the code that enforces an invariant.

AI can make those things easier to work with. It can summarize them, compare them, and expose contradictions between them. That is useful work.

But when the generated explanation disagrees with the owned source, the owned source wins. And when there is no owned source, AI should not fill the gap so smoothly that nobody notices.

The gap is the information.

It tells us that the knowledge has not yet been placed where it belongs.

## Keeping authority visible

For AI-assisted development, I think this is the rule worth holding onto:

A generated explanation may start the conversation. The source of truth must finish it.

That does not mean every team needs more ceremony. Most teams have enough of that already. The point is smaller and more concrete: when AI says something important about the domain, we should know whether it is repeating an owned source, interpreting weak evidence, or guessing.

AI can be useful in all of those cases. The important thing is that the status stays visible.

Bounded contexts must still own the meaning.

## Next

This leads to the next practical issue.

If bounded contexts own meaning, they need a way to publish some of that meaning to the outside world. Not everything. Not the whole internal model. Only the parts other contexts are allowed to depend on.

That is where contracts become more than technical schemas.

They become published language.
