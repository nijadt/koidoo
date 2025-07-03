# Koidoo: Requirements Specification

## 1. Introduction

This document outlines the complete set of requirements for the Koidoo peer-to-peer parcel delivery platform. It serves as a foundational reference for development, testing, and project management, ensuring that all stakeholders have a clear and unambiguous understanding of the system's expected functionalities and performance characteristics. These requirements are derived from the Koidoo Project Overview, Technical Architecture Document, and the detailed System Design and Conception document [1, 2, 3].

## 2. Functional Requirements (FR)

Functional requirements define what the system must *do*. They describe the interactions between the system and its users, as well as the system's behavior in response to specific inputs or conditions. These requirements are primarily derived from the Use Cases, Activity Diagrams, and Sequence Diagrams presented in the system design.

### 2.1. User Management

*   **FR-UM-001: User Registration:** The system shall allow new users to register an account by providing a unique email address, username, and password. Upon successful registration, a confirmation email shall be sent to the user.
*   **FR-UM-002: User Login:** The system shall allow registered users to log in using their email/username and password. The system shall support multi-factor authentication (MFA) for enhanced security.
*   **FR-UM-003: Profile Management:** Users shall be able to view, update, and delete their personal profile information, including contact details, profile picture, and address.
*   **FR-UM-004: Role Assignment:** The system shall assign specific roles (Sender, Traveler, Admin) to users based on their registration type and verified status.
*   **FR-UM-005: Password Reset:** The system shall provide a mechanism for users to securely reset their forgotten passwords.

### 2.2. Identity Verification (KYC)

*   **FR-KYC-001: Mandatory Identity Verification:** All users (Senders and Travelers) shall undergo a mandatory identity verification (KYC) process before they can initiate or accept transactions.
*   **FR-KYC-002: Document Upload:** The system shall allow users to securely upload identity documents (e.g., passport, national ID) for verification.
*   **FR-KYC-003: Biometric Verification:** The system shall integrate with a third-party service to perform biometric face matching against the uploaded identity document.
*   **FR-KYC-004: Verification Status Tracking:** The system shall track and display the KYC status (Pending, Verified, Rejected) for each user.
*   **FR-KYC-005: Admin Review:** Platform administrators shall be able to review and manually approve or reject KYC submissions.

### 2.3. Parcel Management

*   **FR-PM-001: Create Parcel Request:** Senders shall be able to create a new parcel request by providing:
    *   Origin and destination addresses.
    *   Parcel dimensions (length, width, height) and weight.
    *   Detailed description of contents and declared value.
    *   Desired delivery deadline.
    *   Pickup and drop-off preferences (meet-up, relay point, door-to-door).
*   **FR-PM-002: Parcel Photo Upload:** Senders shall be able to upload optional photos of the parcel.
*   **FR-PM-003: Prohibited Items Check:** The system shall warn senders about prohibited items based on content description and destination country regulations.
*   **FR-PM-004: View Parcel Details:** Senders, Travelers (for matched parcels), and Admins shall be able to view detailed information about a parcel request.
*   **FR-PM-005: Update Parcel Status:** Senders and Travelers shall be able to update the status of a parcel (e.g., picked up, in transit, delivered).
*   **FR-PM-006: Cancel Parcel Request:** Senders shall be able to cancel a parcel request before it is matched or picked up.

### 2.4. Trip Management

*   **FR-TM-001: Publish Trip:** Travelers shall be able to publish details of their upcoming trips, including:
    *   Departure and arrival locations.
    *   Departure and arrival dates/times.
    *   Available luggage weight and volume capacity.
    *   Mode of transport (flight, train, car).
*   **FR-TM-002: Manage Trip Details:** Travelers shall be able to view, update, and cancel their published trips.
*   **FR-TM-003: Proof of Travel Upload:** Travelers shall be required to upload proof of travel (e.g., flight tickets) for verification.

### 2.5. Matching and Negotiation

*   **FR-MN-001: Intelligent Matching:** The system shall automatically match parcel requests with suitable traveler trips based on origin, destination, dates, and available capacity.
*   **FR-MN-002: Browse Parcel Offers:** Travelers shall be able to browse a curated list of parcel offers that match their published trips.
*   **FR-MN-003: Accept Parcel Offer:** Travelers shall be able to accept a specific parcel offer.
*   **FR-MN-004: Negotiate Terms:** Senders and Travelers shall be able to negotiate specific terms of a matched parcel (e.g., delivery fee, exact pickup/drop-off time) via the in-app chat.
*   **FR-MN-005: Match Confirmation:** The system shall confirm a match once both Sender and Traveler agree to the terms.

### 2.6. Tracking and Notifications

*   **FR-TN-001: Real-time Parcel Tracking:** Senders and Recipients shall be able to track the real-time status and location of a parcel from pickup to delivery.
*   **FR-TN-002: Milestone Notifications:** The system shall send automated notifications (push, SMS, email) to relevant users for key parcel milestones (e.g., matched, picked up, in transit, delivered, dispute raised).
*   **FR-TN-003: Chat Notifications:** Users shall receive notifications for new messages in the in-app chat.
*   **FR-TN-004: Customizable Notifications:** Users shall be able to customize their notification preferences.

### 2.7. Payment Processing

*   **FR-PP-001: Secure Escrow System:** The system shall hold payment funds in a secure escrow account until successful delivery of the parcel is confirmed.
*   **FR-PP-002: Payment Initiation:** Senders shall initiate payment for a parcel upon match confirmation.
*   **FR-PP-003: Payout to Traveler:** Funds shall be released from escrow to the Traveler upon confirmed delivery by the Sender/Recipient.
*   **FR-PP-004: Commission Deduction:** The system shall automatically deduct Koidoo's commission from the transaction amount before payout.
*   **FR-PP-005: Transaction History:** Users shall be able to view their complete transaction history.
*   **FR-PP-006: Optional Insurance Purchase:** Senders shall be able to purchase optional insurance for their parcels during the request creation process.

### 2.8. Communication and Feedback

*   **FR-CF-001: In-App Chat:** The system shall provide a secure in-app messaging system for direct communication between Senders and Travelers for matched parcels.
*   **FR-CF-002: Mutual Ratings and Reviews:** Users shall be able to rate and review each other after a completed transaction.
*   **FR-CF-003: Dispute Resolution:** The system shall provide a mechanism for users to raise disputes regarding a transaction. Administrators shall be able to mediate and resolve disputes.

### 2.9. Administrative Functions

*   **FR-AF-001: User Management (Admin):** Administrators shall be able to view, manage (activate/deactivate), and audit user accounts.
*   **FR-AF-002: Parcel/Trip Management (Admin):** Administrators shall be able to view and manage all parcel requests and trip publications.
*   **FR-AF-003: Content Moderation:** Administrators shall be able to monitor and moderate in-app chat content and parcel descriptions for prohibited items or inappropriate language.
*   **FR-AF-004: System Configuration:** Administrators shall be able to configure system parameters (e.g., commission rates, prohibited items list).
*   **FR-AF-005: Reporting and Analytics:** Administrators shall have access to reports and dashboards for key operational metrics and business intelligence.

## 3. Non-Functional Requirements (NFR)

Non-functional requirements define how the system *performs*. They specify criteria that can be used to judge the operation of a system, rather than specific behaviors. These are crucial for the overall quality and success of the Koidoo platform.

### 3.1. Performance

*   **NFR-PERF-001: Response Time:** The system shall respond to 90% of user requests within 2 seconds under normal load conditions.
*   **NFR-PERF-002: Matching Speed:** The intelligent matching algorithm shall identify and present suitable matches within 5 seconds of a new parcel request or trip publication.
*   **NFR-PERF-003: Transaction Processing:** Payment transactions (initiation, escrow, release) shall be processed within 500 milliseconds, excluding external payment gateway processing time.
*   **NFR-PERF-004: Scalability:** The system shall be capable of supporting 100,000 concurrent users and processing 500,000 transactions per month without significant degradation in performance.
*   **NFR-PERF-005: Load Handling:** The system shall gracefully handle peak loads up to 2x average load for short durations (e.g., 1 hour) with no more than 5% increase in response time.

### 3.2. Security

*   **NFR-SEC-001: Data Encryption (Transit):** All data transmitted between client applications, backend services, and external integrations shall be encrypted using TLS 1.2 or higher.
*   **NFR-SEC-002: Data Encryption (Rest):** All sensitive user data (e.g., personal identifiable information, payment details) stored in the database and cloud storage shall be encrypted at rest.
*   **NFR-SEC-003: Authentication:** The system shall implement robust authentication mechanisms (e.g., OAuth 2.0, JWTs) to verify user identities.
*   **NFR-SEC-004: Authorization:** The system shall enforce role-based access control (RBAC) to ensure users can only access authorized functionalities and data.
*   **NFR-SEC-005: Input Validation:** The system shall perform comprehensive input validation and sanitization to prevent common web vulnerabilities (e.g., SQL injection, XSS).
*   **NFR-SEC-006: Fraud Detection:** The system shall incorporate automated and manual fraud detection mechanisms to identify and flag suspicious activities.
*   **NFR-SEC-007: Compliance:** The system shall comply with relevant data protection regulations (e.g., GDPR, CCPA) and financial industry standards (e.g., PCI DSS for payment handling).
*   **NFR-SEC-008: Audit Trails:** The system shall maintain comprehensive audit trails for all critical user actions and administrative activities.

### 3.3. Reliability and Availability

*   **NFR-REL-001: Uptime:** The system shall maintain an uptime of 99.9% (excluding scheduled maintenance).
*   **NFR-REL-002: Data Backup and Recovery:** The system shall implement automated daily backups of all critical data with a recovery point objective (RPO) of 24 hours and a recovery time objective (RTO) of 4 hours.
*   **NFR-REL-003: Error Handling:** The system shall provide informative error messages to users and log detailed error information for debugging.
*   **NFR-REL-004: Fault Tolerance:** Critical services shall be designed with redundancy and failover mechanisms to ensure continuous operation in case of component failure.

### 3.4. Usability and User Experience (UX)

*   **NFR-UX-001: Intuitive Interface:** The user interface shall be intuitive and easy to navigate for both Senders and Travelers, requiring minimal training.
*   **NFR-UX-002: Mobile Responsiveness:** The mobile applications shall provide a consistent and optimized user experience across various iOS and Android devices and screen sizes.
*   **NFR-UX-003: Clear Feedback:** The system shall provide clear and timely feedback to users for all actions, indicating success, failure, or progress.
*   **NFR-UX-004: Accessibility:** The system shall adhere to WCAG 2.1 AA guidelines to ensure accessibility for users with disabilities.
*   **NFR-UX-005: Localization:** The system shall support multiple languages (initially English and French) and adapt to regional date/time and currency formats.

### 3.5. Maintainability and Extensibility

*   **NFR-MAINT-001: Modular Design:** The system shall be designed with a modular, microservices-based architecture to facilitate independent development, deployment, and maintenance of components.
*   **NFR-MAINT-002: Code Quality:** The codebase shall adhere to established coding standards, best practices, and be well-documented.
*   **NFR-MAINT-003: Testability:** All system components shall be designed to be easily testable, with comprehensive unit, integration, and end-to-end tests.
*   **NFR-MAINT-004: Configurability:** Key business rules and parameters (e.g., commission rates, notification templates) shall be configurable without requiring code changes.
*   **NFR-MAINT-005: API Documentation:** All internal and external APIs shall be well-documented using standards like OpenAPI/Swagger.

### 3.6. Portability

*   **NFR-PORT-001: Cloud Agnostic (Partial):** The system architecture shall minimize vendor lock-in where feasible, allowing for potential migration to different cloud providers in the future.
*   **NFR-PORT-002: Cross-Platform Mobile:** The mobile application shall be deployable on both iOS and Android platforms from a single codebase.

## 4. User Interface (UI) Requirements

UI requirements specify the visual and interactive aspects of the system's user interface. While detailed UI/UX design will be a separate effort, these high-level requirements guide the overall look and feel.

### 4.1. General UI Requirements

*   **UI-GEN-001: Consistent Design:** The UI shall maintain a consistent design language, color scheme, typography, and component styling across all platforms (mobile apps, web portal).
*   **UI-GEN-002: Responsive Layouts:** All UI elements shall adapt gracefully to different screen sizes and orientations (portrait/landscape).
*   **UI-GEN-003: Intuitive Navigation:** Navigation elements shall be clearly visible, logically organized, and easy to use.
*   **UI-GEN-004: Visual Feedback:** The UI shall provide clear visual feedback for user actions (e.g., button presses, form submissions, loading states).

### 4.2. Specific UI Elements (Examples)

*   **UI-SPEC-001: User Dashboard:** A personalized dashboard for Senders and Travelers displaying relevant information (active parcels/trips, notifications, earnings/expenses).
*   **UI-SPEC-002: Parcel Request Form:** A multi-step form for creating parcel requests with clear input fields, validation, and progress indicators.
*   **UI-SPEC-003: Trip Publication Form:** A form for travelers to publish trip details, including date pickers, location selectors, and capacity inputs.
*   **UI-SPEC-004: Chat Interface:** A real-time messaging interface within matched transactions, supporting text and potentially image sharing.
*   **UI-SPEC-005: Tracking Map:** An interactive map displaying the current location and route of a parcel.
*   **UI-SPEC-006: Ratings and Reviews Form:** A simple and clear form for submitting ratings and textual reviews.
*   **UI-SPEC-007: Admin Dashboard:** A secure web-based dashboard for administrators with tools for user management, content moderation, and reporting.

## 5. External Interface Requirements (EIR)

External interface requirements define the interactions between the Koidoo system and external systems or third-party services.

*   **EIR-PAY-001: Payment Gateway Integration:** The system shall integrate with a payment gateway (e.g., Stripe Connect, MangoPay) to handle:
    *   Secure collection of funds from Senders.
    *   Holding funds in escrow.
    *   Secure payouts to Travelers.
    *   Processing Koidoo's commission.
    *   Handling refunds and chargebacks.
*   **EIR-IDV-001: Identity Verification Service Integration:** The system shall integrate with an identity verification service (e.g., Onfido, Veriff) for:
    *   Document scanning and OCR.
    *   Biometric facial recognition.
    *   Verification status callbacks.
*   **EIR-MAP-001: Mapping Service Integration:** The system shall integrate with a mapping service (e.g., Mapbox, Google Places API) for:
    *   Address auto-completion and validation.
    *   Geocoding and reverse geocoding.
    *   Route calculation and optimization.
    *   Displaying interactive maps.
*   **EIR-NOTIF-001: Notification Service Integration:** The system shall integrate with external notification providers (e.g., Firebase Cloud Messaging, Twilio SMS, SendGrid for email) for:
    *   Sending push notifications to mobile devices.
    *   Sending SMS messages.
    *   Sending transactional emails.
*   **EIR-INS-001: Insurance Provider API Integration (Optional):** The system shall have the capability to integrate with third-party insurance providers (e.g., CoverGenius, Baloise) to offer optional parcel insurance.

## 6. Data Requirements (DR)

Data requirements specify the characteristics of the data managed by the system, including data types, storage, and integrity.

*   **DR-STOR-001: Persistent Storage:** All critical application data (user profiles, parcel details, trip details, transaction records, messages, ratings) shall be stored persistently in a relational database (PostgreSQL).
*   **DR-STOR-002: Media Storage:** User-uploaded media files (e.g., parcel photos, profile pictures, ID documents) shall be stored in a scalable and secure cloud storage solution (e.g., AWS S3, Cloudflare R2).
*   **DR-INT-001: Data Integrity:** The system shall enforce data integrity constraints (e.g., foreign keys, unique constraints) to ensure data consistency and accuracy.
*   **DR-INT-002: Data Validation:** All data inputs shall be validated against predefined rules and formats before storage.
*   **DR-RET-001: Data Retention:** The system shall define and adhere to data retention policies for different types of data, complying with legal and regulatory requirements.

## 7. References

*   [1] Koidoo Project Overview: [./wakadoo_improved_overview.md](./wakadoo_improved_overview.md)
*   [2] Koidoo Technical Architecture Document: [./koidoo_technical_architecture.md](./koidoo_technical_architecture.md)
*   [3] Koidoo: System Design and Conception: [./koidoo_design_conception.md](./koidoo_design_conception.md)


