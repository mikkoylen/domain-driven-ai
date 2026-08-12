# AI-assisted development needs boundaries

Most writing about AI-assisted development seems to settle into three positions. There is the productivity account, where an agent is judged by how quickly it turns a ticket into code, the prompting account, where better instructions are expected to produce better engineering, and the safety account, where review and tests catch whatever it gets wrong. I recognise something useful in all three, but none quite describes the concern I have run into when a change reaches across a system.

The code usually works. I have watched an agent satisfy a request by reusing a type from another domain, passing the same model from an incoming API to persistence, and adding a field to a published response because the internal object already contained it. The implementation was efficient, the tests were green, and the architecture was a little less honest afterwards.

Developers make the same compromises. A developer who has spent two years in a system may remember why two similar `Customer` classes were kept separate, or why a reachable table belongs to another team. The agent sees imports, public methods and repeated patterns. When a repository contains the intended architecture and five years of exceptions, the exceptions may be its clearest instructions.

What concerns me is therefore less about whether an agent can generate a change and more about what keeps that change inside the part of the system where it belongs. That leads back to bounded contexts, published contracts and ownership, which are older concerns than generative AI. AI has merely increased the speed at which an unclear boundary can be crossed.

## The repository teaches the shortest path

A coding agent learns much of the architecture from the code around a task, which makes the repository both its most useful source of context and its most misleading one. If five endpoints return persistence objects directly because the first was written under time pressure, a sixth implementation has strong evidence that this is the house style, even when the team has spent a year separating API, domain and persistence models.

The agent can find conventions nobody documented and reproduce a Spring Boot integration pattern without needing every decision restated in a prompt. I have benefited from that pattern recognition where the ceremony is predictable and the design has already been decided.

The cost is that frequency starts to look like intent. A direct database read added during an incident or an internal DTO leaked into an OpenAPI response can appear more authoritative than an architecture diagram nobody updates. The flattering account is that the agent understands the codebase, the more accurate one is that it understands what the codebase repeatedly permits.

A bounded context reduces that ambiguity by saying where a model applies, who owns it and how its meaning is published. A box labelled `Ordering` in a diagram, however, is not yet a development constraint.

## A bounded task changes the answer

I have used a deliberately ordinary example when testing this distinction, adding a customer's loyalty level to an order flow. With only that sentence, an agent has several plausible routes, it can import the customer context's internal `Customer` type, query the customer store or add another shared model. Every route can produce the requested field.

The answer changes when the task states that Ordering owns the order, Customer owns loyalty status, and Ordering may learn about that status only through a published contract. Ordering can consume a customer event, keep a consumer-owned projection containing the few facts it needs and translate those facts into its own language. A missing field in the event then becomes a visible contract question rather than permission to reach into somebody else's data.

This creates more types. The external model records what Ordering receives, the internal model what an order means, and the contract model what Ordering publishes. Before coding agents, I sometimes accepted one Kotlin data class flowing through all three roles because the mappings and tests felt disproportionate to a small change.

AI has weakened that excuse. It can generate the types, mappings and tests, leaving the design decision exposed instead of hiding it behind boilerplate. The separation also limits accidental data exposure, because an internal fraud indicator cannot drift into a public response merely because the serializer found it. Generated mapping code is cheap, an accidental contract is not.

## Written guidance needs hard edges

My first attempt to provide architectural context was one large Markdown file at the root of the repository. It became less useful as it accumulated unrelated instructions. A narrower hierarchy has worked better, the repository describes the context map, each bounded context records its language, owned data and contracts, and module-level notes contain genuinely local constraints.

This material helps both agents and people entering the codebase, but it creates another synchronisation problem. A sentence saying that Ordering never imports Customer internals has little value once the Gradle dependency graph allows it and six months of code demonstrate otherwise.

I treat written guidance as an explanation of intent, while deterministic checks provide the edge. ArchUnit can reject forbidden dependencies, OpenAPI and AsyncAPI schemas can validate what crosses a boundary, and contract or security tests can expose incompatible and accidental changes. None understands the domain, but each can preserve an accepted decision after the conversation that produced it has been forgotten.

AI can help maintain these constraints, generate a dependency rule from an established policy, add contract tests alongside a schema change, or point out where the context description and the code disagree. It should not quietly reconcile that disagreement, because choosing which side is correct is an architectural decision.

## Capability has grown faster than authority

A coding agent can inspect a repository, edit twenty files and run a Gradle build while I am still holding the requirement in my head. That reach is useful inside one bounded context. Across Customer, Ordering and Payments, it can hide three ownership decisions inside one coherent-looking change.

A consumer can explain that it needs a new fact and propose an event field, but it does not get to redefine the producer's language or update its database. An agent working for the consumer has no reason to receive more authority than the team that asked it to act.

This is why file count is a poor proxy for risk. A twenty-file mechanical refactoring behind an existing contract may preserve meaning, while a one-line addition to an AsyncAPI schema may publish a new domain fact to every consumer. The second change looks smaller and may deserve the larger conversation.

Respecting ownership means that work sometimes stops at a boundary. The agent may produce a contract proposal instead of a completed feature, leaving another team to decide whether the fact belongs in its published language. That delay can be frustrating, but it makes a hidden decision visible. An agent being able to edit a file does not mean the task gave it the right to change what that file means.

## Bounded contexts keep speed contained

AI has made some kinds of coding enjoyable for me again after more than twenty years of development work, particularly when repetitive implementation no longer interrupts the design. It has also made it possible to produce a large change before I have formed a reliable mental model of everything it touches. Faster generation increases the value of a smaller blast radius.

That is where this series has led me. More material was not better context until it was scoped. AI could not become the source of truth because the truth still needed an owner. Contracts made a context visible at its edges, while separate external, internal and contract models kept one meaning from sliding into another. Each argument depended on the same underlying idea, bounded contexts really do matter, perhaps more when an agent can move through a codebase faster than the people responsible for it.

The useful goal is therefore not only to help an agent understand a bounded context, but to make it work within one. The context should define the language it may use, the data it may change, the contracts through which it may collaborate and the decisions that require another owner. Repository guidance can explain those limits, and executable architecture rules can make the agent enforce them while it works.

A good agent-assisted change should be able to move quickly inside an established boundary and become deliberately slower at its edge. It should generate the mappings instead of collapsing the models, add the contract checks instead of bypassing the contract, and stop with a proposal when the next step belongs to another context. That does not remove architectural judgement, but it keeps the consequences of one task from spreading silently through the system.

A well-enforced boundary can still preserve the wrong model, and domain understanding still changes through conversations and production evidence. I do not yet know how much of that judgement coding agents will eventually share. For now, I want them to help enforce the boundaries we have deliberately chosen, not erase those boundaries simply because generating the larger change has become easy.
