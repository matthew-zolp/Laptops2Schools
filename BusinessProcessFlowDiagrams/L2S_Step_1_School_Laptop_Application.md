```mermaid
sequenceDiagram
    participant School as School User
    participant Portal as Experience Cloud Portal
    participant SF as Salesforce (IndividualApplication)

    School->>Portal: Log in to Customer Portal
    Portal->>School: Display OmniScript Application Form
    School->>Portal: Enter school details, laptop needs, supporting docs
    Portal->>SF: Create IndividualApplication record
    Portal->>SF: Attach supporting documentation
    SF-->>Portal: Confirm application receipt
    Portal-->>School: Show confirmation message
```