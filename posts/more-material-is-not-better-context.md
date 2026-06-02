# More Material Is Not Better Context

I keep noticing how natural it feels to solve problems with AI by adding more input and trying to explain things better to the model.

If the answer is weak, add the missing file. If the design looks shallow, add more documentation. If the model gets the behavior wrong, paste in the ticket or the API schema.

I used to do this too, but it didn't seeem to help.

There is a point where the model behaviour becomes suspicious. The prompt gets larger, the retrieval gets broader, and the answer still feels slightly off. Not obviously wrong. Just too smooth. Too willing to connect things that people in the domain would keep apart.

Maybe the AI did not need more material. Maybe it needed a smaller, cleaner world.

## When words travel too freely

Software systems are full of familiar words like "Customer", "Order", "Product" or "Account".

They look harmless because everyone recognizes them. But most systems I have worked with do not have one clean meaning for these words.

A customer in one part of the business may be the person who pays. Somewhere else it may be the person receiving the goods. In another place it may be a profile in a loyalty system.

People usually know this, at least informally. They hesitate. They ask, “Which customer do you mean?” They have learned where the traps are.

AI does not hesitate in the same way. If the material mixes several meanings of the same word, the answer may calmly use them as if they belonged together.

The output can look coherent while the model underneath has become weaker.

## Ubiquitous language gives AI something to follow

This is where I keep coming back to ubiquitous language.

Not as a DDD slogan or a glossary. It has a more significant purpose.

The useful part of ubiquitous language is that it shows up in the work. In conversations, code, tests, examples, commands, events, and documentation. The same words keep appearing because people are trying to make the model visible.

For AI, this can be and is powerful.

AI is often better at consistency than people are, once the language is clear. People take shortcuts. We keep old names around because renaming is annoying. We borrow terms from APIs, database tables, and earlier projects.

AI can help clean some of that up. It can keep generated tests aligned with the domain language. It can notice when documentation and code are drifting. It can suggest names that fit the model better than the technical names we accidentally kept.

But only if we give it a language worth following.

Otherwise it will also be consistent with the wrong thing.

## Language needs a place

Ubiquitous language does not work well as a company-wide vocabulary and that's why it's always context specific.

Complex domains usually have several local languages that overlap, collide, and evolve. The same word can have different useful meanings in different parts of the business. Sometimes that is a problem. Sometimes it is just reality.

A bounded context gives the language somewhere to live.

Inside one context, a word can become precise enough to use in code. The rules around it can be tested. The commands and events can use it. The business can recognize it. The team can change it when they learn more.

Outside that context, the same word may need translation.

This feels especially important with AI because AI tends to remove friction from language. It can make rough ideas sound finished. It can make two similar concepts sound identical. It can bridge gaps that should have remained visible.

Sometimes we need the boundary to stay visible.

## Better context shows how meaning moves

A practical bounded context already gives us clues about what AI should know.

The internal model shows how the context thinks about its own domain. The contracts show what the context exposes to others. The external models show how the context interprets things owned elsewhere.

Those separations are useful because they stop everything from becoming one model.

An external event should not accidentally become the local domain model. A public contract should not simply mirror the internal model. A database shape should not decide the language of the business.

AI can blur these lines quickly because it is good at copying structure. If two things look similar, it may treat them as the same. If an upstream schema has the fields it needs, it may reuse it directly. If an internal model is convenient, it may expose it as a contract.

The code may compile. The explanation may sound reasonable.

The boundary still got weaker.

A better AI context says: work with this internal language, publish through these contracts, treat these external concepts as translated input, and do not collapse them just because the structures look similar. Make sure the model understands what belongs to the context and what doesn't.

## The quieter failure

Hallucination gets most of the attention. I am more worried about a quieter failure.

AI can use real material and still produce the wrong model.

It can read real code, real documentation, real schemas, and real examples. Then it combines them across a boundary that should have held. No invented API. No fake requirement. No obvious nonsense.

Just a small weakening of the language.

These mistakes are harder to spot because they look productive. There is a pull request. There are tests. There is a tidy explanation and a summary of the changes done.

The damage shows up later, when the next change becomes harder to reason about.

## Designing context

There has been a lot of talk about context management and I think DDD offers a really good framework for that.

For a task inside a bounded context, I want the AI to know the local language, the concepts owned by the context, the rules that protect them, and the contracts used to communicate outward. I also want it to know what belongs elsewhere.

DDD helps here because it has always cared about language, ownership, and models that reflect a part of the world. AI makes those concerns easier to ignore, because it can produce something plausible so quickly.

But it also makes them more useful.

A bounded context gives AI a smaller world.

Ubiquitous language gives that world words it can use consistently.

The work for us is to make sure those words still belong to the people who understand the domain.

The work for us is to make sure those words still belong to the people who understand the domain.
The work for us is to make sure those words still belong to the people who understand the domain.
