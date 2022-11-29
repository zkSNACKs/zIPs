# Relevance Realization

```
zIP: 6
Layer: Development
Title: Relevance Realization
Author: nopara73, David Palhazi
Status: Proposed
Type: Process
Created: 2022-11-29
License: lol
```

## Motivation
As the number of zkSNACKs contributors increases synchronization of action becomes a challenge. To improve upon this the [Relevance Realization project](https://github.com/orgs/zkSNACKs/projects/18) and the new Coordinator status has been established.
The goal is with that zIP to collect and write down the actively used relevance realization meeting path /workflow.

  

## Ewoks
Ewoks own the ewok board. Their work starts with the Backlog, to which anyone can add items, as well all the issues ever opened in Wasabi Wallet GitHub repository are getting added here automatically.
Ewoks take the backlog and process it: ewoks may archive a card, put it into Triaged, into Minefield, into Not Now or into Done buckets.
If the ewoks put a card into Triaged, then they must set a Project, estimate its Importance, Urgency and Size.
Triaged is where the work of Coordinators start.

## Coordinator
A Coordinator is a person that coordinates different boards within the Relevance Realization project. Coordinators MUST oversee, understand and maintain their boards.

### Path of the issues
![image](https://user-images.githubusercontent.com/119300488/204535610-31342862-343b-4591-bf58-9a0e6a0a9943.png)

### Urgency and importance - how to decide the attributes correctly
![image](https://user-images.githubusercontent.com/119300488/204541958-3518d232-a9f0-414a-bd20-f5685a0ac308.png)

 

### Processing Cards

Coordinators should try to collapse multiple issues into single cards. For example if there 3 different issues about Send workflow bugs, then they should be collapsed into a single card: "Fix Send Workflow". This is useful, because this way Coordinators can recognize dependencies between tasks and spare extra work done on issues by developers.

## KATAs

### Meeting KATA

0. Is there anything important to discuss?
0. Filter tasks: urgency:"asap💚" // work that must be done ASAP
0. Discuss Doing bucket // don't forget about ongoing works
0. Filter tasks: importance:"must💚"
0. Filter tasks: size:"small💚" // low hanging fruits
0. Is there anything else to discuss?


### Card KATA

0. Are there similar cards or work to be done from what we can create a single card out of?
0. Did ewoks set Project, Importance, Urgency and Size correctly?
0. Is there any task that should be done before this? If yes, then place it into Parking and link to it in Waiting For property of the card.
0. Is there any task that relies on this card? If yes, then link to it in Follow-up Action property of the card.
0. If issue is in the Doing status must add Assignees to the card the doers and their coordinator as well.   
0. If possible Next Action shall be filled out. If the card gets into the Doing, then Next Action is required to be filled out.

