---
title: Reposit API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Reposit Partner API! You can use our API to create new Reposits from your system.

<aside class="notice">
For testing please use <code>https://demo.reposit.co.uk/deposits/</code> as the base url in the examples below.
</aside>

# Authentication

> To authorize, use this code:

```shell
curl "api_endpoint_here"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Make sure to replace `reallySecureApiKey` with your API key.

<aside class="warning">API keys are designed to map to a single lettings agency. If you are integrating a system that has a concept of organisations/agencies then each one would need to use it's own api key; they would need to create an account with us first and enter their api key into your system.</aside>
Reposit requires you to have an API key to allow access to the API. Getting an API key is simple:

1. [Register](https://reposit.co.uk/register/) an agency or landlord account ([Click here](https://demo.reposit.co.uk/register/) if you'd like to make a demo account for testing).
2. Navigate to your 'Account' section. Your API key will be displayed on this page.

Reposit expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer reallySecureApiKey`

<aside class="notice">
You must replace <code>reallySecureApiKey</code> with a supplier API key.
</aside>

# CRM Integrations

> To authorize as a referrer, use this code:

```shell
curl "api_endpoint_here"
  -H "Authorization: Bearer reallySecureApiKey"
  -H "Reposit-Referrer-Token: GreatCRM-bg3khR9oNz"
```

If you are a third party system that is acting as referrer between a landlord / letting agent and Reposit (e.g. CRM systems) then you should include an additional header in `ALL requests`.

`Reposit-Referrer-Token: GreatCRM-bg3khR9oNz`

<aside class="warning">
This code will be used to track activity between your system and ours; so please remember to do this. If you are a referrer system that would like to request a token, <a href="mailto:brendan@reposit.co.uk">send us an email</a>.
</aside>
<aside class="notice">
 You will need to provide both an integration token and a referrer token. The integration token identifies the letting agency that has an account with us and the referrer token identifies your system.
</aside>

# Routes

## Get Supplier account

> HTTP Request

```shell
curl "https://reposit.co.uk/deposits/v1/suppliers/me"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Example response:

```json
{
  "id": "sup_DFed239dl8974Dccx",
  "name": "Great Lets",
  "type": "AGENCY",
  "created_at": "2018-11-10 11:12:11.999"
}
```

This endpoint returns information about the supplier account associated with the integration token you provided.

<aside class="notice">
This endpoint is useful if you need to obtain a supplier id for calling other endpoints. Such as <a href="#get-supplier-agents">Getting supplier agents</a>.
</aside>

### HTTP Request

`GET https://reposit.co.uk/deposits/v1/suppliers/me`

## Get Supplier agents

> HTTP Request

```shell
curl "https://reposit.co.uk/deposits/v1/suppliers/:id/agents"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Example response:

```json
[
  { "id": "usr_dGdls1203xl00p", "firstName": "Janet", "lastName": "Savoy" },
  { "id": "usr_bG60oPld44Hxa1", "firstName": "Frank", "lastName": "Galloway" }
]
```

This endpoint returns all agent user accounts associated with this supplier. This is required when [creating a Reposit](#create-a-reposit) as you'll need to pass in a valid agent id.

<aside class="notice">
 If you are integrating create a Reposit, you should allow the user to select which agent to associate with the Reposit.
</aside>

### HTTP Request

`GET https://reposit.co.uk/deposits/v1/suppliers/:id/agents`

### Request params

| Parameter | Description                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------- |
| id        | Supplier id - You can obtain this from calling the [Get Supplier account](#get-supplier-account) endpoint |

## Get a Reposit

> HTTP Request

```shell
curl "https://reposit.co.uk/deposits/v1/reposits/:id"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Example response payload:

```json
{
  "id": "rep_xdFEdfgeDxoC",
  "ppm": 5000,
  "headcount": 1,
  "startDate": "2017-01-18",
  "endDate": "2018-01-18",
  "letOnly": null,
  "createdAt": "2017-01-12T00:00:00.000Z",
  "closedAt": null,
  "deactivatedAt": null,
  "address": {
    "line1": "Techhub London",
    "line2": "20 Ropemaker Street",
    "town": "London",
    "country": "GBR",
    "postcode": "EC2Y 9AR"
  },
  "agent": {
    "id": "usr_XOdlpqwSvvm",
    "firstName": "Bob",
    "lastName": "Jenkins"
  },
  "supplier": {
    "id": "sup_FlaDTencPL",
    "name": "Hans International",
    "type": "AGENCY"
  },
  "tenants": [
    {
      "email": "tenant1@reposit.co.uk",
      "createdAt": "2017-01-12T00:00:00.000Z",
      "status": "REGISTERED"
    }
  ],
  "landlordUrl": "http://reposit.co.uk/deposits/v1/reposits/rep_xdFEdfgeDxoC/landlord",
  "policiesUrl": "http://reposit.co.uk/deposits/v1/reposits/rep_xdFEdfgeDxoC/policies"
}
```

This endpoint returns a single Reposit by id.

### HTTP Request

`GET https://reposit.co.uk/deposits/v1/reposits/:id`

### Response attributes

| Parameter     | Type                | Description                                                                                        |
| ------------- | ------------------- | -------------------------------------------------------------------------------------------------- |
| id            | string              | Identifier, always starts with 'rep\_'                                                             |
| ppm           | number              | Monthly rent in pounds                                                                             |
| headcount     | number              | Total number of tenants                                                                            |
| ppm           | number              | Monthly rent in pounds                                                                             |
| startDate     | Date                | Start date of the tenancy (YYYY-MM-DD)                                                             |
| endDate       | Date                | End date of the tenancy (YYYY-MM-DD)                                                               |
| letOnly       | boolean             | Set to true if the Landlord will manage the claims process instead of the agent. Defaults to false |
| createdAt     | timestamp           | Date and time of creation                                                                          |
| closedAt      | timestamp           | Date Reposit was closed due to expiry (null if not closed)                                         |
| deactivatedAt | timestamp           | Date Reposit was manually deactivated (null if not deactiated)                                     |
| address       | [Address](#address) | Address of the property on the tenancy agreement                                                   |
| agent         | Agent               | User that 'owns' the Reposit                                                                       |
| supplier      | Object              | Letting Agency that manages the property                                                           |
| tenants       | Tenant[]            | List of tenants and their status (INVITED, REGISTERED, CONFIRMED, SIGNED or PAID)                  |
| landlordUrl   | string              | Url to get more information about the landlord                                                     |
| policiesUrl   | string              | Url to get more information about insurance policies issued for this Reposit                       |

## Get Reposit policies

> HTTP Request

```shell
curl "https://reposit.co.uk/deposits/v1/reposits/:id/policies"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Example response payload:

```json
[
  {
    "id": "pol_w41bETkXo33P",
    "type": {
      "id": "CAN_V2_3000_24",
      "category": "DEPOSIT_WAIVER"
    },
    "startDate": "2018-12-31T00:00:00.000Z",
    "endDate": "2020-12-31T00:00:00.000Z",
    "documents": [
      {
        "id": "doc_GVRxNIylVf36",
        "type": "Policy Summary",
        "name": "pol_w41bETkXo33P",
        "href": "https://docs.cloudfront.net/1234/policy-certificate.pdf",
        "createdAt": "2018-12-10T11:46:11.326Z"
      }
    ]
  }
]
```

Returns all insurance policies associated with a Reposit

### HTTP Request

`GET https://reposit.co.uk/deposits/v1/reposits/:id/policies`

### Response attributes

| Parameter | Type                                | Description                                                                                         |
| --------- | ----------------------------------- | --------------------------------------------------------------------------------------------------- |
| id        | string                              | Identifier, always starts with 'pol\_'                                                              |
| type      | [PolicyType](#policytype)           | Type of insurance policy                                                                            |
| startDate | timestamp                           | Policy cover start date                                                                             |
| endDate   | timestamp                           | Policy cover end date                                                                               |
| documents | [PolicyDocument[]](#policydocument) | List of files or documents associated with the policy (Normally includes a certificate of issuance) |

## Create a Reposit

> HTTP Request

```shell
curl -X POST "https://reposit.co.uk/deposits/v1/reposits"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Example request payload:

```json
{
  "address": {
    "line1": "229 Shoreditch High Street",
    "line2": "Shoreditch",
    "town": "London",
    "country": "GBR",
    "postcode": "E1 6PJ"
  },
  "landlord": {
    "firstName": "Bobby",
    "lastName": "Moore",
    "email": "bobtheman@gmail.com",
    "address": {
      "line1": "229 Shoreditch High Street",
      "line2": "Shoreditch",
      "town": "London",
      "country": "GBR",
      "postcode": "E1 6PJ"
    }
  },
  "agentId": "usr_bG60oPld44Hxa1",
  "ppm": 2000,
  "headcount": 2,
  "startDate": "2018-10-21",
  "endDate": "2019-10-21",
  "tenantEmails": ["tenant1@hotmail.co.uk", "tenant2@sky.com"],
  "letOnly": false
}
```

This endpoint creates a Reposit with all required information.

<aside class="notice">
Once a Reposit is created, tenants will be emailed and need to complete the process through the Reposit dashboard before the Reposit becomes 'Active' and the landlord is covered.
</aside>

### HTTP Request

`POST https://reposit.co.uk/deposits/v1/reposits`

### Payload

| Parameter    | Type                  | Description                                                                                                                                                        |
| ------------ | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| address      | [Address](#address)   | Address of the property on the tenancy agreement.                                                                                                                  |
| landlord     | [Landlord](#landlord) | Landlord of the property.                                                                                                                                          |
| agentId      | string                | ID of the agent who will receive all notifications related to this Reposit. You can lookup agent ids via the [Get supplier agents](#get-supplier-agents) endpoint. |
| ppm          | number                | Monthly rent in pounds.                                                                                                                                            |
| headcount    | number                | Total number of tenants on the tenancy agreement.                                                                                                                  |
| startDate    | Date                  | Start date of the tenancy (YYYY-MM-DD)                                                                                                                             |
| endDate      | Date                  | End date of the tenancy. (YYYY-MM-DD)                                                                                                                              |
| tenantEmails | string[]              | Array of email addresses for all tenants on the tenancy agreement.                                                                                                 |
| letOnly      | boolean               | Set to true if the Landlord will manage the claims process instead of the agent. Defaults to false.                                                                |

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

# Webhooks

Reposit uses webhooks to alert you of events that happen within our system. These are POST requests to your server that are sent as soon as an event occurs. The body of the request contains details about the event.

All webhooks will be sent to a single endpoint on your system. If your system returns an error when we try to send an event - we will retry up to a maximum of 5 times per event.

<aside class="notice">
 Webhooks need to be activated upon request, if you'd like to activate webhooks for your account please <a href="mailto:brendan@reposit.co.uk">contact us</a>.
</aside>

## Overview

| Resource | Event                    | Description                                                                                                                                                                                                       |
| -------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| reposit  | reposit.tenant.confirmed | When any tenant on the Reposit confirms                                                                                                                                                                           |
| reposit  | reposit.tenant.signed    | When any tenant on the Reposit signs legal documents for the Reposit                                                                                                                                              |
| reposit  | reposit.tenant.paid      | When any tenant completes payment for the Reposit                                                                                                                                                                 |
| reposit  | reposit.completed        | When all tenants on a Reposit complete payment and the Reposit becomes 'Active'. When you receive this event, you can follow the href property to get more information, such as the insurance policy certificate. |

## reposit.tenant.confirmed

> Tenant confirmed example webhook payload

```json
{
  "resource_type": "reposit",
  "action": "reposit.tenant.confirmed",
  "timestamp": "2018-12-20T10:44:23.131Z",
  "resource": {
    "id": "ep_F32iOv3xE3Nmrp",
    "href": "https://reposit.co.uk/deposits/v1/reposits/ep_F32iOv3xE3Nmrp"
  },
  "data": {
    "tenant": {
      "email": "tenant@reposit.co.uk",
      "createdAt": "2018-12-20T10:36:05.679Z",
      "status": "CONFIRMED"
    }
  }
}
```

This event is triggered when a tenant confirms the details of the Reposit.

## reposit.tenant.signed

> Tenant signed example webhook payload

```json
{
  "resource_type": "reposit",
  "action": "reposit.tenant.signed",
  "timestamp": "2018-12-20T10:44:23.131Z",
  "resource": {
    "id": "ep_F32iOv3xE3Nmrp",
    "href": "https://reposit.co.uk/deposits/v1/reposits/ep_F32iOv3xE3Nmrp"
  },
  "data": {
    "tenant": {
      "email": "tenant@reposit.co.uk",
      "createdAt": "2018-12-20T10:36:05.679Z",
      "status": "SIGNED"
    }
  }
}
```

This event is triggered when a tenant signs the legal documents for the Reposit.

## reposit.tenant.paid

> Tenant paid example webhook payload

```json
{
  "resource_type": "reposit",
  "action": "reposit.tenant.paid",
  "timestamp": "2018-12-20T10:44:23.131Z",
  "resource": {
    "id": "ep_F32iOv3xE3Nmrp",
    "href": "https://reposit.co.uk/deposits/v1/reposits/ep_F32iOv3xE3Nmrp"
  },
  "data": {
    "tenant": {
      "email": "tenant@reposit.co.uk",
      "createdAt": "2018-12-20T10:36:05.679Z",
      "status": "PAID"
    }
  }
}
```

This event is triggered when a tenant completes payment for the Reposit.

<aside class="warning">
This does not mean the Reposit is active, all tenants must complete payment first.
</aside>

## reposit.completed

> Reposit completed example webhook payload

```json
{
  "resource_type": "reposit",
  "action": "reposit.completed",
  "timestamp": "2018-12-20T10:48:03.169Z",
  "resource": {
    "id": "rep_F32iOv3xE3Nmrp",
    "href": "https://reposit.co.uk/deposits/v1/reposits/rep_F32iOv3xE3Nmrp"
  }
}
```

This event is triggered when all tenants have paid and the cover to the Landlord is in place.

When you receive this event, your client can follow the 'href' property to get the full status of the Reposit and any policy documents if you wish to display those to your users. (See [Get Reposit policies](#get-reposit-policies))

# Objects

## Address

> Full JSON structure for an address looks like this:

```json
{
  "line1": "229 Shoreditch High Street",
  "line2": "Shoreditch",
  "town": "London",
  "country": "GBR",
  "postcode": "E1 6PJ"
}
```

| Parameter | Required? | Description                                                                                    |
| --------- | --------- | ---------------------------------------------------------------------------------------------- |
| line1     | Y         | First line of the address                                                                      |
| line2     | N         | Second line of the address                                                                     |
| town      | Y         | Town or City                                                                                   |
| country   | Y         | [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) country code e.g. 'GBR' |
| postcode  | Y         | 6 digit postcode e.g. 'EC2M 6YP'                                                               |

## Landlord

> Full JSON structure for a landlord looks like this:

```json
{
  "firstName": "Bobby",
  "lastName": "Moore",
  "email": "bobtheman@gmail.com",
  "address": {
    "line1": "229 Shoreditch High Street",
    "line2": "Shoreditch",
    "town": "London",
    "country": "GBR",
    "postcode": "E1 6PJ"
  }
}
```

We use the Landlord's information to name them on the insurace policies which provide them with cover when a Reposit is completed.
A landlord can be either a company or a person.

| Parameter | Required? | Description                                               |
| --------- | --------- | --------------------------------------------------------- |
| firstName | Y         | First name                                                |
| lastName  | Y         | Last name                                                 |
| email     | Y         | Email address (to receive policy documents)               |
| address   | Y         | Home or business address ([See address object](#address)) |

## PolicyType

> Full JSON structure for a policy type looks like this:

```json
{
  "id": "CAN_V2_3000_24",
  "category": "DEPOSIT_WAIVER"
}
```

A policy type represents an particular type of insurance policy. Reposits may have different policy types depending on the duration and amount of cover required.

For most cases the category be 'DEPOSIT_WAIVER' which is our default deposit replacement product.

| Parameter | Description                                                 |
| --------- | ----------------------------------------------------------- |
| id        | Identifier of the policy type                               |
| category  | Category of policy (Will be 'DEPOSIT_WAIVER' in most cases) |

## PolicyDocument

> Full JSON structure for a policy document looks like this:

```json
{
  "id": "doc_GVRxNIylVf36",
  "type": "Policy Summary",
  "name": "pol_w41bETkXo33P",
  "href": "https://docs.cloudfront.net/1234/policy-certificate.pdf",
  "createdAt": "2018-12-10T11:46:11.326Z"
}
```

A policy document is typically a certificate of issuance outlining the policy cover in a PDF format.

| Parameter | Description                                                            |
| --------- | ---------------------------------------------------------------------- |
| id        | Identifier of the policy type                                          |
| type      | Type of document. Generally 'Policy Summary' for issuance certificates |
| name      | Name of the document                                                   |
| href      | Link to access the document                                            |
| createdAt | Date and time of document creation                                     |
