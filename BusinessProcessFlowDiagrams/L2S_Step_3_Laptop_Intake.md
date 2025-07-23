# Step 3: Laptop Intake Process Automation

```mermaid
flowchart TD
    A[Laptops arrive at Shipping Hub] --> B[Update Donation Batch status to Arrived]
    B --> C[Assign laptops to L2S IT Specialists via OmniChannel routing]

    C --> D{Does laptop contain PII?}
    D -- No --> E[Continue standard intake process]
    E --> E1[Record Specs: Processor, Memory, etc.]
    E1 --> E2[Designate for School or Recycling]
    E2 --> E3[Update Inventory, Assign Value]
    E3 --> E4[Auto-assign record to School or Recycling AC]

    D -- Yes --> F[IT Specialist sets PII Flag on Laptop]
    F --> G[Flow: Email sent to L2S Security Team with laptop link]
    G --> H[Manager assigns Laptop to Security Team Member]
    H --> I[Security Team reviews PII files and updates Laptop__c record]

    I --> J[Click Notify Corporation via Flow]
    J --> K[Email sent to corporation]

    K --> L{Did Corporation respond in 5 days?}
    L -- Yes --> M[Follow instructions: Delete or Return Drive]
    M --> N[If Delete: Delete files and remove PII Flag]
    N --> O[Reassign record to original IT Specialist]

    L -- No --> P[Create Task for Security Team: Delete files in 24h]
    P --> Q[Scheduled Flow checks for 24-hour delay]
    Q -- PII Flag removed --> O
    Q -- PII Flag still set --> R[Reassign to Security Team Manager]

    %% Audit Trail (optional enhancement)
    M --> AT3[Create audit log: Corporation instruction received]

    %% End state
    E4 --> Z[Ready for Laptop Allocation or Recycling]
    O --> Z
    R --> Z
```

**Key Automation Points:**
- Status and assignment updates are automated on arrival.
- PII_Flag triggers email and assignment flows.
- Corporation notification is automated
- Task creation and scheduled flow handle escalation if no response.
- Record reassignment is automated after PII is cleared.