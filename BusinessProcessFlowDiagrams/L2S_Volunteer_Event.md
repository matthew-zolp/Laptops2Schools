```mermaid
flowchart TD
  A1[School confirms receipt via portal]
  A2[Triggers status update and email to Volunteer Contact]
  A3[Volunteer logs into portal]
  A4[Clicks 'Check In' → JobPositionAssignment record updated]

  B1[Volunteer views assigned Laptops]
  B2[Downloads software from cloud]
  B3[Fills in Software Name, License ID, Expiration Date]
  B4[LaptopSoftwareInstallation__c records created]
  B5[Volunteer Completes Assignment - Checks Out in Portal]

  C1[Volunteers collaborate using Slack - Channel per Region]

  A1 --> A2 --> A3 --> A4 --> B1 --> B2 --> B3 --> B4 --> B5 --> C1
  ```
