```mermaid
flowchart TD
  subgraph Intake["School Application Intake"]
    A1[School submits IndividualApplication record]
    A2[IndividualApplication includes<br/>Class Types & Software Requested]
  end

  subgraph Matching["OmniScript in Custom Tab"]
    M1[Retrieve SoftwareToSpecMapping__mdt Records]
    M2[Query Laptop__c where Designated_For = 'School']
    M3[Compare Laptop Specs<br/>to CMDT Min Requirements]
    M4[Return Matching Laptop List]
    M5[School AC selects laptops to allocate]
    F1[Create LaptopAllocation__c Record and relate to Application]
    F3[Update Laptop__c Allocated = TRUE]
  end

  subgraph Approval["Conditional Approval"]
    C1[Check Allocation Total YTD for School]
    C2{Greater Than 500 laptops this school year?}
    C3[Trigger Approval Process to AC Manager]
    C4[Approved]
  end
subgraph ScreenFlow["ScreenFlow"]
    VolunteerAssignment
end
  subgraph VolunteerAssignment["Assign Volunteer"]
    V1[Query Volunteers in Region with Skills matching Software]
    V2[Check Language Compatibility & Availability]
    V3[School AC assigns best-fit Volunteer]
  end
  subgraph Automation["Finalize Allocation"]
    F2[Create VolunteerInitiative/Job Position Assignment/Job Position Shift for Volunteer]
    E3[Relate Volunteer Initiative to IndividualApplication record]
    F4[Trigger Inventory System Sync]
    F5[Create ShippingRecord__c]
  end

  subgraph LetterGen["School & Volunteer Notification"]
    L1[Generate Allocation Letter in Local Language - OmniStudio DocGen]
    L2[Include Delivery Date, Donor Corp, Value, Volunteer Contact]
    L3[Send Email to School & Volunteer with Letter - Flow]
  end

  A1 --> A2 --> M1 --> M2 --> M3 --> M4 --> M5 --> F1 --> F3 --> C1 --> C2
  C2 -- No --> V1
  C2 -- Yes --> C3 --> C4 --> V1
  V1 --> V2 --> V3 --> F2 --> E3 --> F4 --> F5 --> L1 --> L2 --> L3
