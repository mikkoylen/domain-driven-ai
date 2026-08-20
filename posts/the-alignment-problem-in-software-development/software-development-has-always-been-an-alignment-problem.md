# Software development has always been an alignment problem

The phrase alignment problem is now strongly associated with AI safety. In that discussion, the concern is whether an AI system behaves consistently with human values and intent. That is not the subject of this series.

I am interested in an older problem inside software organisations. A business has an intent. Management turns that intent into strategies, goals and plans. Teams turn those goals and plans into software. Customers and operational systems then respond to what was actually built, which may be quite different from what anyone originally intended.

Software organisations have spent decades trying to improve this chain. Scrum brought teams closer to customers. DevOps brought development and operations closer together. Lean challenged work that did not create value, while product operating models asked teams to own outcomes rather than deliver projects. I have worked in organisations using parts of most of them, and each has helped with a real problem.

The discussion around them often becomes a choice between methods. One organisation needs more product thinking, another needs better flow, and another needs clearer architecture. Those diagnoses can all be correct. I think they are also different expressions of the same underlying problem. The alignment problem.

Somebody has an intent, somebody else must act on a representation of that intent, and meaning can change between the two. Misalignment occurs when the resulting behaviour satisfies the representation but misses the original outcome it was meant to produce.

## Methods solve a problem they cannot remove

The useful parts of any given methodology are usually mechanisms for improving alignment. A sprint review lets a team compare what it built with what customers or stakeholders expected. A cross-functional team reduces the number of handovers between analysis, development, testing and operations. These mechanisms can genuinely help when the conditions they assume are present.

The trouble begins when the mechanism travels without the conditions that made it work. Scrum assumes that a team can learn from an increment and change what it does next. DevOps assumes that the people changing a system can observe and influence its production behaviour. Outcome ownership assumes that a team can see an outcome and has enough authority to respond to it.

An organisation can adopt the visible parts of a methodology while leaving its underlying assumptions unmet.

I have seen teams hold every expected ceremony while all the decisions had already been made months in advance. Releases still passed through a separate gate, and success still meant completing the planned scope rather than producing a meaningful change. The ceremonies made coordination more orderly, which was genuinely useful. They did not give the team a way to question whether the work was still the right work.

Replacing the framework would not have fixed that. The names and meetings would have changed, while the authority, information and incentives underneath them remained the same.

## Intent is lost one translation at a time

Recently I was looking at a product organisation with more than ten developers split across three squads. They served multiple business areas through one shared digital channel. At the strategic level, the intent sounded reasonable, improve the profitability and quality of digital purchasing.

By the time that intent reached a development team, it had usually become a collection of features. The features became backlog items, the backlog items became code, and the code had to pass testing and a release process. Each step made the work more concrete. Each step also made the original intent a little harder to see. The process provided no feedback that could challenge the original assumptions.

No single translation looked obviously wrong. A strategy has to lead to choices. A team needs to break the work into pieces small enough to implement effectively. Testers need observable acceptance criteria, and a release process needs a way to decide whether a change is safe.

The drift appeared in what those translations left behind. A feature could enter the backlog without a baseline for the behaviour it was meant to change. Acceptance criteria could prove that the requested interaction existed without showing whether customers could use it successfully. A release could be declared complete when it reached production, even though nobody had decided when and how its effect would be evaluated.

Everyone could attend the same planning session and receive the same slide deck. That did not prevent the loss. The strategy, backlog item and test case served different decisions, so each preserved a different part of the original meaning.

Misalignment can grow even if everyone understands every word they are given.

## Local success can make the whole worse

The organisation could produce good software. Its mobile application had high customer satisfaction, and its user base was growing steadily. Experienced developers cared about quality, and a manual testing stage caught defects that would otherwise have reached customers.

The process was built to protect customers. That is what it did.

The same process meant that the fastest changes took about a month from merge to production. Larger changes could take more than six months. Because releases were expensive, teams accumulated more changes in each one. Because each release contained more changes, it required broader testing and carried more risk.

Every local decision was defensible. Developers completed their tickets. Testers reduced escaped defects. Projects controlled scope. Looking at the day-to-day work, it all seemed fine. The larger system still became worse at discovering whether it was building the right thing.

A scope metric would have hidden that problem. It could show steady progress while customer assumptions remained untested for months and each release increased the blast radius of a wrong decision.

Misalignment does not require incompetence or neglect. Local rationality is enough.

## Alignment does not require one shared model

When this problem becomes visible, the natural response is to ask for stronger alignment. That can quickly become more central planning, common terminology and another coordination layer. I do not think uniformity is the answer.

A strategy group, a product team, a payment service and an operations function make different decisions. They need different representations of the same organisation.

The same issue appears in software architecture. A Customer context may define a customer as a person with a loyalty relationship. Ordering may care about the party placing and paying for an order. Forcing both into one enterprise `Customer` model creates apparent consistency, but it hides a difference that matters.

The models must connect. They do not have to be identical.

This is one of the ideas I explored in the [Domain-Driven AI](https://blog.unplugit.fi/series/domain-driven-ai) series. Bounded contexts give language and meaning a place to belong, rather than asking the entire organisation or codebase to share one model.

I think organisational alignment needs a similar idea. The question is not how to make every team think alike. It is how to let teams make different local decisions without losing their connection to a wider intent.

That shifts the problem towards boundaries and authority. It also creates the first question for the rest of this series, which decisions should a team own, and where should it have to stop and involve another owner?

## AI makes ambiguity executable

AI enters this series as a possible aid to alignment in software organisations, not as the subject of the AI safety alignment problem. It matters because it can act on unclear knowledge much faster than people usually can. That speed works in both directions.

A person can read a strategy promising seamless customer experiences and recognise that it gives little guidance for a concrete trade-off. An AI system can quickly turn the same words into goals, plans, backlog items and code. The result may look coherent even though the missing decision is still missing.

I have seen the software version of this with coding agents. Given a request to expose a customer's loyalty level in an order flow, an agent can find an existing customer type, update the endpoint and produce passing tests. It has followed the strongest evidence available in the repository. It may also have crossed an ownership boundary and turned an internal model into a public contract.

Developers make the same shortcut. I certainly have, especially when creating a lot of code to keep models separate felt disproportionate to a small change. AI changes the scale of the problem. It can carry one unclear assumption through twenty files before anyone has understood the assumption it is spreading.

The same capability could help in the other direction. AI can point out that a strategy contains no measurable outcome, that two parts of the organisation use `customer` differently, or that a backlog item has no visible connection to its stated purpose. It can make a missing decision visible and return it to the people who own it.

That is the opportunity I want to explore. AI makes ambiguity executable. Used carefully, it may also make ambiguity harder to hide.

## Alignment has to remain unfinished

A more detailed chain of instructions from executives to code would not remove this problem. A precise metric can become a target. A clear contract can preserve the wrong meaning perfectly. Some errors only become visible after people and software act together in the real world.

This leaves three questions for the rest of the series.

The first is where decisions should live, and how a team can have real autonomy without quietly taking authority that belongs elsewhere.

The second is how strategies, goals, facts and decisions can cross organisational boundaries without becoming anonymous text that AI can interpret however it likes.

The third begins when production evidence contradicts the original plan. Feedback only matters if it can travel back far enough to change a team decision, a metric or even the strategy itself. That learning loop has to become the organisation's self-correcting mechanism.

I do not yet know what a complete AI-assisted version of this system looks like, or where assistance quietly becomes authority. I am more certain about the underlying problem. Software development has always depended on imperfect representations of intent, and faster execution does not make those representations more truthful.
