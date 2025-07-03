# Koidoo: System Design and Conception

## 1. Introduction

This document details the comprehensive design and conception of the Koidoo peer-to-peer parcel delivery platform. It translates the business requirements and technical architecture overview into detailed system designs using Unified Modeling Language (UML) and Systems Modeling Language (SysML) diagrams. The aim is to provide a clear, unambiguous, and complete blueprint for the development and implementation of the Koidoo application, ensuring all stakeholders have a shared understanding of the system's structure, behavior, and relationships.

## 2. High-Level System Architecture

This section presents the foundational architectural views of the Koidoo system, illustrating its primary actors, core functionalities, and major components. These diagrams provide a high-level understanding of 'what' the system does and 'what' its main building blocks are.

### 2.1. UML Use Case Diagram

The Use Case Diagram illustrates the interactions between the primary actors and the Koidoo system, representing the system's functional requirements from an external perspective. It identifies the main functionalities offered by the system and how different types of users interact with them.

**Actors:**
*   **Sender:** An individual or small business that wants to send a parcel internationally.
*   **Traveler:** An individual who has available luggage space and is willing to carry parcels for a fee.
*   **Recipient:** The person who receives the parcel at the destination.
*   **Admin/Moderator:** Koidoo platform staff responsible for system management, user support, and compliance.
*   **Payment Gateway:** An external system responsible for processing financial transactions.
*   **Identity Verification Service:** An external system responsible for verifying user identities.
*   **Notification Service:** An external system responsible for sending notifications (SMS, Email, Push).
*   **Mapping Service:** An external system providing location and routing functionalities.

**Use Cases:**
*   **Manage User Profile:** Actors (Sender, Traveler, Recipient, Admin) can create, view, update, and delete their profiles.
*   **Verify Identity:** Sender and Traveler undergo identity verification.
*   **Create Parcel Request:** Sender initiates a request to send a parcel.
*   **Manage Trip:** Traveler publishes and manages their travel plans and available luggage space.
*   **Browse Parcel Offers:** Traveler views available parcel requests matching their trip.
*   **Accept Parcel Offer:** Traveler agrees to carry a specific parcel.
*   **Negotiate Terms:** Sender and Traveler can negotiate delivery terms (e.g., fee, pickup/drop-off details).
*   **Manage Parcel Status:** Sender and Traveler update the status of a parcel (e.g., picked up, in transit, delivered).
*   **Track Parcel:** Sender and Recipient can monitor the real-time status and location of a parcel.
*   **Process Payment:** Payment Gateway handles the escrow and release of funds upon successful delivery.
*   **Manage Disputes:** Admin/Moderator handles disagreements between Sender and Traveler.
*   **Send Notification:** Notification Service sends alerts to users.
*   **Provide Location Data:** Mapping Service provides location and routing information.
*   **Rate and Review:** Sender and Traveler provide feedback on each other after a transaction.
*   **Manage System (Admin):** Admin/Moderator performs administrative tasks like user management, content moderation, and system configuration.

```mmd
--- 
title: Koidoo Use Case Diagram
--- 

classDiagram
    direction LR

    actor Sender
    actor Traveler
    actor Recipient
    actor Admin/Moderator
    actor "Payment Gateway" as PaymentGateway
    actor "Identity Verification Service" as IDService
    actor "Notification Service" as NotificationService
    actor "Mapping Service" as MappingService

    rectangle KoidooSystem {
        usecase "Manage User Profile"
        usecase "Verify Identity"
        usecase "Create Parcel Request"
        usecase "Manage Trip"
        usecase "Browse Parcel Offers"
        usecase "Accept Parcel Offer"
        usecase "Negotiate Terms"
        usecase "Manage Parcel Status"
        usecase "Track Parcel"
        usecase "Process Payment"
        usecase "Manage Disputes"
        usecase "Send Notification"
        usecase "Provide Location Data"
        usecase "Rate and Review"
        usecase "Manage System (Admin)"
    }

    Sender --|> "Manage User Profile"
    Sender --|> "Verify Identity"
    Sender --|> "Create Parcel Request"
    Sender --|> "Negotiate Terms"
    Sender --|> "Manage Parcel Status"
    Sender --|> "Track Parcel"
    Sender --|> "Process Payment"
    Sender --|> "Rate and Review"

    Traveler --|> "Manage User Profile"
    Traveler --|> "Verify Identity"
    Traveler --|> "Manage Trip"
    Traveler --|> "Browse Parcel Offers"
    Traveler --|> "Accept Parcel Offer"
    Traveler --|> "Negotiate Terms"
    Traveler --|> "Manage Parcel Status"
    Traveler --|> "Process Payment"
    Traveler --|> "Rate and Review"

    Recipient --|> "Track Parcel"
    Recipient --|> "Manage User Profile"

    Admin/Moderator --|> "Manage User Profile"
    Admin/Moderator --|> "Manage Disputes"
    Admin/Moderator --|> "Manage System (Admin)"

    "Process Payment" <.. PaymentGateway : uses
    "Verify Identity" <.. IDService : uses
    "Send Notification" <.. NotificationService : uses
    "Provide Location Data" <.. MappingService : uses

    "Accept Parcel Offer" <.. "Browse Parcel Offers" : <<extends>>
    "Negotiate Terms" <.. "Accept Parcel Offer" : <<extends>>
    "Manage Parcel Status" <.. "Create Parcel Request" : <<includes>>
    "Manage Parcel Status" <.. "Accept Parcel Offer" : <<includes>>
    "Track Parcel" <.. "Manage Parcel Status" : <<includes>>
    "Process Payment" <.. "Manage Parcel Status" : <<includes>>
    "Rate and Review" <.. "Process Payment" : <<includes>>

```

### 2.2. UML Component Diagram

The Component Diagram illustrates the high-level structure of the Koidoo system, showing the major software components and their interfaces. It provides a modular view of the system, highlighting how different parts interact to deliver the overall functionality.

**Components:**
*   **Mobile App (Frontend):** User-facing application for iOS and Android (React Native).
*   **Web Portal (Frontend):** Web-based interface for admin and limited user access (React).
*   **API Gateway:** Entry point for all client requests, routing to appropriate microservices.
*   **User Service:** Manages user profiles, authentication, and KYC.
*   **Parcel Service:** Manages parcel creation, details, and status.
*   **Trip Service:** Manages traveler trip publications and available space.
*   **Matching Service:** Implements the core logic for matching parcels with trips.
*   **Payment Service:** Handles all payment-related operations, including escrow.
*   **Notification Service:** Manages sending various types of notifications.
*   **Chat Service:** Provides in-app messaging functionality.
*   **Document Service:** Manages secure storage and access to user documents.
*   **PostgreSQL Database:** Persistent storage for all application data.
*   **Identity Verification API:** External service for identity verification.
*   **Payment Gateway API:** External service for payment processing.
*   **Mapping API:** External service for location and routing.
*   **Notification API:** External service for sending push, SMS, and email notifications.

```mmd
--- 
title: Koidoo Component Diagram
--- 

classDiagram
    direction LR

    component "Mobile App" as MobileApp
    component "Web Portal" as WebPortal
    component "API Gateway" as APIGateway
    component "User Service" as UserService
    component "Parcel Service" as ParcelService
    component "Trip Service" as TripService
    component "Matching Service" as MatchingService
    component "Payment Service" as PaymentService
    component "Notification Service" as NotificationServiceInternal
    component "Chat Service" as ChatService
    component "Document Service" as DocumentService
    component "PostgreSQL DB" as PostgreSQLDB

    component "Identity Verification API" as IDVerificationAPI
    component "Payment Gateway API" as PaymentGatewayAPI
    component "Mapping API" as MappingAPI
    component "Notification API" as NotificationAPIExternal

    MobileApp --> APIGateway : REST/HTTP
    WebPortal --> APIGateway : REST/HTTP

    APIGateway --> UserService : REST/HTTP
    APIGateway --> ParcelService : REST/HTTP
    APIGateway --> TripService : REST/HTTP
    APIGateway --> MatchingService : REST/HTTP
    APIGateway --> PaymentService : REST/HTTP
    APIGateway --> NotificationServiceInternal : REST/HTTP
    APIGateway --> ChatService : REST/HTTP
    APIGateway --> DocumentService : REST/HTTP

    UserService --> PostgreSQLDB
    ParcelService --> PostgreSQLDB
    TripService --> PostgreSQLDB
    MatchingService --> PostgreSQLDB
    PaymentService --> PostgreSQLDB
    ChatService --> PostgreSQLDB
    DocumentService --> PostgreSQLDB

    UserService --> IDVerificationAPI : REST/HTTP
    PaymentService --> PaymentGatewayAPI : REST/HTTP
    MatchingService --> MappingAPI : REST/HTTP
    NotificationServiceInternal --> NotificationAPIExternal : REST/HTTP

    IDVerificationAPI ..> UserService : Callback/Response
    PaymentGatewayAPI ..> PaymentService : Callback/Webhook
    MappingAPI ..> MatchingService : Data
    NotificationAPIExternal ..> NotificationServiceInternal : Status

```




## 3. User Interaction Flows

This section details the dynamic behavior of the Koidoo system, illustrating how users interact with the platform to achieve their goals. UML Activity Diagrams will show the flow of activities, while UML Sequence Diagrams will depict the interactions between objects in a time-ordered sequence.

### 3.1. UML Activity Diagram: Create Parcel Request

This activity diagram illustrates the process a Sender follows to create a parcel request on the Koidoo platform.

```mmd
--- 
title: Koidoo Activity Diagram: Create Parcel Request
--- 

sequenceDiagram
    participant Sender
    participant MobileApp
    participant BackendAPI
    participant ParcelService
    participant MappingAPI
    participant DocumentService

    Sender->>MobileApp: Initiates "Create Parcel Request"
    MobileApp->>Sender: Displays parcel details form

    loop Input Parcel Details
        Sender->>MobileApp: Enters Origin, Destination, Size, Weight
        MobileApp->>MappingAPI: Requests location validation/suggestions
        MappingAPI-->>MobileApp: Returns validated locations/suggestions
        Sender->>MobileApp: Selects/confirms locations
        Sender->>MobileApp: Enters Contents, Value, Delivery Deadline
        MobileApp->>Sender: Displays Prohibited Items Warning
        Sender->>MobileApp: Confirms compliance
    end

    Sender->>MobileApp: Uploads Parcel Photos (Optional)
    MobileApp->>DocumentService: Uploads photos
    DocumentService-->>MobileApp: Confirms upload

    Sender->>MobileApp: Selects Pickup/Drop-off Preferences
    MobileApp->>BackendAPI: Submits Parcel Request Data
    BackendAPI->>ParcelService: Creates new Parcel Request
    ParcelService-->>BackendAPI: Confirms Parcel Creation
    BackendAPI-->>MobileApp: Confirms Parcel Request Submission
    MobileApp->>Sender: Displays Confirmation and Next Steps
```

### 3.2. UML Sequence Diagram: Parcel Matching and Acceptance

This sequence diagram illustrates the interaction between the Sender, Traveler, and various system components during the parcel matching and acceptance process.

```mmd
--- 
title: Koidoo Sequence Diagram: Parcel Matching and Acceptance
--- 

sequenceDiagram
    participant SenderApp as Sender
    participant TravelerApp as Traveler
    participant BackendAPI
    participant MatchingService
    participant ParcelService
    participant TripService
    participant NotificationService

    Sender->>BackendAPI: Submits Parcel Request (via ParcelService)
    BackendAPI->>ParcelService: Store Parcel Request
    ParcelService-->>BackendAPI: Parcel Stored
    BackendAPI-->>Sender: Confirmation

    Traveler->>BackendAPI: Publishes Trip (via TripService)
    BackendAPI->>TripService: Store Trip Details
    TripService-->>BackendAPI: Trip Stored
    BackendAPI-->>Traveler: Confirmation

    activate MatchingService
    MatchingService->>ParcelService: Query Unmatched Parcels
    ParcelService-->>MatchingService: Return Parcel List
    MatchingService->>TripService: Query Available Trips
    TripService-->>MatchingService: Return Trip List
    MatchingService->>MatchingService: Execute Matching Algorithm
    MatchingService-->>BackendAPI: Notify Potential Matches (to Traveler)
    deactivate MatchingService

    BackendAPI->>NotificationService: Send Push Notification to Traveler
    NotificationService-->>Traveler: "New Parcel Offers Available"

    Traveler->>TravelerApp: Views Parcel Offers
    TravelerApp->>BackendAPI: Request Parcel Offers (filtered by trip)
    BackendAPI->>MatchingService: Get Matching Parcels
    MatchingService-->>BackendAPI: Return Matching Parcels
    BackendAPI-->>TravelerApp: Display Parcel Offers

    Traveler->>TravelerApp: Selects and Reviews Parcel Offer
    TravelerApp->>BackendAPI: Accepts Parcel Offer
    BackendAPI->>MatchingService: Record Acceptance
    MatchingService->>ParcelService: Update Parcel Status (e.g., "Accepted")
    ParcelService-->>MatchingService: Status Updated
    MatchingService-->>BackendAPI: Acceptance Confirmed

    BackendAPI->>NotificationService: Send Push Notification to Sender
    NotificationService-->>Sender: "Your Parcel Offer Accepted!"
    BackendAPI-->>TravelerApp: Confirmation of Acceptance
```




## 4. Design Data Model (UML Class Diagram)

This section defines the static structure of the Koidoo system, illustrating the main entities (classes), their attributes, and the relationships between them. The UML Class Diagram provides a blueprint for the database schema and the object-oriented design of the application.

**Key Entities (Classes):**
*   **User:** Represents a registered user of the Koidoo platform. Can be a Sender, Traveler, or Admin.
    *   `userId: UUID` (Primary Key)
    *   `username: String`
    *   `email: String`
    *   `passwordHash: String`
    *   `userType: Enum (Sender, Traveler, Admin)`
    *   `registrationDate: DateTime`
    *   `lastLogin: DateTime`
    *   `kycStatus: Enum (Pending, Verified, Rejected)`
    *   `profilePictureUrl: String`
    *   `contactNumber: String`
*   **SenderProfile:** Specific attributes for a Sender.
    *   `senderId: UUID` (Primary Key, Foreign Key to User)
    *   `address: String`
    *   `city: String`
    *   `country: String`
    *   `postalCode: String`
*   **TravelerProfile:** Specific attributes for a Traveler.
    *   `travelerId: UUID` (Primary Key, Foreign Key to User)
    *   `passportNumber: String`
    *   `nationality: String`
    *   `travelerRating: Float`
    *   `totalParcelsCarried: Integer`
*   **Parcel:** Represents a parcel to be sent.
    *   `parcelId: UUID` (Primary Key)
    *   `senderId: UUID` (Foreign Key to User)
    *   `originAddress: String`
    *   `destinationAddress: String`
    *   `weightKg: Float`
    *   `dimensionsCm: String` (e.g., "LxWxH")
    *   `contentsDescription: String`
    *   `declaredValue: Float`
    *   `currency: String`
    *   `deliveryDeadline: DateTime`
    *   `status: Enum (Pending, Matched, PickedUp, InTransit, Delivered, Cancelled, Disputed)`
    *   `creationDate: DateTime`
    *   `lastUpdateDate: DateTime`
    *   `photoUrls: List<String>`
    *   `isInsured: Boolean`
    *   `insuranceAmount: Float`
*   **Trip:** Represents a traveler's planned trip.
    *   `tripId: UUID` (Primary Key)
    *   `travelerId: UUID` (Foreign Key to User)
    *   `departureLocation: String`
    *   `destinationLocation: String`
    *   `departureDate: DateTime`
    *   `arrivalDate: DateTime`
    *   `availableWeightKg: Float`
    *   `availableVolumeCm3: Float`
    *   `transportMode: Enum (Flight, Train, Car, etc.)`
    *   `status: Enum (Active, Completed, Cancelled)`
*   **Match:** Represents a successful match between a Parcel and a Trip.
    *   `matchId: UUID` (Primary Key)
    *   `parcelId: UUID` (Foreign Key to Parcel)
    *   `tripId: UUID` (Foreign Key to Trip)
    *   `negotiatedFee: Float`
    *   `status: Enum (Pending, Accepted, Rejected, Confirmed)`
    *   `matchDate: DateTime`
*   **Transaction:** Records financial transactions.
    *   `transactionId: UUID` (Primary Key)
    *   `matchId: UUID` (Foreign Key to Match)
    *   `senderId: UUID` (Foreign Key to User)
    *   `travelerId: UUID` (Foreign Key to User)
    *   `amount: Float`
    *   `currency: String`
    *   `koidooCommission: Float`
    *   `paymentGatewayFee: Float`
    *   `status: Enum (Pending, HeldInEscrow, ReleasedToTraveler, Refunded)`
    *   `transactionDate: DateTime`
*   **RatingReview:** Stores ratings and reviews between users.
    *   `reviewId: UUID` (Primary Key)
    *   `reviewerId: UUID` (Foreign Key to User)
    *   `reviewedId: UUID` (Foreign Key to User)
    *   `parcelId: UUID` (Foreign Key to Parcel, Optional)
    *   `rating: Integer (1-5)`
    *   `comment: String`
    *   `reviewDate: DateTime`
*   **Message:** Stores chat messages.
    *   `messageId: UUID` (Primary Key)
    *   `senderId: UUID` (Foreign Key to User)
    *   `receiverId: UUID` (Foreign Key to User)
    *   `matchId: UUID` (Foreign Key to Match)
    *   `content: String`
    *   `timestamp: DateTime`
    *   `isRead: Boolean`
*   **Dispute:** Records and manages disputes.
    *   `disputeId: UUID` (Primary Key)
    *   `matchId: UUID` (Foreign Key to Match)
    *   `raisedById: UUID` (Foreign Key to User)
    *   `description: String`
    *   `status: Enum (Open, UnderReview, Resolved, Closed)`
    *   `raisedDate: DateTime`
    *   `resolvedDate: DateTime`
    *   `resolutionDetails: String`

```mmd
--- 
title: Koidoo Class Diagram
--- 

classDiagram
    direction LR

    class User {
        +UUID userId
        +String username
        +String email
        +String passwordHash
        +Enum userType
        +DateTime registrationDate
        +DateTime lastLogin
        +Enum kycStatus
        +String profilePictureUrl
        +String contactNumber
    }

    class SenderProfile {
        +UUID senderId
        +String address
        +String city
        +String country
        +String postalCode
    }

    class TravelerProfile {
        +UUID travelerId
        +String passportNumber
        +String nationality
        +Float travelerRating
        +Integer totalParcelsCarried
    }

    class Parcel {
        +UUID parcelId
        +UUID senderId
        +String originAddress
        +String destinationAddress
        +Float weightKg
        +String dimensionsCm
        +String contentsDescription
        +Float declaredValue
        +String currency
        +DateTime deliveryDeadline
        +Enum status
        +DateTime creationDate
        +DateTime lastUpdateDate
        +List<String> photoUrls
        +Boolean isInsured
        +Float insuranceAmount
    }

    class Trip {
        +UUID tripId
        +UUID travelerId
        +String departureLocation
        +String destinationLocation
        +DateTime departureDate
        +DateTime arrivalDate
        +Float availableWeightKg
        +Float availableVolumeCm3
        +Enum transportMode
        +Enum status
    }

    class Match {
        +UUID matchId
        +UUID parcelId
        +UUID tripId
        +Float negotiatedFee
        +Enum status
        +DateTime matchDate
    }

    class Transaction {
        +UUID transactionId
        +UUID matchId
        +UUID senderId
        +UUID travelerId
        +Float amount
        +String currency
        +Float koidooCommission
        +Float paymentGatewayFee
        +Enum status
        +DateTime transactionDate
    }

    class RatingReview {
        +UUID reviewId
        +UUID reviewerId
        +UUID reviewedId
        +UUID parcelId
        +Integer rating
        +String comment
        +DateTime reviewDate
    }

    class Message {
        +UUID messageId
        +UUID senderId
        +UUID receiverId
        +UUID matchId
        +String content
        +DateTime timestamp
        +Boolean isRead
    }

    class Dispute {
        +UUID disputeId
        +UUID matchId
        +UUID raisedById
        +String description
        +Enum status
        +DateTime raisedDate
        +DateTime resolvedDate
        +String resolutionDetails
    }

    User "1" -- "0..1" SenderProfile : has
    User "1" -- "0..1" TravelerProfile : has
    User "1" -- "*" Parcel : creates
    User "1" -- "*" Trip : creates
    User "1" -- "*" Transaction : initiates
    User "1" -- "*" RatingReview : gives/receives
    User "1" -- "*" Message : sends/receives
    User "1" -- "*" Dispute : raises

    Parcel "1" -- "0..1" Match : is_part_of
    Trip "1" -- "0..1" Match : is_part_of
    Match "1" -- "*" Transaction : generates
    Match "1" -- "*" Message : relates_to
    Match "1" -- "*" Dispute : relates_to

    Parcel "1" -- "*" RatingReview : relates_to

```




## 5. Design System Decomposition and Interfaces (SysML BDD, IBD)

This section delves into the system's structure from a SysML perspective, focusing on the definition of system blocks and their internal composition and interconnections. SysML Block Definition Diagrams (BDD) define the types of blocks and their relationships, while Internal Block Diagrams (IBD) show the internal structure of a block in terms of its parts, ports, and connectors.

### 5.1. SysML Block Definition Diagram (BDD): System Context and Main Blocks

This BDD illustrates the top-level system context of Koidoo and defines its primary functional and physical blocks, along with their relationships (e.g., composition, association, generalization).

**Blocks:**
*   **KoidooSystem:** The top-level system block representing the entire Koidoo platform.
*   **UserApplication:** Abstract block for user-facing applications (MobileApp, WebPortal).
*   **MobileApp:** Specific block for iOS/Android mobile applications.
*   **WebPortal:** Specific block for the web-based interface.
*   **BackendServices:** Abstract block for all backend microservices.
*   **UserService:** Manages user-related functionalities.
*   **ParcelService:** Manages parcel-related functionalities.
*   **TripService:** Manages trip-related functionalities.
*   **MatchingService:** Handles the core matching logic.
*   **PaymentService:** Manages payment transactions.
*   **NotificationService:** Manages notifications.
*   **ChatService:** Manages in-app chat.
*   **DocumentService:** Manages document storage.
*   **Database:** Persistent data storage.
*   **ExternalService:** Abstract block for all external third-party integrations.
*   **IdentityVerificationService:** External service for KYC.
*   **PaymentGatewayService:** External service for payment processing.
*   **MappingService:** External service for location and mapping.
*   **NotificationProviderService:** External service for sending notifications (e.g., FCM, Twilio).

```d2
direction: right

KoidooSystem: {
  shape: rectangle
  label: "Koidoo System"
}

UserApplication: {
  shape: rectangle
  label: "User Application"
}

MobileApp: {
  shape: rectangle
  label: "Mobile App"
}

WebPortal: {
  shape: rectangle
  label: "Web Portal"
}

BackendServices: {
  shape: rectangle
  label: "Backend Services"
}

UserService: {
  shape: rectangle
  label: "User Service"
}

ParcelService: {
  shape: rectangle
  label: "Parcel Service"
}

TripService: {
  shape: rectangle
  label: "Trip Service"
}

MatchingService: {
  shape: rectangle
  label: "Matching Service"
}

PaymentService: {
  shape: rectangle
  label: "Payment Service"
}

NotificationService: {
  shape: rectangle
  label: "Notification Service"
}

ChatService: {
  shape: rectangle
  label: "Chat Service"
}

DocumentService: {
  shape: rectangle
  label: "Document Service"
}

Database: {
  shape: cylinder
  label: "Database"
}

ExternalService: {
  shape: rectangle
  label: "External Service"
}

IdentityVerificationService: {
  shape: rectangle
  label: "Identity Verification Service"
}

PaymentGatewayService: {
  shape: rectangle
  label: "Payment Gateway Service"
}

MappingService: {
  shape: rectangle
  label: "Mapping Service"
}

NotificationProviderService: {
  shape: rectangle
  label: "Notification Provider Service"
}

KoidooSystem -> UserApplication: { label: "composes", style: { stroke-dash: 2 } }
KoidooSystem -> BackendServices: { label: "composes", style: { stroke-dash: 2 } }
KoidooSystem -> ExternalService: { label: "uses", style: { stroke-dash: 2 } }

UserApplication -> MobileApp: { label: "generalizes", style: { stroke-dash: 2 } }
UserApplication -> WebPortal: { label: "generalizes", style: { stroke-dash: 2 } }

BackendServices -> UserService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> ParcelService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> TripService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> MatchingService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> PaymentService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> NotificationService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> ChatService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> DocumentService: { label: "composes", style: { stroke-dash: 2 } }
BackendServices -> Database: { label: "uses", style: { stroke-dash: 2 } }

ExternalService -> IdentityVerificationService: { label: "generalizes", style: { stroke-dash: 2 } }
ExternalService -> PaymentGatewayService: { label: "generalizes", style: { stroke-dash: 2 } }
ExternalService -> MappingService: { label: "generalizes", style: { stroke-dash: 2 } }
ExternalService -> NotificationProviderService: { label: "generalizes", style: { stroke-dash: 2 } }

UserService -> IdentityVerificationService: { label: "integrates with" }
PaymentService -> PaymentGatewayService: { label: "integrates with" }
MatchingService -> MappingService: { label: "integrates with" }
NotificationService -> NotificationProviderService: { label: "integrates with" }

```

### 5.2. SysML Internal Block Diagram (IBD): Backend Services

This IBD shows the internal structure of the `BackendServices` block, detailing its constituent parts (microservices), their ports, and the connectors that define their interactions. This provides a more granular view of the backend's internal communication.

**Parts (Microservices):**
*   `userSvc: UserService`
*   `parcelSvc: ParcelService`
*   `tripSvc: TripService`
*   `matchingSvc: MatchingService`
*   `paymentSvc: PaymentService`
*   `notificationSvc: NotificationService`
*   `chatSvc: ChatService`
*   `documentSvc: DocumentService`
*   `db: Database`

**Ports and Interfaces:**
*   Each service exposes an API (e.g., `apiPort`) for external communication (from API Gateway) and internal communication (between services).
*   Database access is through a `dbPort`.

**Connectors:**
*   Represent communication pathways (e.g., RESTful API calls, message queues).

```d2
direction: right

BackendServices: {
  shape: rectangle
  label: "Backend Services"
  userSvc: {
    shape: rectangle
    label: "User Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  parcelSvc: {
    shape: rectangle
    label: "Parcel Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  tripSvc: {
    shape: rectangle
    label: "Trip Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  matchingSvc: {
    shape: rectangle
    label: "Matching Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
      parcelApi: { side: left, shape: circle, label: "Parcel API" }
      tripApi: { side: left, shape: circle, label: "Trip API" }
      notifApi: { side: right, shape: circle, label: "Notif API" }
    }
  }
  paymentSvc: {
    shape: rectangle
    label: "Payment Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  notificationSvc: {
    shape: rectangle
    label: "Notification Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  chatSvc: {
    shape: rectangle
    label: "Chat Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  documentSvc: {
    shape: rectangle
    label: "Document Service"
    ports: {
      api: { side: right, shape: circle, label: "API" }
      db: { side: bottom, shape: circle, label: "DB" }
    }
  }
  db: {
    shape: cylinder
    label: "PostgreSQL DB"
  }

  userSvc.db -> db
  parcelSvc.db -> db
  tripSvc.db -> db
  matchingSvc.db -> db
  paymentSvc.db -> db
  notificationSvc.db -> db
  chatSvc.db -> db
  documentSvc.db -> db

  matchingSvc.parcelApi -> parcelSvc.api
  matchingSvc.tripApi -> tripSvc.api
  matchingSvc.notifApi -> notificationSvc.api

  chatSvc.api -> userSvc.api: { label: "User Info" }
  paymentSvc.api -> userSvc.api: { label: "User Info" }

}
```




## 6. Model System Behavior (SysML Activity, Sequence Diagrams)

This section describes the dynamic behavior of the Koidoo system from a SysML perspective, focusing on the flow of control and data between system blocks and their internal parts. SysML Activity Diagrams are used to model the sequence of actions and decisions, while SysML Sequence Diagrams illustrate the time-ordered exchange of messages between interacting blocks.

### 6.1. SysML Activity Diagram: Parcel Delivery Lifecycle

This SysML Activity Diagram models the end-to-end lifecycle of a parcel delivery within the Koidoo system, highlighting the activities performed by different system blocks and external services. It provides a comprehensive view of the process from parcel creation to final delivery and payment release.

```d2
direction: right

KoidooSystem: {
  shape: rectangle
  label: "Koidoo System"
  SenderApp: {
    shape: rectangle
    label: "Sender Application"
  }
  TravelerApp: {
    shape: rectangle
    label: "Traveler Application"
  }
  BackendAPI: {
    shape: rectangle
    label: "Backend API"
  }
  ParcelService: {
    shape: rectangle
    label: "Parcel Service"
  }
  TripService: {
    shape: rectangle
    label: "Trip Service"
  }
  MatchingService: {
    shape: rectangle
    label: "Matching Service"
  }
  PaymentService: {
    shape: rectangle
    label: "Payment Service"
  }
  NotificationService: {
    shape: rectangle
    label: "Notification Service"
  }
  ExternalPaymentGateway: {
    shape: rectangle
    label: "External Payment Gateway"
  }
}

start -> CreateParcelRequest
CreateParcelRequest -> SubmitParcelRequest
SubmitParcelRequest -> FindMatchingTraveler
FindMatchingTraveler -> OfferParcelToTraveler
OfferParcelToTraveler -> AcceptParcelOffer
AcceptParcelOffer -> ArrangePickup
ArrangePickup -> ParcelPickedUp
ParcelPickedUp -> InTransit
InTransit -> ParcelDelivered
ParcelDelivered -> ConfirmDelivery
ConfirmDelivery -> ReleasePayment
ReleasePayment -> end

CreateParcelRequest: { label: "Create Parcel Request" }
SubmitParcelRequest: { label: "Submit Parcel Request" }
FindMatchingTraveler: { label: "Find Matching Traveler" }
OfferParcelToTraveler: { label: "Offer Parcel to Traveler" }
AcceptParcelOffer: { label: "Accept Parcel Offer" }
ArrangePickup: { label: "Arrange Pickup" }
ParcelPickedUp: { label: "Parcel Picked Up" }
InTransit: { label: "In Transit" }
ParcelDelivered: { label: "Parcel Delivered" }
ConfirmDelivery: { label: "Confirm Delivery" }
ReleasePayment: { label: "Release Payment" }

KoidooSystem.SenderApp -> CreateParcelRequest
CreateParcelRequest -> KoidooSystem.BackendAPI
KoidooSystem.BackendAPI -> SubmitParcelRequest
SubmitParcelRequest -> KoidooSystem.ParcelService
KoidooSystem.ParcelService -> FindMatchingTraveler
FindMatchingTraveler -> KoidooSystem.MatchingService
KoidooSystem.MatchingService -> OfferParcelToTraveler
OfferParcelToTraveler -> KoidooSystem.TravelerApp
KoidooSystem.TravelerApp -> AcceptParcelOffer
AcceptParcelOffer -> KoidooSystem.BackendAPI
KoidooSystem.BackendAPI -> ArrangePickup
ArrangePickup -> KoidooSystem.TravelerApp
KoidooSystem.TravelerApp -> ParcelPickedUp
ParcelPickedUp -> KoidooSystem.BackendAPI
KoidooSystem.BackendAPI -> InTransit
InTransit -> KoidooSystem.BackendAPI
KoidooSystem.BackendAPI -> ParcelDelivered
ParcelDelivered -> KoidooSystem.SenderApp
KoidooSystem.SenderApp -> ConfirmDelivery
ConfirmDelivery -> KoidooSystem.BackendAPI
KoidooSystem.BackendAPI -> ReleasePayment
ReleasePayment -> KoidooSystem.PaymentService
KoidooSystem.PaymentService -> ExternalPaymentGateway
ExternalPaymentGateway -> ReleasePayment

```

### 6.2. SysML Sequence Diagram: Payment Processing with Escrow

This SysML Sequence Diagram illustrates the detailed message exchange during the payment processing, focusing on the escrow mechanism and the interactions between the Payment Service, External Payment Gateway, and other relevant system blocks.

```d2
direction: right

KoidooSystem: {
  shape: rectangle
  label: "Koidoo System"
  SenderApp: {
    shape: rectangle
    label: "Sender Application"
  }
  PaymentService: {
    shape: rectangle
    label: "Payment Service"
  }
  ExternalPaymentGateway: {
    shape: rectangle
    label: "External Payment Gateway"
  }
  TravelerApp: {
    shape: rectangle
    label: "Traveler Application"
  }
  ParcelService: {
    shape: rectangle
    label: "Parcel Service"
  }
}

sequenceDiagram
  SenderApp -> PaymentService: InitiatePayment(parcelId, amount)
  PaymentService -> ExternalPaymentGateway: RequestEscrow(senderAccount, amount)
  ExternalPaymentGateway --> PaymentService: EscrowInitiated(transactionId)
  PaymentService -> SenderApp: PaymentConfirmation(transactionId, status: "Held in Escrow")

  note over SenderApp, TravelerApp: Parcel Delivery Process

  TravelerApp -> ParcelService: ConfirmDelivery(parcelId)
  ParcelService --> PaymentService: DeliveryConfirmed(parcelId)
  PaymentService -> ExternalPaymentGateway: ReleaseEscrow(transactionId, travelerAccount)
  ExternalPaymentGateway --> PaymentService: EscrowReleased(transactionId)
  PaymentService -> TravelerApp: PayoutConfirmation(amount)
  PaymentService -> SenderApp: DeliveryCompleteNotification()

  alt Dispute Occurs
    SenderApp -> PaymentService: RaiseDispute(parcelId, reason)
    PaymentService -> ExternalPaymentGateway: HoldEscrow(transactionId)
    ExternalPaymentGateway --> PaymentService: EscrowHeld(transactionId)
    PaymentService -> SenderApp: DisputeRegistered()
    note over PaymentService: Initiate Dispute Resolution Process
    PaymentService -> ExternalPaymentGateway: (After Resolution) ReleaseEscrow(transactionId, resolvedAccount)
    ExternalPaymentGateway --> PaymentService: EscrowReleased(transactionId)
  end

```




## 7. Establish Requirements Traceability (SysML Requirements Diagram)

This section focuses on ensuring that all identified requirements are addressed by the system design. A SysML Requirements Diagram visually links requirements to the design elements that satisfy them, providing clear traceability and facilitating impact analysis for changes.

**Key Requirements (Examples):**
*   **REQ-FUNC-001: User Registration:** The system shall allow users to register and create a profile.
*   **REQ-FUNC-002: Identity Verification:** The system shall require users to verify their identity (KYC).
*   **REQ-FUNC-003: Parcel Request Creation:** The system shall enable senders to create parcel requests with detailed specifications.
*   **REQ-FUNC-004: Trip Publication:** The system shall enable travelers to publish their trip details and available luggage space.
*   **REQ-FUNC-005: Parcel-Trip Matching:** The system shall automatically match parcel requests with suitable trips.
*   **REQ-FUNC-006: Secure Payment Processing:** The system shall facilitate secure escrow-based payment processing.
*   **REQ-NONFUNC-001: Scalability:** The system shall be scalable to support 100,000 concurrent users.
*   **REQ-NONFUNC-002: Security:** The system shall protect user data and transactions from unauthorized access.
*   **REQ-NONFUNC-003: Performance:** The system shall respond to user requests within 2 seconds for 90% of interactions.

**Design Elements (Examples):**
*   **User Service:** Responsible for user registration and profile management.
*   **Identity Verification Service:** Integrates with external KYC providers.
*   **Parcel Service:** Manages parcel data and requests.
*   **Trip Service:** Manages trip data and publications.
*   **Matching Service:** Implements the matching algorithm.
*   **Payment Service:** Handles payment logic and escrow.
*   **API Gateway:** Ensures secure and scalable access to backend services.
*   **PostgreSQL DB:** Provides persistent and scalable data storage.

```d2
direction: right

KoidooSystem: {
  shape: rectangle
  label: "Koidoo System"
}

requirements: {
  shape: rectangle
  label: "Requirements"
  REQ_FUNC_001: {
    shape: rectangle
    label: "REQ-FUNC-001: User Registration"
  }
  REQ_FUNC_002: {
    shape: rectangle
    label: "REQ-FUNC-002: Identity Verification"
  }
  REQ_FUNC_003: {
    shape: rectangle
    label: "REQ-FUNC-003: Parcel Request Creation"
  }
  REQ_FUNC_004: {
    shape: rectangle
    label: "REQ-FUNC-004: Trip Publication"
  }
  REQ_FUNC_005: {
    shape: rectangle
    label: "REQ-FUNC-005: Parcel-Trip Matching"
  }
  REQ_FUNC_006: {
    shape: rectangle
    label: "REQ-FUNC-006: Secure Payment Processing"
  }
  REQ_NONFUNC_001: {
    shape: rectangle
    label: "REQ-NONFUNC-001: Scalability"
  }
  REQ_NONFUNC_002: {
    shape: rectangle
    label: "REQ-NONFUNC-002: Security"
  }
  REQ_NONFUNC_003: {
    shape: rectangle
    label: "REQ-NONFUNC-003: Performance"
  }
}

design_elements: {
  shape: rectangle
  label: "Design Elements"
  UserService: {
    shape: rectangle
    label: "User Service"
  }
  IdentityVerificationService: {
    shape: rectangle
    label: "Identity Verification Service"
  }
  ParcelService: {
    shape: rectangle
    label: "Parcel Service"
  }
  TripService: {
    shape: rectangle
    label: "Trip Service"
  }
  MatchingService: {
    shape: rectangle
    label: "Matching Service"
  }
  PaymentService: {
    shape: rectangle
    label: "Payment Service"
  }
  APIGateway: {
    shape: rectangle
    label: "API Gateway"
  }
  PostgreSQLDB: {
    shape: cylinder
    label: "PostgreSQL DB"
  }
}

REQ_FUNC_001 -> UserService: { label: "satisfies" }
REQ_FUNC_002 -> IdentityVerificationService: { label: "satisfies" }
REQ_FUNC_003 -> ParcelService: { label: "satisfies" }
REQ_FUNC_004 -> TripService: { label: "satisfies" }
REQ_FUNC_005 -> MatchingService: { label: "satisfies" }
REQ_FUNC_006 -> PaymentService: { label: "satisfies" }

REQ_NONFUNC_001 -> APIGateway: { label: "refines" }
REQ_NONFUNC_001 -> PostgreSQLDB: { label: "refines" }
REQ_NONFUNC_002 -> APIGateway: { label: "refines" }
REQ_NONFUNC_002 -> UserService: { label: "refines" }
REQ_NONFUNC_003 -> APIGateway: { label: "refines" }
REQ_NONFUNC_003 -> MatchingService: { label: "refines" }

```




## 8. Conclusion

This document has provided a comprehensive design and conception of the Koidoo peer-to-peer parcel delivery platform, leveraging both UML and SysML diagrams. From high-level use cases and component interactions to detailed data models, system decomposition, behavioral modeling, and requirements traceability, every effort has been made to create a clear and actionable blueprint for development. This detailed design ensures that the Koidoo system will be robust, scalable, secure, and aligned with its core business objectives, ultimately delivering a seamless and trustworthy experience for all users.

## 9. References

*   [1] Koidoo Project Overview: [./wakadoo_improved_overview.md](./wakadoo_improved_overview.md)
*   [2] Koidoo Technical Architecture Document: [./koidoo_technical_architecture.md](./koidoo_technical_architecture.md)
*   [3] UML (Unified Modeling Language) Official Site: [https://www.uml.org/](https://www.uml.org/)
*   [4] SysML (Systems Modeling Language) Official Site: [https://www.omg.org/sysml/](https://www.omg.org/sysml/)


