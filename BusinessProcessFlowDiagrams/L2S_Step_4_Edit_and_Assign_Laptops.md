## Step 5: Automating Laptop Monetary Value and Assignment

### Steps to Automate
- L2S IT Specialists complete the donation intake process by adding details to the laptop records (manufacture date, processor type, memory, hard drive capacity).
- Laptops are designated for schools or recycling based on these details.
- Laptops for schools are assigned a monetary value using a government estimating web service.
- Laptop records are assigned to School ACs or Recycling ACs based on region and current workload.

```mermaid
flowchart TD
    A[L2S IT Specialist edits Laptop__c details]
    B{Are all details filled out?}
    A --> B
    B -- "Yes" --> C[MuleSoft API calls government webservice for value]
    C --> D[MuleSoft updates Laptop__c with monetary value]
    D --> E{Designation status set?}
    E -- "No" --> F[User must set designation to School or Recycling]
    E -- "Yes" --> G[Designate Laptop button becomes visible]
    G --> H[User selects Designate Laptop]
    H --> I{Designation}
    I -- "School" --> J[Assign to Regional School AC queue]
    I -- "Recycling" --> K[Assign to Regional Recycling AC queue]
```
**Notes:**
- Assignment to Users from queues is automated via OmniChannel based on region and capacity.