```mermaid
erDiagram
    Account {
        string BusinessRecordType
        string SchoolRecordType
        string RecyclingCompanyRecordType
    }
    Contact {
        string VolunteerRecordType
        bool IsPriorityRecord
    }
Laptop_Recycle_Rate__c{
    lookup Account
    currency Value__c
    date Manufacture_Date__c
    picklist ProcessorType__c
    picklist OS__c
    int Memory__c
    int Hard_Drive_Capacity__c
}
    Volunteer_Feedback__c {
        string Comment__c
        int Rating__c
    }
JobPositionAssignment{
    lookup VolunteerInitiative
}
    Laptop_Sale__c {
        date Sale_Date__c
        currency Sale_Price__c
    }
    Donation_Batch__c {
        picklist Status__c
        lookup Account__c
        lookup Donation_Coordinator__c
    }
    IndividualApplication{}
    LaptopSoftwareInstallation__c {
        string Software_Name__c
        date Install_Date__c
    }
    Laptop__c {
        date Manufacture_Date__c
        picklist ProcessorType__c
        picklist OS__c
        int Memory__c
        int Hard_Drive_Capacity__c
        string Allocated_To_RecyclingCompany__c
        bool PII_Flag__c
        bool Available__c
        picklist Designated_For__c
        currency Market_Value__c
    }
    Laptop_Allocation__c {
        date Allocation_Date__c
        string RelatedApplication
        picklist Status__c
    }
    VolunteerInitiative {
        string InitiativeName
    }
    TaxLetter__c {
        picklist Status__c
        lookup Donation_Batch__c
    }
    Shipping_Information__c {
        date DeliveryDate__c
        string TrackingNumber__c
        string Carrier__c
        string ShippingStatus__c
        date ShippedDate__c
        date EstimatedArrivalDate__c
        string ShippingMethod__c
    }

    Account ||--o{ Donation_Batch__c : "company donates"
    Account ||--o{ Laptop_Sale__c : "company purchases"
    Account ||--o{ Laptop_Recycle_Rate__c : "defines"
    Account ||--o{ IndividualApplication : "creates"
    Donation_Batch__c ||--o{ Laptop__c : contains
    Donation_Batch__c ||--o{ Shipping_Information__c : "ships"
    IndividualApplication ||--o{ Laptop_Allocation__c : "related to"
    Account ||--o{ Laptop_Allocation__c : "school receives"
    Laptop_Allocation__c ||--o{ Laptop__c : allocates
    VolunteerInitiative ||--|| Laptop_Allocation__c : "assigned to"
    VolunteerInitiative ||--o{ JobPositionAssignment : "assigns"
    Laptop__c ||--o{ Laptop_Sale__c : "assigned to"
    Laptop__c ||--o{ Laptop_Recycle_Rate__c : "priced by"
    Contact ||--o{ Volunteer_Feedback__c : "describes"
    Account ||--o{ Volunteer_Feedback__c : "gives"
    Laptop__c ||--o{ LaptopSoftwareInstallation__c : "downloads"
    Donation_Batch__c ||--o{ TaxLetter__c : "generates"
    Laptop_Allocation__c ||--o{ Shipping_Information__c : "ships"
    Account ||--o{ Laptop__c : "owns"
    Account ||--o{ Contact : "has"