```mermaid
flowchart TD
  subgraph Experience_Cloud_Portal["Experience Cloud - School User"]
    UI1[Login as School Portal User]
    UI2[Start Thank-You Submission Flow]
    UI3[Select from Applications]
    UI5[Upload Thank You Video File]
    UI6[Submit Flow]
  end

  subgraph Screen_Flow[Salesforce Screen Flow : System Context]
    FLOW1[Get Laptops__c in Laptop_Allocation__c]
    FLOW2[Get Donation_Batch__c related to Laptops__c]
    FLOW3[Get Companies from Dontation_Batch__c records]
    FLOW4[Store Uploaded File ContentVersion]
    LOOP[Loop Through Donor Accounts]
    CHATTER1[Post FeedItem to Donor Account Chatter Feed]
    CHATTER2[Attach Uploaded Video using FeedAttachment]
  end

  UI1 --> UI2 --> UI3 --> UI5 --> UI6 --> FLOW1 --> FLOW2 --> FLOW3 --> FLOW4 --> LOOP
  LOOP --> CHATTER1 --> CHATTER2
  ```
