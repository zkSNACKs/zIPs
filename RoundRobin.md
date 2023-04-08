# Round Robin

```
zIP: 7
Layer: Development
Title: Round Robin
Author: molnard, pule08
Status: Proposed
Type: Active
Created: 2023-03-30
License: lol
```


## Motivation 

This process prevents working on controversial items lacking consensus, as it can lead to unproductive arguments, frustration, and further energy expenditure from our team members. Early detection of these tendencies is crucial to avoid wasting time and resources.

## Abstract 

At times, certain items on the `relevance realization` board (RR board) may be challenging for the team leader to evaluate. In such cases, the team leader may choose to initiate a 'Round Robin discussion'. During this discussion, relevant team members collaborate to reach a consensus on the item's future.

## Process

1. If an item appears in the `Triaged` column of the project board, the team leader may find it challenging to evaluate without further information. 
2. In this case, the team leader or coordinator creates an item with a proper description on the `round robin` project board, making it a candidate for discussion.
3. During the weekly team meeting, the team leader brings up the candidates and the team decides whether the item is worth discussing. At this stage, there is no time for extended debate; only the worthiness of the item is considered.
4. Based on the result of the votes, the team leader either able to make a decision and act accordingly on the `RR` board or request a `Round Robin` meeting to get more details and opinion.
5. The coordinator schedules a `Round Robin` meeting and invites relevant team members or those who are already involved in the issue.
6. During the `Round Robin` meeting, the topic is discussed until the team leader can make a decision on what to do with the item. After that, the team leader takes appropriate action. Like moving item to Will Do status, or not not or Minefield. 




    ```mermaid
    flowchart TD
    A[Item added to Triaged] -->|on Relevance Realisation Team leader evaluate| B{Hard or Easy to evaluate}
    B --> |Hard| C(Item added to Round Robin board)
    B --> |Easy|D(Status update on Relevance R board)
    D --> F[Will Do]
    D --> G[Not Now]
    D --> H[Minefield]
    C --> |item prep for vote| I(present&vote on weekly team meeting)
    I --> J{vote of worthiness}
    J --> |Team Leader sum&decide| D(Status update on Relevance R board)
    J --> |need details| N[Round Robin]
    N --> |coordinator schedule&invite| O{Round Robin discussion}
    O --> |Team Leader decide| D(Status update on Relevance R board)
    ```
    
    
