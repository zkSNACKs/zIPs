# Wasabi Product Development

## Overview

We propose a system to help prioritize the product development without a static hierarchy, allowing the number of contributors to scale without adding too many inefficiencies or formal authority.

## Motivation

Which feature to build next? How does it look like? How do we make sure our technical power users like it without confusing the noobs we target in the first place? Who gets to make a decision on that? How often?

Recent conversations have happened around potential redesigns for product features, such as the coinjoin status “music” box or unearthing buried features such as coin control. This pushed the team to think about the reasoning behind it and discuss how features are prioritized in Wasabi 2.0.

- There seems to be a lack of precise consensus around the type of end user, which can hamper the team’s ability to know what features to work on. _"For grandmas and mainstream users"_ don't cut it.
- Copywriting and UI/UX are primary concerns to make a great product experience. That requires a precise understanding of the users, which goes beyond just writing software.
- Inconsistencies in the user journey confuse less technical users who are the primary target of WW2 and may be put off by privacy wallets in general.
- Listening to user feedback can be useful but a great care should also be applied to filter vocal power users who are often technical may lead the team in the wrong direction, forcing U-turns later on.

### _"People don't want a quarter-inch drill, they want a quarter-inch hole."_

Meetings currently exist for discussing implementation details but less so for prioritization of features, which can lead to miscommunication, redesigns and inefficiencies. Could it be done better? Probably. This is what this proposal is about. This could allow Wasabi to scale to 100+ contributors and further reduce the need for authority by having a clear system of prioritization.

We hope it is not taken as an authoritative voice telling contributors how to work but as lessons shared from past external experiences after observing how zkSNACKs function today.

## Implementation

### 1. Identify a Product "Owner"

Choose a product owner to be the voice of the product roadmap working with the management team, users, bitcoin communities, and other stakeholders to define what problems should be solved. Product owners should manage the product backlog and keep it updated to help with prioritization of feature requests and bugs fixing. He focuses on the “why” and “what”, not the “how”.

How can a product owner fail? Underpowered (cannot make decisions without external approvals), overworked (dealing with too many projects outside of product), distant or proxy (replacing overworked leading to miscommunication and weak decision making) and committee (group of people without anyone in charge leading to endless meetings, frustration and inefficiencies). _I believe the Wasabi team does it all more or less regularly._

One person should be in charge to lead product direction, backlog prioritization and planning meetings with scrum master, communicating clearly to the team. He should be close to being 100% dedicated or find someone else who can. This is pretty much done by Max, which he does very well it seems, though he is involved on many different projects.

### 2. Identify a "Scrum Team"

The scrum team is self-managing and responsible for developing functionality. It decides how to turn the product backlog into features by working on actionable tasks and issues. The team needs to understand the product vision, have a holistic view of the backlog and understand the end users really well. There should not be set roles as everyone should be free to choose what to work on based on personal interests and skills. _This is already pretty much true for Wasabi and seems to be running quite well._

### 3. Identify a "Scrum Master"

Choose a scrum master to be the technical feasibility lead, the coach and facilitator of the development “scrum” team to help with implementation, feature scoping, work breakdown and to avoid scope creep from the product owner. He focuses on the “how” and protects the scrum team from external noise from the product owner, internal stakeholders or external users. He organizes sprint planning, sprint review and retrospective meetings as well as holds daily scrum meetings to efficiently remove obstacles in the way of the team. A SM can change for each feature or working project. _This seems to be also done quite well for Wasabi AFAIK._

### 4. Set a clear product vision

Who is using the product? Why? What needs does the product address? What are the unique selling points to bitcoiners? How does it compare to other competing offerings?

Everyone should buy into the product vision for obvious reasons. It should be short and sweet, and repeated often. This is helpful not only for the product team but marketing, support, etc. End users should be precisely defined to avoid confusion and discuss features and bug fixing with empathy. _This is also well done already though better clarity of the end users could help._

### 5. Set a product roadmap and groom it

Living and breathing list of feature requests and bugs fixing focused on the coming 6-12 months to create an open and ongoing dialogue between the product owner, the scrum team and scrum master as well as other stakeholders to manage goals and risks.

The product backlog is managed by the product owner and is split in three main levels: (1) fine-grained and detailed items ready for processing in the coming sprint, (2) medium-grained user stories kept for short/medium term collecting more user feedback and design iterations, (3) coarse-grained items like epics that are for longer term and need much more research.

Grooming the product backlog happens regularly to keep it clean and organized. Everyone should know more or less how the backlog looks to understand the prioritization. The whole product team should participate in the grooming session to make sure everyone is on the same page. _This is the perhaps weakest link in the current product development IMO. There is no dedicated forum to discuss and set prioritization for Wasabi._

### 6. Set sprints and prepare for them

A sprint is a time lapse for the team to write software. It should have a clear goal. Before a sprint is started, there is a backlog grooming session. Schedule sprints with fixed deadlines (2 weeks). Scope of releases can vary, shrink or expand. Time to ship new releases should not. New releases should be shipped in consistent intervals. Adding regular frequency to ship new releases sets a cadence in product development, helping the team get better at scoping and identifying “unknown unknowns” for future sprints.

The sprint planning session should include well-defined and agreed-upon user stories to define what features and bugs fixing should be worked on by the scrum team. The goal is NOT to add stress on the development team but to foster the best environment to write software and avoid second thoughts, redesigns and miscommunication. Ensure clarity, testability and feasibility. An item can be included in a sprint if there is a way to verify that it is satisfied within the sprint with testing. A clear definition of “done” should be established.

Sizing items and workload is done by the scrum team in full autonomy before a sprint scope is agreed upon. No matter what happens, a sprint should lead to a release. Even if scope must be cut, a release should happen. A design phase should happen prior to writing too much software with the goal to get team consensus as early as possible to bring clarity on the scope of work and avoid time wasted for developers. _Release cadance looks quite regular already so not sure there would be anything else to do. A UI/UX design phase could be added though to help define features for the UI/UX concerns._

### 7. Review the sprint, rinse and repeat to scale

Each scrum team can have its own set of items to work on based on the product backlog grooming session. Each scrum team has its scrum master, which can be a dev on the team of course so it scales well.

After a sprint is done, a release is shipped. A sprint review should be scheduled to assess how well the sprint happened. Was everything shipped? Did we overestimate or underestimate the amount of work? Why? How can we fix it? Was it cool to work on? Scrum master and product owner should use this time to improve the coming sprints in the next product backlog grooming session.

Rinse and repeat, have fun and build the world’s best bitcoin wallet without anyone's permission!
