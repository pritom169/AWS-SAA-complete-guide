# AWS - Solution Architect Associate - A Complete Guide

## AWS Accounts - The Basics

An AWS Account serves as a container for both identities (users) and resources. When creating an AWS account, you must provide an account name (e.g., PROD), a unique email address (each account requires a distinct email), and a credit card for billing purposes.

Each AWS account has a root user, which is created during the account setup. The root user has full administrative control over all resources and configurations within that specific account. Access by the root user cannot be restricted.

The credit card associated with the account is used to pay for any billable resources or services consumed under that account.

### Identity and Access Management (IAM)

AWS Identity and Access Management (IAM) allows you to create and manage identities within an AWS account. These identities can include:

- Users
- Groups
- Roles

By default, new IAM identities have no permissions. Permissions must be explicitly granted to allow access to specific AWS resources or actions.

Since each AWS account is isolated, permissions assigned to a user or group are limited to that account. Cross-account access can be configured, but for simplicity, it can be assumed that permissions apply only within a single account.

By default, all access to AWS accounts and resources is denied, except for the root user, who has unrestricted access.

```mermaid
graph TD
    subgraph AWS Account [Your AWS Account]
        direction LR
        A[AWS Logo] --> B(Account ROOT USER)
        B -- "Has full control over THIS AWS Account" --> C{IAM}
        C -- "Manages Identity and Access Management" --> D[IAM Users, Groups, and Roles]
        D -- "Can have FULL or LIMITED permissions" --> E(S3 Bucket)
        D -- "Can have FULL or LIMITED permissions" --> F(EC2 Instance)
        E -- "Resources bill to" --> G[AWS Account Payment Method]
        F -- "Resources bill to" --> G
        H(Explosion - Impact of errors) -- "Can be contained by separate accounts" --> I(Pennywise - Bad actors)
        I --> H
    end

    J[When creating an AWS Account] --> K(Provide account name e.g. PROD)
    J --> L(Unique email address)
    J --> M(Credit Card)

    K --> A
    L --> B
    M --> G

    N[AWS Accounts can contain the impact of admin errors, or exploits by bad actors.]
    O["Use separate accounts for separate things (DEV, TEST, PROD) or teams/products/clients"]

    style A fill:#fff,stroke:#333,stroke-width:2px
    style B fill:#f9f,stroke:#333,stroke-width:2px
    style C fill:#ccf,stroke:#333,stroke-width:2px
    style D fill:#bbf,stroke:#333,stroke-width:2px
    style E fill:#afa,stroke:#333,stroke-width:2px
    style F fill:#faa,stroke:#333,stroke-width:2px
    style G fill:#fcf,stroke:#333,stroke-width:2px
    style H fill:#f00,stroke:#333,stroke-width:2px
    style I fill:#f90,stroke:#333,stroke-width:2px
    style J fill:#eee,stroke:#333,stroke-width:2px
    style K fill:#eee,stroke:#333,stroke-width:2px
    style L fill:#eee,stroke:#333,stroke-width:2px
    style M fill:#eee,stroke:#333,stroke-width:2px
    style N fill:#eee,stroke:#333,stroke-width:2px
    style O fill:#eee,stroke:#333,stroke-width:2px
```

### First AWS Account

When an AWS account is created, a **root user** is automatically generated. This root user has **full administrative control** over the entire account.

After creating the account, it is essential to **secure the root user** by enabling **Multi-Factor Authentication (MFA)**. MFA adds an extra layer of protection by requiring a time-based verification code (from a physical or virtual device) in addition to the standard username and password.

You should also **configure a budget** to monitor and control potential costs. This helps prevent unexpected charges from running services or misconfigurations.

As a best practice, avoid using the root user for daily operations. Instead, create an **IAM identity**—for example, an **IAM Admin user**—with the necessary administrative permissions. This IAM user will be used throughout the course.

Once the setup is complete, the same process will be repeated to create a **Production account**.

This structure will serve as the foundation for the rest of the course.

```mermaid
graph LR
    A[You]

    subgraph "1. AWS General (MANAGEMENT)"
        direction LR
        B(Account Root User)
        D[Budget]
        E[IAM]
        F(IAMADMIN)
        G[AWS]
        C{Mobile Phone}

        B -- MFA Token --> C
    end

    subgraph "2. AWS Production"
        direction LR
        H(Account Root User)
        J[Budget]
        K[IAM]
        L(IAMADMIN)
        M[AWS]
        I{Mobile Phone}

        H -- MFA Token --> I
    end

    %% Connections from You to the General account
    A --> B
    A --> D
    A --> E
```

### Gmail being Awesome

You don’t need to actually create new Gmail accounts — Gmail supports aliases that let you create effectively unlimited unique addresses that all deliver to one inbox.

If your address is alice@gmail.com, any email to alice+shopping@gmail.com, alice+github@gmail.com, alice+newsletters@gmail.com will arrive in alice@gmail.com. Use whatever tag you like after the +.

Banking information:

- pritom+banking@gmail.com
- pritom+jobapps+companyA@gmail.com

## Securing an AWS Account

Securing an AWS account requires a multi-layered approach that protects your root account and all user accounts from unauthorized access.

Your AWS root account is the most critical asset. It has unrestricted access to all AWS services and resources.

MFA adds an essential second layer of security beyond passwords. Without MFA, a static username and password alone provide insufficient protection—this is considered single-factor authentication.

```mermaid
graph TD
    A["👩 Julie<br/>julie@cats.com"] -->|Static Username<br/>+ Infrequently<br/>Changing Password| B["ONE FACTOR<br/>Something Julie<br/>KNOWS"]

    B -->|WITHOUT MFA| C["🔐 ACCOUNT ROOT USER<br/>(of this account)"]

    C -->|Can Access| D["☁️ AWS<br/>GENERAL ACCOUNT<br/>AWS SERVICES"]

    E["🔴 NO CODE or AGED CODE<br/>VULNERABLE"] -->|Brute Force<br/>Credential Stuffing| F["⚠️ Attacker"]

    F -->|Can Steal<br/>Username & Password| G["📌 Gains Access<br/>to Root User"]

    G -->|Full Control| D

    A2["👩 Julie<br/>julie@cats.com"] -->|Static Username<br/>+ Password<br/>+ MFA Code| H["TWO FACTORS<br/>Something Julie<br/>KNOWS + Something<br/>Julie HAS"]

    H -->|WITH MFA| I["🔒 ACCOUNT ROOT USER<br/>(Protected)"]

    I -->|Can Access| J["☁️ AWS<br/>GENERAL ACCOUNT<br/>AWS SERVICES"]

    K["🟢 MFA DEVICE/APP<br/>Generates Code<br/>Every 30s"] -->|Code changes<br/>constantly| L["🛡️ Attacker BLOCKED"]

    L -->|Cannot Access<br/>Even with Password| I

    M["🎭 ESPIONAGE or<br/>SOCIAL ENGINEERING"] -->|"FAILS with MFA ❌"| I

    style B fill:#ff6b6b,stroke:#cc0000,color:#000
    style C fill:#ff6b6b,stroke:#cc0000,color:#000
    style E fill:#ff6b6b,stroke:#cc0000,color:#000
    style F fill:#ff6b6b,stroke:#cc0000,color:#000
    style G fill:#ff6b6b,stroke:#cc0000,color:#000
    style D fill:#ff6b6b,stroke:#cc0000,color:#000

    style H fill:#51cf66,stroke:#2d8a3d,color:#000
    style I fill:#51cf66,stroke:#2d8a3d,color:#000
    style J fill:#51cf66,stroke:#2d8a3d,color:#000
    style K fill:#51cf66,stroke:#2d8a3d,color:#000
    style L fill:#51cf66,stroke:#2d8a3d,color:#000

    style M fill:#ffd700,stroke:#ff9800,color:#000
```

Now, to secure your account:

- Navigate to Console Home.
- Click your account name in the top-right corner.
- Select Security Credentials.
- You’ll be directed to the Identity and Access Management (IAM) dashboard, where all security credentials can be managed.
- If Multi-Factor Authentication (MFA) is not yet enabled, activate it under the Multi-Factor Authentication (MFA) section.
