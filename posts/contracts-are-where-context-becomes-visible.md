# Contracts Are Where Context Becomes Visible

Most writing about bounded contexts pays close attention to what happens inside the boundary. The model should use the language of the domain. Business rules should have a clear home. Concepts that mean different things should be kept apart.

That work matters. I have seen how quickly a system becomes difficult to change when the internal model loses those distinctions.

I am more interested here in another part of the context: the surface it presents to everyone else.

Other teams do not see the aggregates or the private concepts that make the implementation coherent. They meet an API, a command, an event or a schema. From there, they decide what the context knows and what they can depend on.

The contract is where the context becomes visible.

AI makes that surface more consequential. It can generate clients, handlers, tests and documentation from a contract with very little effort. This is genuinely useful. It also allows unclear language to travel further before anyone notices what is missing.

## The boundary is a published model

Contracts are often treated as technical artefacts. An OpenAPI definition describes HTTP operations. An AsyncAPI schema describes messages. A DTO gives two systems a structure they can both serialise.

All of that is true, but it leaves out the part I have become more interested in.

A contract publishes part of the context's understanding. It tells the outside world what the context accepts, what it is prepared to state as true and which concepts it owns. It does not expose the whole domain model. It exposes the part that other contexts are allowed to build on.

Consider an event called `OrderUpdated`.

The name says that something changed, while avoiding any claim about what is now true. The payload may contain enough fields to reconstruct the answer. The documentation may describe the common cases. The people involved in the original integration may simply remember what they agreed.

That can work for a while.

Then another consumer arrives. A dashboard starts using the event. A support process depends on one of its fields. Someone points an AI tool at the schema and asks it to create a handler. Each new use gives the vague event a little more authority.

The missing distinction may be between an order being confirmed, cancelled, rejected or adjusted. Those are different facts. Consumers can depend on them in different ways. If the contract does not express the distinction, every consumer has to recover it from somewhere else.

The internal model may be precise while the published model remains vague.

## AI lets ambiguity travel further

Weak contracts caused problems long before AI. Consumers depended on accidental fields. Internal structures leaked into public APIs. Events became generic change notifications because naming the business fact took more work.

The friction around those weaknesses used to reveal some of them. A consumer team had to ask what an event meant. Someone writing a client had to inspect the examples. A documentation gap caused a conversation.

AI can remove much of that friction.

When a contract is vague, the model rarely stops at the vague part. It draws intent from names, types, examples, nearby code and whatever documentation happens to be available. The generated result can look more complete than the source material deserves. The code reads well. The tests pass. The explanation sounds settled.

I find this more worrying than an obvious generation failure. Missing meaning does not necessarily produce broken software. It can produce working software built on an assumption that nobody made explicit.

A precise contract gives AI a smaller space in which to infer. A vague one still gives it plenty to generate, but much less to justify what it generated.

AI is unusually willing to continue where a human integration discussion might have paused.

## Exported internals weaken the boundary

One common shortcut is to generate the external contract directly from the internal model. I understand the appeal. The fields and types already exist. Reusing them avoids mappings and duplicated structures. In a small application, the trade-off may be entirely reasonable.

Across bounded contexts, the two models have different responsibilities.

The internal model serves the rules and decisions of the owning context. Its language can be rich in places that matter only inside that boundary. It can change as the team learns more about the domain.

The contract serves communication across the boundary. It needs to be deliberate about what it reveals, what it hides and what other contexts may rely on. Some internal distinctions are irrelevant outside. Some explanations needed by consumers have no natural place in the internal type.

Generating one from the other can hide this design decision. The result looks consistent because the same names and structures pass through every layer. It may also expose implementation choices as promises to consumers.

AI tooling tends to make direct reuse even more attractive. One schema can become a model, an API and a client with very little visible effort. The saved mapping code is easy to count. The lost freedom at the boundary is harder to see.

Some duplication is the cost of keeping meanings separate.

## Commands and events expose ownership

Commands and events make the published model easier to see because they speak in different directions.

A command names something the context is willing to be asked to do. `UpdateOrder` leaves most of the intent outside the contract. `ConfirmOrder`, `SelectDeliveryTime` and `CancelOrder` bring more of the business action into the language of the boundary.

This is useful even when the names are not perfect. The command makes it clearer that the caller is asking the owning context to apply its rules. It is not requesting a direct mutation of someone else's data.

An event carries a different claim. It states something the context is prepared to publish as true. `PaymentCaptured` gives a consumer a fact to reason about. `PaymentUpdated` tells the consumer to look elsewhere for the meaning.

Not every event represents a major business milestone. Some distribute data. Others signal progress in a process. Problems begin when those different kinds of messages are made to look interchangeable. A consumer then has to guess whether it has received a durable business fact, a current data snapshot or a notification that more work may follow.

The contract reveals whether the producing context has made that distinction itself.

## A contract should leave less to infer

Clear contracts do not have to be large. They need enough language to remove the guesses that matter.

Names carry part of that work. Examples, constraints and short explanations carry the rest. A timestamp may represent business time or processing time. An event may be emitted before the wider process has finished. Neither distinction is visible from the type alone.

These details are sometimes treated as supporting documentation. I think they belong closer to the contract because they change what the contract means.

They also change what AI can reasonably produce from it. A schema with vague names gives a model structure. A schema with ownership, constraints and examples gives it a boundary.

I do not think the answer is a heavy review process. The useful habit is to read a proposed contract as language as well as an integration mechanism. I try to look at what the outside world can now see, what the names appear to promise and which conclusions a consumer could draw without access to the implementation discussion.

Most future consumers will not have been in that discussion. Neither will the AI tool pointed at the repository six months later.

## The visible model deserves care

I still care deeply about the model inside a bounded context. Poor internals eventually show up as scattered rules, inconsistent language and changes that take longer than they should.

The contract creates a different kind of consequence. It shapes how other contexts understand the boundary. Those consumers turn its names and structures into their own assumptions. AI now helps them do that faster and at a larger scale.

This makes contracts more important in AI-assisted development, although not because schemas have suddenly become architecture. Their meaning has always mattered. What has changed is the speed at which the visible model can be reused and amplified.

A clear contract gives AI something defensible to work from. A vague contract gives it room to invent coherence.

I am still unsure how much meaning belongs directly in a schema and how much needs to live in material around it. The boundary will never explain the whole context. It should at least make clear which parts of the context the outside world is allowed to believe.
