# AI Should Not Become the Source of Truth

Most discussion about AI-generated documentation focuses on accuracy. Can the model find the right information? Does it hallucinate? Can its answer be verified?

Those concerns matter, but I have become more worried about a quieter failure.

I have repeatedly asked AI to explain part of a software system by reading its code, documentation and tests. The result is often useful. It connects details that were spread across several places and produces a clearer explanation than any single source provided.

At first, that feels like progress.

The discomfort comes later, when the explanation starts to feel more authoritative than the material behind it. It is coherent, easy to reuse and written with none of the uncertainty present in the system itself.

AI can make unsettled knowledge look settled.

## A coherent answer can acquire authority

Most teams already have gaps in their shared knowledge.

Some rules are written down. Others are enforced in code or captured indirectly in tests. A few are remembered by people who have worked with the system long enough to know its history. Some are visible only through the way the system behaves.

AI is good at smoothing these fragments into a readable answer. That is part of what makes it useful.

Suppose an explanation says that an order can be cancelled until payment has been captured. The current implementation may support that claim. There may also be tests that expect the same behaviour.

Still, neither tells me whether this is an agreed business rule or simply how the implementation happens to work today.

That uncertainty is easy to lose once the explanation has been written cleanly. Someone reuses it in a design discussion. Later, it appears in documentation. The next person treats that document as input for another piece of work.

Nobody explicitly decided that AI should define the cancellation rule. Its explanation became authoritative because it was the easiest version to understand and reuse.

The individual steps are reasonable. The authority emerges through accumulation.

## Meaning still needs an owner

This is where I find Domain-Driven Design useful in a very practical way.

A bounded context gives particular words and rules a place where their meaning is owned. If the ordering context owns order cancellation, then the team responsible for that context must be able to say what cancellation means in its model.

Other contexts may call its API, react to published events or maintain local projections. They can depend on what the ordering context publishes, but they should not redefine the rule from the outside.

I think AI needs to work within the same boundary.

It can interpret the ordering context. It can locate code that appears to enforce a rule and tests that demonstrate the current behaviour. It can also draft a clearer explanation than the team has written so far.

What it cannot do is turn that interpretation into an owned domain rule by itself.

If generated prose is allowed to carry that authority, the model starts drifting away from the context that is responsible for it. The system may appear better documented while its meaning becomes less clearly owned.

## Generated documentation should expose its gaps

I do not want AI to avoid inference. Much of its value comes from connecting information that was never organised for a single reader.

The problem is hidden inference.

When AI makes a statement about the domain, I want the answer to show what supports it. A rule found in an owned contract has a different status from behaviour inferred from code. A conclusion drawn from a test is different again from something the model could not verify at all.

The cancellation example could then be described more honestly:

> Cancellation before payment capture appears to be supported by the current implementation and tests. No owned business rule confirming this constraint was found.

That answer is less polished, but more useful. It explains the observed behaviour without silently promoting it into a domain decision.

I have started asking for this explicitly when generating documentation:

> Require evidence for important claims. Mark unknowns as TBD instead of filling the gap. Highlight places where a decision or more clarity is needed. Ask when the source material is not sufficient.

This makes the document look less finished. It may contain unresolved questions, missing decisions and awkward contradictions.

That is a fair cost. Those gaps were already part of the system. The generated document has only made them visible.

Sometimes that is the most valuable result of the work. Instead of producing a smoother summary, AI reveals that an important rule has no clear owner or authoritative source.

## Drafts need somewhere to land

Generated material should remain provisional until it moves into an owned place.

An AI-generated explanation can become documentation after someone responsible for the context reviews and publishes it. Until then, it is a draft based on the evidence that happened to be available.

The same applies to generated schemas. A schema becomes a contract when the owning context accepts it, publishes it and takes responsibility for changing it. Text produced in a chat session has none of those properties on its own.

This boundary is easy to blur because AI makes plausible artefacts cheap. A useful draft can spread through tickets, chat messages and design documents before anyone has decided what status it should have.

Avoiding generated artefacts would throw away much of the benefit. I would rather make their transition into owned material explicit.

A draft can tolerate uncertainty. A source of truth has to make that uncertainty visible or resolve it.

## The source of truth may be less readable

There is something unglamorous about most authoritative sources.

The rule may live in a contract, a policy, a decision record or the code that enforces an invariant. None of these is guaranteed to provide the clearest explanation for a person encountering the system for the first time.

AI can make these artefacts easier to work with. It can summarise them, compare versions and expose contradictions between them.

The phrase “source of truth” can still make the situation sound cleaner than it is. An owned document can be outdated. A test may preserve an obsolete assumption. Code may behave differently from the published contract.

Ownership does not remove those conflicts. It tells us where the conflict must be resolved.

When sources disagree, AI can show the disagreement and gather the available evidence. It should not settle the domain meaning by choosing whichever interpretation forms the most coherent story. That decision belongs to the people responsible for the context.

The gap remains useful information. It shows where the system’s knowledge has not yet been made dependable.

## Authority must remain visible

The rule I want to preserve is simple:

A generated explanation may start the conversation. An owned source must finish it.

This does not require a new approval process for every paragraph produced with AI. It requires enough discipline to distinguish between what the model found, what it inferred and what the owning context has accepted.

AI remains useful in each case. The status of the output is what matters.

Bounded contexts must still own their meaning.

That leaves a practical problem for the next part of this series. A context cannot keep all of that meaning to itself. Other parts of the system need stable facts and rules they are allowed to depend on, without gaining access to the whole internal model.

Some of that meaning has to be published.

That is where contracts begin to carry more than structure.
