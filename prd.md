# Product Requirements Document: Digital Wallet MVP

## 1. Prioritized Feature List

### Must-Have (MVP Scope)

-   **User Registration & Authentication**: Secure sign-up, login, and
    session management (e.g., JWT).

-   **Wallet Creation**: Automatic provisioning of a default digital
    wallet upon successful user registration.

-   **Add Money (Simulated)**: Ability for users to credit their wallet
    balance (mocking a successful bank/card deposit).

-   **Withdraw Money (Simulated)**: Ability for users to debit their
    wallet balance (mocking a bank withdrawal).

-   **Peer-to-Peer (P2P) Transfer**: Instant, atomic funds transfer
    between two registered users using a unique identifier (email or
    phone number).

-   **Transaction History (Passbook)**: A paginated, chronologically
    ordered ledger showing all incoming and outgoing transactions
    (transfers, deposits, withdrawals) for a user.

-   **Idempotent Payments**: API-level guarantee using idempotency keys
    to ensure duplicate payment requests (e.g., due to network retries)
    result in only one state change.

### Nice-to-Have (Post-MVP / Fast Follows)

-   **Admin Oversight API/Dashboard**: Endpoints for administrators to
    view system-wide metrics, flagged transactions, and user balances.

-   **Scheduled Reconciliation**: Automated background cron jobs to
    verify ledger integrity and detect anomalies between aggregate user
    balances and the sum of transaction logs.

-   **Transaction Rollbacks/Refunds**: Mechanisms to safely reverse a
    completed P2P transfer.

-   **Webhooks**: Event notifications for third-party integrations.

------------------------------------------------------------------------

## 2. Core Business Rules

-   **Wallet Allocation**: A user can have exactly **one (1)** active
    wallet at any given time.

-   **Currency Support**: Single currency only (e.g., USD) for the MVP.

-   **Minimum Balance Policy**: The minimum required balance to maintain
    the account is **\$0**.

-   **Negative Balance**: A wallet balance **cannot go below zero**. No
    overdraft facilities or credit lines are provided.

-   **Insufficient Funds**: Any withdrawal or transfer request that
    exceeds the current available balance will be rejected immediately
    with a structured "Insufficient Funds" error.

-   **Transfer Limits**:

    -   **Maximum per-transaction limit**: \$1,000.
    -   **Maximum daily transfer limit**: \$5,000 per rolling 24-hour
        window.

------------------------------------------------------------------------

## 3. User Roles & Permissions

### USER

-   **Capabilities**: Can view their own profile, wallet balance, and
    transaction history. Can add simulated funds, withdraw funds, and
    initiate P2P transfers to other users.

-   **Restrictions**: Cannot access, query, or modify other users' data,
    wallets, or transaction histories.

### ADMIN

-   **Capabilities**: Can view all user profiles, wallet balances, and
    the system-wide transaction ledger. Can freeze or unfreeze user
    accounts/wallets in cases of suspicious activity.

-   **Restrictions**: Cannot initiate transfers on behalf of a user.
    Cannot arbitrarily modify balances without a system-logged
    adjustment transaction (even then, direct balance manipulation
    should be restricted).

------------------------------------------------------------------------

## 4. Explicit Non-Goals for MVP

-   **Real Payment Gateway Integration**: We will not integrate with
    Stripe, PayPal, Plaid, or real banking networks. All external fund
    movements (Add/Withdraw) are simulated.

-   **Multi-Currency & FX**: No handling of foreign exchange rates,
    cross-border payments, or multiple base currencies.

-   **KYC/AML Compliance Engine**: Complex identity verification (e.g.,
    uploading ID documents) and regulatory anti-money laundering checks
    are out of scope.

-   **Notifications**: Push notifications, SMS, or email alerts for
    transaction receipts are not included in the MVP.

-   **Frontend/Mobile Application**: The MVP is strictly a backend API
    service. Client interactions will be validated via a Postman
    collection or Swagger UI.

------------------------------------------------------------------------

## 5. Definition of Done (DoD)

The Digital Wallet MVP is considered "Done" when the backend system is
fully implemented and deployed to a staging environment, exposing
documented (e.g., OpenAPI/Swagger) REST or GraphQL endpoints that
successfully fulfill all "Must-Have" features. The codebase must have at
least 80% unit and integration test coverage, with specific concurrency
tests proving that race conditions cannot result in double-spending or
negative balances. Additionally, all core business rules must be
rigorously enforced at the database level (e.g., constraints and ACID
transactions), and the system must pass a basic security review ensuring
strict role-based access control (RBAC) separating USER and ADMIN
capabilities.
