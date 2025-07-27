```mermaid
flowchart TD
  subgraph Portal_User["Student in Portal"]
    S1[Submit Question via Chat Widget]
    S2[Post Question in Chatter Forum]
  end

  subgraph Live_Chat["Enhanced Chat"]
    LC1[Pre-chat Form Captures Details]
    LC2[Omni-Channel Routes Chat by Skills]
    LC3[Virtual Expert Responds Live]
    LC4[Case Automatically Created & Updated]
  end

  subgraph Chatter_Forum["Chatter Forum"]
    CF1[Student Posts Question]
    CF2[Experts & Students Collaborate via Posts & Comments]

  end

  S1 -->|Syncronous help| LC1 --> LC2 --> LC3 --> LC4
  S2 -->|Asynchronous help| CF1 --> CF2
  ```
