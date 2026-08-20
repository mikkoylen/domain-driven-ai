# Internal, External, and Contract Models Should Not Be the Same Thing

I have never particularly enjoyed writing mapping code.

It often feels like work that should not be necessary. You receive an object, copy most of its fields into another object, and then do roughly the same thing again before sending data outside the service.

Sooner or later, someone asks the obvious question.

Why do we have three models that look almost identical?

Why not use one?

I understand the temptation. I have taken that shortcut myself. Sometimes it may even be a reasonable decision, especially in a small and short-lived application.

But I have also seen what happens when the same model gradually becomes an API request, an event payload, a persistence structure, and the internal representation of the domain.

At first, the code looks simpler. Later, every change seems to affect something it should not.

A field cannot be renamed because an external consumer might depend on it. A domain object starts collecting serialization annotations. An upstream system introduces a nullable value, so nullability spreads into business logic. Internal restructuring becomes contract evolution.

The models looked the same, so we treated them as the same thing.

They were not.

## Similar fields can hide different responsibilities

Software models are easy to compare structurally.

They have names, fields, types, and relationships. When two models contain the same data, the duplication is obvious. The difference in meaning is harder to see.

Consider a product.

A product context may own a rich model containing descriptions, classifications, lifecycle state, packaging information, sales restrictions, and other details needed to manage products.

An ordering context may only need to know whether an item can currently be sold through a particular channel.

Calling both objects `Product` does not make them the same model.

One describes how the product context understands and manages a product. The other describes the small part of that information another context needs to make its own decisions.

Their structures may overlap. Their reasons for existing do not.

I find that ownership is usually a better way to understand this than structure.

Who owns the meaning of the model? Why does it change? Who is allowed to decide what a field means?

Once those questions are asked, the separation becomes easier to justify.

## The internal model reflects local understanding

The internal model belongs to the bounded context.

It contains the concepts, rules, and language the context needs to do its work. Entities, value objects, aggregates, policies. Whatever form the model takes, it should be shaped by the domain rather than by the preferences of an API framework, database library, or message serializer.

I do not think of a domain model as a finished description of reality. It is closer to a working theory.

We model what we currently understand, build something around it, and eventually discover where the model is incomplete or simply wrong. Then we change it.

Perhaps a boolean turns out to be the result of a policy. Perhaps one entity is actually two different concepts that happen to share an identifier. Perhaps some piece of data we considered important has no role in any real business decision.

The internal model needs room to change as that understanding develops.

That becomes difficult when it is also a public contract.

If external consumers depend directly on the internal model, every improvement becomes an integration concern. The team starts preserving structures that no longer fit the domain because changing them might break somebody else.

Over time, the internal model stops reflecting the team’s current understanding. It starts reflecting the history of everything that has ever been exposed.

Transport concerns leak in as well. Serialization annotations appear on domain objects. Fields become nullable because a particular API version allows them to be absent. Framework-generated types enter business logic because using them avoids another conversion.

None of these decisions seem especially harmful on their own. Together, they shape the model around its technical surroundings instead of the domain.

## External data needs a local interpretation

The same issue appears on the other side of the boundary.

A context consumes an event or calls an API owned by another context. A generated client already contains all the necessary types, so those types are passed directly into application and domain logic.

Very efficient. No mapping required.

It also means the producer’s model has entered the consumer.

The producing context owns the facts it publishes. It does not own how another context interprets those facts. That interpretation belongs to the consumer.

This is why I think an external model should be consumer-owned.

It can be smaller than the upstream contract. It can rename concepts where the local language differs. It can reshape information into something more useful locally. It may even combine information from several upstream messages.

That does not corrupt the producer’s model. It acknowledges that two bounded contexts do not see the world in exactly the same way.

Suppose a customer context publishes a large `CustomerUpdated` event. An ordering context may only care about the customer identifier and whether the customer is currently allowed to place an order.

Replicating the entire customer structure does not make the ordering context more informed. It gives it more fields to store, more upstream changes to absorb, and more concepts that do not belong to its own domain.

A local external model makes that dependency explicit. It says: this is the part of the upstream information that matters here, and this is how we interpret it.

That is the anti-corruption layer in a fairly ordinary form. Not a grand architectural mechanism. Just a deliberate translation at the boundary.

## Contracts are promises, not exported objects

The contract model faces outward.

It contains the commands, events, API schemas, and DTOs a context deliberately exposes. These models describe what other systems are allowed to depend on.

That gives them a different responsibility from the internal model.

A contract may need to remain stable while the implementation changes substantially. It may expose less information than the context holds internally. It may preserve an older representation because compatibility matters more than internal elegance.

Most importantly, every field in a contract should be there intentionally.

Publishing an internal entity directly is attractive because the object already exists. No additional model. No mapping. Less code.

But an internal entity contains details designed for internal behaviour. Once those details are exposed, they become part of somebody else’s understanding of the context.

A field that seemed harmless may be stored by consumers. Its absence may be interpreted as meaningful. Its name may become part of the language used between teams.

At that point, it is no longer an implementation detail, regardless of whether anyone intended to make it public.

A contract should therefore be designed as a contract. It should not be produced by serializing whatever object happens to be available at the end of a transaction.

## Mapping makes the translation visible

Separating the models means writing mappings.

External data is translated before it enters the internal model. Internal state is translated before it becomes an event, API response, or another published contract.

This adds code. Sometimes a surprising amount of it.

Still, I have become less convinced that this code is merely boilerplate.

A mapping decides what a missing value means. It decides whether an external status corresponds directly to an internal state or only contributes to a policy decision. It decides which internal details are safe to publish and how a local concept should be expressed to outsiders.

Those decisions exist whether we write a mapper or not.

Without an explicit mapping, they tend to hide in deserialization settings, nullable domain fields, convenience methods, and assumptions scattered through handlers.

The code is shorter, but the translation has not disappeared. It has become implicit.

I would rather see that translation in one slightly boring function than discover it later across five different parts of the system.

Duplication is still a cost. Creating several identical models without a clear reason does not improve the design.

But when models belong to different owners and change for different reasons, a few repeated fields may be the cheaper option.

The alternative is shared meaning that nobody quite owns.

## Model boundaries are also security boundaries

There is another reason I have become cautious about passing models directly across boundaries: security.

Every input needs validation anyway.

We need to decide which fields are accepted, whether their values are valid, and whether the caller is allowed to request the change. Reusing an internal model does not remove that work. It mostly makes the accepted surface less explicit.

This can become dangerous as the internal model evolves.

Imagine that an internal object gains a field such as `approved`, `riskLevel`, `accountRole`, or `manualReviewRequired`.

If the same object is also used as an API request, a caller may suddenly be able to provide a value that was only meant to be controlled internally.

The request may deserialize correctly. It may even pass basic format validation.

The problem is not that the value is malformed. The caller should never have been allowed to set it.

This is sometimes described as mass assignment or over-posting, but the underlying issue is broader. The external input model has not clearly defined which parts of the internal state may be influenced from outside.

The same risk exists in the other direction.

When an internal object is serialized directly into an API response or event, every new field becomes a potential addition to the published contract.

Most additions will probably be harmless. Some will not.

Internal models often accumulate information that should remain inside the context: personal information, fraud indicators, internal identifiers, operational flags, access decisions, pricing details, or information about how a business rule was evaluated.

If the published contract is assembled explicitly, exposing that information requires a deliberate mapping change.

If the internal object is published directly, the exposure may happen as a side effect of an unrelated domain change.

I think this is an important part of the case for separate models. The mapping does more than translate between meanings. It controls which information is allowed to cross the boundary.

For inbound data, the contract acts as an allow-list for external influence.

For outbound data, it acts as an allow-list for disclosure.

Separate models do not replace validation, authorization, or data classification. But they give those responsibilities a visible place.

Without that separation, security depends too much on developers remembering which internal fields happen to be safe every time the model changes.

## AI will usually take the shortcut

AI-assisted development makes this more interesting.

Without clear instructions, an AI coding assistant will usually take the obvious shortcut.

If an API request and an internal object contain similar fields, it may use the same type. If a generated event class already contains the data needed by the domain, it may pass that object through directly. If an internal entity can be serialized, creating a separate response model may look unnecessary.

I cannot really blame the tool for this.

From the code alone, reuse often appears to be the cleanest solution. Fewer classes. Fewer mappings. Fewer tests.

The ownership boundary is not obvious unless we make it obvious.

An AI tool can see structural similarity. It cannot determine from field names alone that one model is an external promise, another is a consumer-owned interpretation, and a third is part of a domain model that needs to evolve independently.

It will also not automatically know that a new internal field is sensitive or that an external caller must never control it.

Unless those constraints are visible in the architecture or stated in the instructions, the models simply look redundant.

This is one of the risks I keep noticing with AI-generated code. It is very good at following visible similarities. Architectural responsibility is often much less visible.

A generated change can be clean, consistent, and well tested while still weakening a bounded context or exposing information that was never meant to cross it.

There is nothing uniquely wrong with AI here. Developers have been collapsing models for decades. AI just makes the shortcut faster and easier to apply across a larger codebase.

## AI also makes separation much cheaper

The opposite is also true.

Once the architectural rule is clear, AI is very good at preserving the separation.

Tell it that inbound contracts, external projections, internal models, and outbound contracts must remain distinct, and it can generate much of the work that used to make this design feel expensive.

It can create the types and mapping functions. It can add input validation, conversion tests, and serialization tests. It can handle optional values deliberately instead of allowing nullability to drift into the domain.

It can generate explicit outbound mappings that expose only approved fields. It can compare contract versions and identify which adapters need attention. It can add architecture checks that prevent contract or generated client types from entering the internal model.

When a contract changes, it can update the affected translation layer without pushing the change through the whole application.

This changes the practical trade-off.

For years, one objection to separate models has been the amount of repetitive code involved. That objection made some sense when every DTO, mapper, validation rule, and test had to be written and maintained manually.

It makes much less sense when AI can produce most of that code quickly.

There is still real design work involved.

AI cannot decide from structural similarity what an external value means inside the domain. It cannot decide which fields a caller should be allowed to influence or which internal details are safe to publish. It cannot determine whether two concepts with the same fields actually represent the same thing.

Those are domain, ownership, and security decisions.

But once people have made those decisions, much of the remaining implementation is mechanical.

That is exactly the kind of work AI handles well.

So why take the shortcut?

If explicit models protect domain autonomy, make validation clearer, and reduce the risk of leaking internal information, then using one model everywhere is difficult to justify merely because mapping is tedious.

AI can be used to erase the boundary. It can also be used to make the boundary cheap to maintain.

The result depends heavily on the instructions and architectural constraints we give it.

## Make the intended model flow obvious

It is not enough to tell an AI assistant to respect DDD or preserve bounded contexts. Those instructions are too broad to guide an individual code change.

The intended flow should be visible in the repository.

For example:

* `contracts` contains the models published by the context
* `external` contains local projections of information received from elsewhere
* `internal` contains the domain model
* external models are always mapped before entering domain logic
* internal models are always mapped before crossing the context boundary

The exact names are not important. The separation is.

Module dependencies, package visibility, architecture tests, naming conventions, and code review rules can reinforce it. A short piece of documentation can explain why the models exist separately and who owns each one.

Security constraints can be stated in equally practical terms:

* input contracts contain only fields callers may provide
* input mapping performs validation and authorization before changing state
* internal types are never accepted directly at external boundaries
* outbound contracts contain only intentionally published fields
* internal objects are never serialized directly into external responses or events

These are useful instructions for developers. They are also useful instructions for AI.

When architectural intent is encoded in forms the tool can inspect, AI can follow it consistently. When the intent exists only in someone’s head, the tool fills in the gaps using common programming habits.

Those habits usually favour reuse.

Reuse is visible. Ownership is not.

## A little duplication can preserve a lot of freedom

I still do not like mapping code very much.

But I dislike accidental coupling and accidental data exposure more.

Internal, external, and contract models may sometimes look almost identical. That alone is not a strong reason to combine them.

A better test is whether they belong to the same owner, express the same meaning, expose the same security surface, and are expected to change for the same reasons.

Often they do not.

The internal model expresses how the context currently understands its domain.

The external model expresses how it interprets information from somewhere else.

The contract model expresses what the context intentionally accepts from or promises to others.

Keeping those roles separate creates some duplication. It also allows each model to evolve without quietly dragging the others along.

AI changes the economics of that choice. It makes shortcuts cheaper, but it also makes explicit translations cheaper. We can use it either to remove the boundary or to maintain the boundary with much less effort.

I think the second use is more valuable.

Not because every service needs three versions of every object. It does not. But when the meanings, owners, security concerns, and reasons for change are different, the models should be allowed to be different too.

The implementation work is no longer a convincing excuse.

The harder part is making the boundary deliberate—and making it visible enough that both people and AI keep it intact.
