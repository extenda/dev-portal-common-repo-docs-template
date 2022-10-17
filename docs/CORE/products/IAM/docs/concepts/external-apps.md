# External Apps

## Overview

External Apps - are specific named entity that are developed to create tenant apps.
It provides a unique API KEYs functionality to exchange id tokens from tenant federated system for IAM tokens.

[For more information about federated system](./identity-providers.md)

## User stories:
- I am as a tenant want to talk to Hiiretail APIs from front-end/mobile app.
- I am as a developer want to get IAM token for standalone/federated user for testing purposes.

## Usage
To access Hiiretail APIs from frontend app, you usually have your own
federated identity system. External Apps provide an integration from your app to Hiiretail.

- Create external apps in IAM UI.
Example link: https://[tenant-alias].hiiretail.com/iam/external-app

- Exchange your federated token for [IAM token](./tokens.md) using external app API_KEY.
[Example](https://developer.hiiretail.com/api/iam-api#tag/IamToken)