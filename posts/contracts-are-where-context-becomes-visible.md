# Internal, External, and Contract Models Should Not Be the Same Thing

Discussions about model separation often end up being discussions about boilerplate. One object arrives at a service boundary, most of its fields are copied into another object, and a similar conversion happens again before data leaves the service.

I have never particularly enjoyed writing that code. I have also taken the shortcut of using one model throughout an application. In a small or short-lived system, that can be a defensible decision.

The trouble usually appears later.

The same class gradually becomes an API request, an event payload, a persistence structure, and the internal representation of the domain. A field cannot be renamed because an external consumer may depend on it. An upstream system introduces a nullable value, so nullability spreads into business logic. Domain objects collect serialisation annotations because they also need to cross a technical boundary.

At first, one model looked simpler. Eventually, every change seemed to affect something it should not.

This is where I think the usual argument about duplicate fields misses the point. The important difference between these models is not their structure. It is who owns their meaning and why they need to change.

## Similar structure hides different responsibilities

Software models are easy to compare by looking at their names, fields, types, and relationships. When two classes contain nearly the same data, the duplication is obvious. Their different responsibilities are less visible.

Consider a product.

The product context may own descriptions, classifications, lifecycle state, packaging information, and sales restrictions. An ordering context may only need to know whether an item can currently be sold through a particular channel.

Both contexts might use the name `Product`. Their models might share an identifier and a few fields. They still do not represent the same understanding.

The product context owns the model it uses to manage products. The ordering context owns the smaller interpretation it needs to make ordering decisions.

I have found ownership more useful than structural similarity when deciding whether models should be shared. The relevant questions are who decides what a field means, which decisions the model supports, and what can cause it to change.

Two models can look identical today and still need the freedom to become different tomorrow.

## The internal model needs room to change

The internal model belongs to the bounded context. It contains the language and rules the context needs to do its work. Its shape should follow the domain rather than an API framework, database library, or message serializer.

I do not see a domain model as a finished description of reality. It is closer to a working theory.

A team models what it currently understands and builds around it. Later, the work exposes mistakes. A boolean turns out to be the result of a policy. One entity turns out to contain two concepts that happen to share an identifier. A field that once appeared important has no part in an actual business decision.

The model should be allowed to follow that learning.

That becomes harder when external consumers depend on the same structure. An improvement to the internal model becomes a contract change. The team starts preserving concepts that no longer fit because removing them might break somebody else.

Transport concerns arrive by the same route. Fields become nullable because an API permits them to be absent. Generated types enter business logic because using them avoids a conversion. Each decision looks harmless on its own.

Together, they make the internal model describe the history of its integrations instead of the context's current understanding.

## Both sides of a boundary need translation

The same problem appears when data enters a context.

A generated API client or event class already contains the required fields, so passing it directly into application and domain logic feels efficient. No local type is needed. No mapping needs to be maintained.

It also allows the producer's model to enter the consumer unchanged.

The producer owns the facts it publishes. It does not own what those facts mean inside another context. That interpretation belongs to the consumer.

If a customer context publishes a large `CustomerUpdated` event, an ordering context may only care about the customer identifier and whether ordering is currently allowed. Copying the full customer structure does not give the ordering context a better model. It gives it more upstream changes to absorb and more concepts that have no role in ordering.

A consumer-owned external model can be smaller. It can use local language, combine information from several messages, or reshape an upstream value into something useful for a local decision. This is the anti-corruption layer in a fairly ordinary form: a deliberate translation at the boundary.

The contract model has a related but different responsibility. It defines what the context intentionally accepts from or promises to others. A contract may remain stable while the internal implementation changes substantially. It may reveal less than the context knows, or preserve an older representation because compatibility matters more than internal elegance.

Publishing an internal entity directly makes all of its visible details available for other systems to depend on. A field may be stored by consumers. Its absence may acquire meaning. Its name may enter the language between teams.

Once that happens, it is no longer an implementation detail, even if nobody meant to publish it.

## Mapping makes hidden decisions visible

Separating models creates mapping code. Sometimes it creates a surprising amount of it.

That cost is real. Several identical classes do not improve a design merely because they sit in different packages.

Still, I have become less convinced that mapping is only boilerplate. A mapping decides what a missing value means. It decides whether an external status maps directly to an internal state or contributes to a policy decision. On the way out, it decides which internal details are part of the promise made to consumers.

Those decisions exist even when no mapper exists.

Without an explicit translation, they tend to hide in deserialisation settings, nullable domain fields, convenience methods, and assumptions scattered through handlers. The code is shorter, but the translation has not disappeared. It has become harder to find.

I would rather see that decision in one slightly boring function than discover parts of it across five places in the application.

Some duplication remains wasteful. If two models have the same owner, express the same meaning, and change for the same reasons, separating them achieves little. But when those conditions differ, repeated fields can be cheaper than shared meaning that nobody quite owns.

## Separate models narrow the security surface

Model separation also gives security decisions a visible place.

Suppose an internal object gains a field such as `approved`, `riskLevel`, `accountRole`, or `manualReviewRequired`. If the object also serves as an API request, a caller may suddenly be able to provide a value that was intended to be controlled only inside the context.

The value may deserialize correctly. It may even pass format validation. The problem is that the caller should never have been able to set it.

An explicit input contract defines the part of the internal state that external input is allowed to influence. It does not replace validation or authorisation, but it makes the accepted surface deliberate.

The same risk exists on the way out. Internal models accumulate information that should remain inside the context: personal data, fraud indicators, operational flags, access decisions, pricing details, or intermediate policy results.

When an outbound contract is assembled explicitly, exposing a new field requires a mapping change. When an internal object is serialised directly, exposure can happen as a side effect of an unrelated domain change.

Separate models do not make a system secure by themselves. They create a boundary where influence and disclosure can be reviewed.

## AI makes model separation cheaper

AI-assisted development makes the shortcut easier to take.

From the code alone, reuse often looks clean. Similar fields suggest one type. Generated clients already contain the required data. An internal entity can already be serialised. The result has fewer classes, fewer mappings, and fewer tests.

I cannot really blame the tool for following what is visible.

Structural similarity is visible in code. Ownership usually is not. A coding agent cannot infer from field names that one model is an external promise, another is a consumer-owned interpretation, and a third needs to evolve with the domain. It will not know that a new internal field is sensitive unless that constraint appears somewhere it can inspect.

Developers have been collapsing models for decades. AI only makes the choice faster and easier to repeat across a codebase.

It also makes deliberate separation cheaper. Once the responsibility of each model is clear, AI can generate types, mapping functions, validation, conversion tests, and serialisation tests. It can update adapters when a contract changes. Architecture checks can keep generated client types and contract types out of the domain model.

The difficult decisions remain human ones. Structural similarity does not reveal what an external value means inside the domain. It does not decide which fields a caller may influence or which internal details are safe to publish.

The repository therefore needs to make the intended flow visible. Package and module boundaries can distinguish published contracts, consumer-owned external models, and the internal domain. Dependency rules can require translation before information enters or leaves domain logic. Short documentation can explain who owns each model and why it exists.

The exact package names matter less than the rule they make visible.

I still do not like mapping code very much. But AI has weakened the practical argument for avoiding it. The mechanical work has become cheaper, while the cost of accidental coupling and accidental disclosure has not.

That does not mean every service needs three versions of every object. I still do not know where the separation stops being worth its cost. In a small application with one owner and one reason for change, a shared model may be entirely reasonable.

The distinction becomes important when ownership, meaning, security exposure, or reasons for change begin to diverge. At that point, the models should be allowed to diverge too.

AI can help maintain that boundary. It cannot decide that the boundary matters unless the architecture makes the decision visible.
