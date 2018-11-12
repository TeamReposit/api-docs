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

Welcome to the Reposit Partner API! You can use our API to authenticate with your supplier token and create new Reposits.

<aside class="notice">
For testing please use the <code>https://demo.reposit.co.uk/deposits/</code> endpoint in the examples below.
</aside>

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer reallySecureApiKey"
```

> Make sure to replace `reallySecureApiKey` with your API key.

Reposit requires you to have an API key to allow access to the API. Getting an API key is simple:

1. [Register](https://reposit.co.uk/register/) an agency or landlord account ([Click here](https://demo.reposit.co.uk/register/) if you'd like to make a demo account for testing).
2. Navigate to your 'Account' section. Your API key will be displayed on this page.

Reposit expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer reallySecureApiKey`

<aside class="notice">
You must replace <code>reallySecureApiKey</code> with your personal API key.
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
  "integrationToken": "reallySecureApiKey",
  "created_at": "2018-11-10 11:12:11.999"
}
```

This endpoint returns information about the logged in supplier account.

<aside class="notice">
Supplier data is useful if you need the supplier id for calling other endpoints. Such as <a href="#get-supplier-agents">Getting supplier agents</a>.
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

### HTTP Request

`GET https://reposit.co.uk/deposits/v1/suppliers/:id/agents`

### Request params

| Parameter | Description                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------- |
| id        | Supplier id - You can obtain this from calling the [Get Supplier account](#get-supplier-account) endpoint |

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
