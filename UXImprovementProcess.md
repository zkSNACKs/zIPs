## UX Improvement Process

```
zIP: 6
Layer: Company
Title: UX Improvement Process
Author: cosmicbutterglue
Status: Proposed
Type: Process
Created: 2022-10-02
License: lol
```
## Motivation

UX improvements are hard. Right now, we are gathering UX feedback from many different places (Twitter, contribution games, Bitcointalk, etc.). The problem: The UX research we gather is not reproducible. We may get many different opinions, but are unable to identify where exactly the user is encountering issues (and where they aren't). This results in improving UX in the dark. We may identify pain points and resolve them, but we may also identify them wrongly and make the user experience worse by accident. This results in a lot of wasted time for developers as well as unhappy users, which are both scenarios we should try to avoid as best we can.

In addition to resolvng UX issues, this process can help eliviate controversy with proposed UX improvements. Instead of debating internally what improvements are the best, we should be gathering reproducible user feedback on proposed improvements to clarify which proposals work best for the user.

## Develop a Streamlined UX Improvement Process

To avoid wasting time on developing UX solutions which may not be needed, or worse, make UX more complicated, we should develop a streamlined UX improvement process. 

### 0. Form a pool of UX Testers

Establish a pool of UX Testers comprised of 3/4 noobs and 1/4 advanced users. For every round of testing, Testers should receive small bounties to motivate participation.

### 1. Identify the issue

Often times we already have a good idea of what is hindering good UX. When such potential pain points are identified, an issue should be opened on GH explaining the perceived UX problem. 

### 2. Get User Feedback

Observe whether the issue exists for the testing groups by giving them tasks to perform in a documented, screen shared call. If the issue exists for users, move to step 3. If the majority of the testing groups does not encounter the issue, mark issue as resolved. If results are inconclusive, widen testing groups.

### 3. Develop a Solution.

Develop a prototype for a solution. This does not yet need to be executable code, a click-dummy would suffice to save time. This would also solve the problem of noobs not knowing how to compile code from source and save time for the testing group.

### 4. Test the Solution

Present the testing groups with the solution in a screen-shared call. Let the testing group interact with the solution and give them tasks to perform to see if the solution solves the problem. This process should be documented (e.g. screen-recorded) to give insights into interactions with the product in general and clearly identify other potential pain points. If the issue is resolved for the majority of the testing groups develop executable code and merge to master. If the solution does not solve the issue return to 3.
