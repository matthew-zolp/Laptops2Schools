# Laptops2Schools Security Configuration

## Organization-Wide Defaults (OWD)

| Object | OWD Setting | Rationale |
|--------|-------------|-----------|
| Account | Private | Req 1: Corporations cannot see each other's records |
| Contact | Controlled by Parent | Inherits Account privacy |
| Opportunity | Private | Contains sensitive donation amounts |
| Donation_Batch__c | Private | Req 2: Only designated contacts see tax amounts |
| Laptop__c | Private | Req 3: PII protection, Req 4: Rate modification control |
| Laptop_Delivery__c | Private | Req 8: Schools only see their allocations |
| Volunteer_Feedback__c | Private | Sensitive feedback data |
| VolunteerInitiative | Public Read Only | Volunteer coordination |

## Role Hierarchy

```
Executive Director
├── L2S Security Team Manager
│   └── L2S Security Team Member
├── Regional Director
│   └── Donation Coordinator (DC)
│       └── Donation Coordinator Assistant
├── Recycling Manager
│   └── Recycling Account Coordinator (AC)
├── Volunteer Manager
│   └── Volunteer Coordinator
└── School Liaison Manager
    └── School Representative
```

## Custom Fields Required

### Account Object
- `Tax_ID__c` (Text, Encrypted)
- `Tax_Letter_Document__c` (File)
- `Region__c` (Picklist)
- `Designated_Tax_Contact__c` (Lookup to Contact)

### Laptop__c Object
- `PII_Flag__c` (Boolean) - Already exists
- `Recycling_Rate__c` (Currency, Encrypted)
- `Tax_Donation_Amount__c` (Currency)
- `Region__c` (Formula from Donation_Batch__c)

### Contact Object
- `Is_Designated_Tax_Contact__c` (Boolean)
- `Region__c` (Picklist)

## Permission Sets

### L2S_Security_Team
- **Purpose**: Req 3 - Access PII laptops
- **Permissions**:
  - View All + Modify All on Laptop__c
  - Custom permission: `Access_PII_Laptops`
  - Field access to all PII-related fields

### L2S_Donation_Coordinator
- **Purpose**: Req 5,6 - Regional access and corporate tax info
- **Permissions**:
  - Read/Write on Donation_Batch__c (filtered by region)
  - Read/Write on Account (filtered by region)
  - Field access to `Tax_ID__c` and `Tax_Letter_Document__c`
  - Custom permission: `Modify_Laptop_Rates` (Req 4)

### L2S_Recycling_AC
- **Purpose**: Req 7 - See recycling payment amounts
- **Permissions**:
  - Read access to `Recycling_Rate__c` field
  - Custom permission: `View_Recycling_Rates`

### L2S_Corporate_Tax_Contact
- **Purpose**: Req 2 - Designated contacts see tax amounts
- **Permissions**:
  - Read access to `Tax_Donation_Amount__c` (own company only)
  - Custom permission: `View_Own_Tax_Amounts`

### L2S_Volunteer_Active
- **Purpose**: Req 9 - Volunteers see assignment details
- **Permissions**:
  - Read access to assigned Laptop_Delivery__c records
  - Read access to related School and Laptop details
  - Custom permission: `View_Assignment_Details`

### L2S_School_User
- **Purpose**: Req 8 - Schools see only their allocations
- **Permissions**:
  - Read access to own Laptop_Delivery__c records
  - Read access to allocated Laptop__c records

## Public Groups

### Security_Team_Group
- **Members**: L2S Security Team Manager, L2S Security Team Members
- **Purpose**: PII laptop access

### Regional_Groups (Create per region)
- **North_Region_DCs**
- **South_Region_DCs** 
- **East_Region_DCs**
- **West_Region_DCs**

### Recycling_Team_Group
- **Members**: Recycling Manager, Recycling ACs
- **Purpose**: Recycling rate visibility

## Sharing Rules

### Account Sharing Rules

1. **Regional Corporation Access**
   ```
   Based on: Region__c = "North"
   Share with: North_Region_DCs
   Access: Read/Write
   ```
   *(Repeat for each region - Req 5)*

2. **Corporate Self-Access**
   ```
   Based on: Record Type = "Business"
   Share with: Account Owner + Designated_Tax_Contact__c
   Access: Read Only
   ```

### Laptop__c Sharing Rules

1. **PII Laptops to Security Team**
   ```
   Based on: PII_Flag__c = TRUE
   Share with: Security_Team_Group
   Access: Read/Write
   ```
   *(Req 3)*

2. **Regional Laptop Access**
   ```
   Based on: Region__c = "North"
   Share with: North_Region_DCs
   Access: Read/Write
   ```
   *(Repeat for each region - Req 5)*

3. **Recycling Rates to ACs**
   ```
   Based on: Recycling_Rate__c != NULL
   Share with: Recycling_Team_Group
   Access: Read Only
   ```
   *(Req 7)*

### Laptop_Delivery__c Sharing Rules

1. **School Allocations**
   ```
   Based on: School_Account__c = [User's Account]
   Share with: School Users
   Access: Read Only
   ```
   *(Req 8)*

2. **Volunteer Assignments**
   ```
   Based on: Assigned_Volunteer__c = [User]
   Share with: Volunteer
   Access: Read Only
   ```
   *(Req 9)*

## Field-Level Security

### Sensitive Fields - Restricted Access

| Field | Accessible To | Requirement |
|-------|---------------|-------------|
| `Tax_ID__c` | DCs assigned to corporation | Req 6 |
| `Tax_Letter_Document__c` | DCs assigned to corporation | Req 6 |
| `Tax_Donation_Amount__c` | Designated contacts only | Req 2 |
| `Recycling_Rate__c` | Recycling ACs only | Req 7 |
| `PII_Flag__c` | Security Team + Managers | Req 3 |

## Process Builder Rules

### Regional Assignment
```apex
// Auto-assign DCs based on Account region
IF(Account.Region__c = "North" AND Account.RecordType = "Business") {
    Account.Owner = North_Region_DC_Queue
}
```

### PII Laptop Restriction
```apex
// Prevent non-security team from editing PII laptops
IF(Laptop__c.PII_Flag__c = TRUE AND !$Permission.Access_PII_Laptops) {
    ERROR: "Only Security Team can modify PII laptops"
}
```

### Tax Contact Validation
```apex
// Ensure designated contacts can only see their company's data
IF(Contact.Is_Designated_Tax_Contact__c = TRUE) {
    // Grant access to related Account tax fields
    Account.Tax_Donation_Amount__c.Visible = TRUE
}
```

## Single Sign-On Configuration

### Internal Users (Req 10, 11)
- **Identity Provider**: Active Directory (ADFS)
- **SSO Method**: SAML 2.0
- **Just-in-Time Provisioning**: Enabled
- **User Provisioning**: Automatic via SCIM
- **Deprovisioning**: Automatic when AD account disabled

### External Users (Req 12)
- **Volunteers**: Facebook Connect
- **Schools**: Twitter OAuth + Facebook Connect
- **Social SSO Configuration**:
  ```xml
  <SocialSSO>
    <Provider>Facebook</Provider>
    <Provider>Twitter</Provider>
    <UserType>Community</UserType>
    <AutoCreateUsers>true</AutoCreateUsers>
  </SocialSSO>
  ```

## Data Visibility Matrix

| User Type | Account | Laptop | Delivery | Tax Info | PII | Recycling |
|-----------|---------|--------|----------|----------|-----|-----------|
| Executive | All | All | All | All | All | All |
| Security Team | Regional | PII Only | Regional | No | Yes | No |
| DC | Regional | Regional | Regional | Assigned | No | No |
| Recycling AC | No | Rates Only | No | No | No | Yes |
| Corporate Contact | Own Only | Own Donations | No | Own Only | No | No |
| School User | Own Only | Allocated | Own Only | No | No | No |
| Volunteer | Assignment | Assignment | Assignment | No | No | No |

## Compliance and Audit

### Audit Trail Requirements
- Enable Field History Tracking on all sensitive fields
- Set up Login History monitoring
- Configure Setup Audit Trail for permission changes
- Implement Custom Audit Log for PII access

### Data Retention
- Corporate tax documents: 7 years
- Laptop PII data: Purge after delivery completion
- Volunteer assignment data: 2 years
- Recycling rate history: 5 years

## Territory Management

### Regional Territories
```
North Region Territory
├── North Corporate Accounts
├── North School Accounts
└── North Volunteer Assignments

South Region Territory
├── South Corporate Accounts
├── South School Accounts
└── South Volunteer Assignments
```

This configuration ensures all 12 requirements are met while maintaining proper data security and access controls throughout the Laptops2Schools operation.