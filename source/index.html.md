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
For testing please use the <code>https://demo.reposit.co.uk/api/</code> endpoint in the examples below.
</aside>

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: reallySecureApiKey"
```

> Make sure to replace `reallySecureApiKey` with your API key.

Reposit requires you to have an API key to allow access to the API. Getting an API key is simple:

1. [Register](https://reposit.co.uk/register/) an agency or landlord account ([Click here](https://demo.reposit.co.uk/register/) if you'd like to make a demo account for testing).
2. Naviagte to your 'Account' section. Your API key will be displayed on this page.

Reposit expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: reallySecureApiKey`

<aside class="notice">
You must replace <code>reallySecureApiKey</code> with your personal API key.
</aside>

# Routes

## Create a Reposit

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`POST https://reposit.co.uk/api/deposits/v1/reposits`

### Payload

| Parameter    | Type     | Description                                                                                            |
| ------------ | -------- | ------------------------------------------------------------------------------------------------------ |
| address      | Address  | Address of the property on the tenancy agreement.                                                      |
| landlord     | Landlord | Landlord of the property.                                                                              |
| ppm          | number   | Monthly rent in pounds.                                                                                |
| headcount    | number   | Total number of tenants on the tenancy agreement.                                                      |
| startDate    | Date     | Start date of the tenancy.                                                                             |
| endDate      | Date     | End date of the tenancy.                                                                               |
| tenantEmails | string[] | Array of email addresses for all tenants on the tenancy agreement.                                     |
| letOnly      | boolean  | Set to true if the Landlord will manage the claims process instead of the account creating the Reposit |

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| ID        | The ID of the kitten to retrieve |

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

| Parameter | Required? | Description                                                |
| --------- | --------- | ---------------------------------------------------------- |
| firstName | Y         | First name                                                 |
| lastName  | Y         | Last name                                                  |
| email     | Y         | Email address (to receive policy documents)                |
| address   | Y         | Home or business address ([See address object](/#address)) |
