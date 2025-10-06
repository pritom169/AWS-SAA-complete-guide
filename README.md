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
