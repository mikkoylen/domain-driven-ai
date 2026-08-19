# More Material Is Not Better Context

Most discussions about context for AI-assisted development focus on how to give the model more relevant material. Larger context windows help. Better retrieval helps. So does adding the file, ticket or schema that explains the missing detail.

I have approached the problem in exactly that way. When an answer was weak, I added more input. When a design looked shallow, I added more documentation. When the model misunderstood the behaviour, I pasted in the implementation or the API contract.

Sometimes that fixed the problem.

But I also reached a point where the prompt kept growing and the answer still felt wrong. It was not obviously wrong. The model had used real material and produced a tidy explanation. It had simply connected things that people working in the domain would normally keep apart.

At that point, adding another document was treating the wrong problem. The material already contained enough detail. What it lacked was a clear boundary around the meanings inside it.

## More input can weaken the answer

Adding material is a sensible response when the model lacks a fact. If it has not seen a business rule, it cannot reliably account for that rule. If it has not seen an interface, it has to guess how another system behaves.

The problem starts when relevant material comes from several different models.

A ticket may use the language of the business process. The implementation may use names inherited from an old database. An API schema may describe the same situation from the perspective of another system. Each source can be correct within its own setting.

Put them together without those settings, and the AI has to resolve the differences by itself. It tends to do that smoothly. Similar names become the same concept. Similar structures become interchangeable. Missing translations disappear into a plausible answer.

I had given the model better coverage of the system, but less certainty about what each part meant.

## Shared words hide different models

Software systems are full of familiar words such as “Customer”, “Order”, “Product” and “Account”. They look safe because everyone recognises them.

In most systems I have worked with, these words do not have one clean meaning.

A customer may be the person who pays. In another part of the business, it may be the person receiving the goods. Somewhere else, it may mean a profile in a loyalty system. None of these meanings is necessarily wrong.

People who know the domain have learned where the ambiguity lives. They hesitate or ask which customer is being discussed. Sometimes they get it wrong too, but the uncertainty is at least visible.

AI does not bring that history into the conversation unless the context contains it. When several meanings appear in the supplied material, the model may use them as though they belong together. The result reads coherently even when the underlying model does not.

That is the failure I had been trying to fix by adding more input.

## A local language gives AI something to follow

This brought me back to ubiquitous language.

I do not find it useful as a glossary maintained beside the actual work. Its value comes from appearing in conversations, code, tests, examples, commands, events and documentation. The same words recur because the team is trying to express the same model.

That gives AI a pattern it can follow. Once the language is clear, AI can be more consistent than people are. People keep old names because changing them is inconvenient. We borrow terminology from database tables, vendor APIs and previous projects. Over time, the model becomes harder to see in the code.

AI can help expose that drift. It can notice when documentation and code use different terms. It can keep generated tests aligned with the domain language. It can suggest a domain name where a technical name has survived by accident.

There is a cost to that consistency. AI will also repeat a poor language with great discipline. If the supplied material mixes several models, the model can make that mixture look deliberate.

I started to see that consistency as conditional. It helps only when the language supplied to the model is clear.

## The boundary must survive the prompt

Ubiquitous language has always been local. Complex organisations contain several languages that overlap, collide and change at different speeds. A company-wide vocabulary rarely removes that reality.

A bounded context gives one of those languages somewhere to live. Inside the context, a word can become precise enough to use in code. Its rules can be tested. Commands and events can express it. The team can adjust the model when its understanding changes.

Outside the context, the same word may need a different meaning. The crossing requires translation.

AI makes that translation easy to skip. If an upstream event already contains fields that look useful, the quickest solution is often to reuse its structure as the local model. The code is shorter and the mapping disappears. That may be a defensible choice for a simple integration, but it also lets an external model decide how the local domain is expressed.

The same thing can happen in the other direction when an internal model is exposed directly as a public contract. AI is good at copying structures, and copying removes the friction that might otherwise make the boundary visible.

A useful context therefore needs more than relevant files. It needs to say which language is local, which concepts are owned elsewhere, and where translation is expected. The model should not have to infer the boundary from filenames and package structures.

## Real material can still produce the wrong model

Hallucination gets more attention because it is easier to recognise. An invented API or requirement can be checked and rejected.

Model blending is quieter.

The AI can read real code, real documentation, real schemas and real examples. It can then combine them across a boundary that should have held. Nothing has been fabricated. There may be a pull request, passing tests and a clear summary of the change.

The weakness appears later. The next change needs another exception. A local rule depends on an upstream representation. A familiar word now has two meanings in the same codebase. Each step was reasonable in isolation, but the model has become harder to reason about.

I find this more worrying than an obviously bad answer. It looks like productive work while it is happening.

## A smaller world is often better context

DDD gives me a practical way to think about AI context because it already deals with language, ownership and models that describe only part of the world.

For work inside a bounded context, I want the AI to know the local language, the concepts owned there and the rules that protect them. It should see how the context communicates with its surroundings. It should also know which concepts belong elsewhere.

I do not think every AI task will fit neatly inside one boundary. Architecture work, integration work and business processes often cross several of them. The crossing still needs to be explicit. The model should know when it is translating instead of quietly treating several languages as one.

I am not yet sure how much of this can be enforced through tooling and how much depends on teams keeping their models healthy. Contracts, evidence and ownership will all matter, but each raises a separate set of problems.

For now, when an AI answer feels smooth but slightly wrong, I no longer assume that the prompt is missing material. I first look for the boundary that the material has hidden.
