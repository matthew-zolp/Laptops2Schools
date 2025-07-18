# Step 3: Laptop Intake Process Automation

```mermaid
flowchart TD
    A[Laptops arrive at Shipping Hub] --> B[Update Donation Batch status to Arrived]
    B --> C[Assign laptops to L2S IT Specialists via OmniChannel routing]
    C --> D{Does laptop contain PII?}
    D -- No --> E[Continue standard intake process]
    D -- Yes --> F[IT Specialist sets PII Flag on Laptop]
    F --> G[Email sent to L2S Security Team with Laptop link]
    G --> H[Security Team Manager assigns Laptop to Security Team Member]
    H --> I[Security Team reviews PII files, Updates PII description on the Laptop__c record]
    I --> J[Security Team clicks Notify Corporation button]
    J --> K[Email via flow sent to Corporation detailing PII: request delete or return drive]
    K --> L{Corporation responds within 5 days?}
    L -- Yes --> M[Security Team follows corporation's instructions]
    M --> N[If delete: Security Team deletes files, removes PII Flag]
    N --> O[Record reassigned to original IT Specialist]
    L -- No --> P[Create Task for Security Team: Delete files in 24h]
    P --> Q[Scheduled Flow waits 24h, checks PII Flag]
    Q -- PII Flag removed --> O
    Q -- PII Flag still set --> R[Assign Laptop to Security Team Manager]
```

**Key Automation Points:**
- Status and assignment updates are automated on arrival.
- PII_Flag triggers email and assignment flows.
- Corporation notification is automated
- Task creation and scheduled flow handle escalation if no response.
- Record reassignment is automated after PII is cleared.