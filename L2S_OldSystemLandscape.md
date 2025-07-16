```mermaid
flowchart TD
    subgraph Current_Systems
        InventorySystem["Inventory System"]
        click InventorySystem "System of record for laptops, schools, and recycling companies" "System of record for laptops, schools, and recycling companies"
        ShippingSystem["Shipping Fulfillment System"]
        CloudStorage["Cloud Storage System"]
    end

    subgraph Users
        DCs["Donation Coordinators (DCs)"]
        ITSpecialists["IT Specialists"]
        SecurityTeam["Security Team"]
        SchoolACs["School Allocation Coordinators (School ACs)"]
        RecyclingACs["Recycling Allocation Coordinators (Recycling ACs)"]
        Managers["L2S Managers"]
        Volunteers["Volunteers"]
        VirtualExperts["Virtual Experts"]
        Corporations["Corporations"]
        RecyclingCompanies["3rd Party Recycling Companies"]
        Schools["Schools"]
    end

    %% System Access
    DCs -- "Web Access, Manual Import/Update" --> InventorySystem
    DCs -- "Web Access" --> ShippingSystem
    ITSpecialists -- "Web Access" --> InventorySystem
    ITSpecialists -- "Web Access" --> ShippingSystem
    SecurityTeam -- "Web Access" --> InventorySystem
    SchoolACs -- "Web Access" --> InventorySystem
    RecyclingACs -- "Web Access" --> InventorySystem
    Managers -- "Web Access" --> InventorySystem
    Volunteers -- "Access Training/Software" --> CloudStorage
    VirtualExperts -- "Support Schools/Students" --> CloudStorage
    Corporations -- "Web Access, Upload CSV" --> InventorySystem
    RecyclingCompanies -- "Web Access, Update Payment" --> InventorySystem
    Schools -- "Web Access" --> InventorySystem
```