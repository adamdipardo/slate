---
title: Contact External API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='http://contact.activeengage.ca/api/internal-docs/'>Internal API Documentation</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

This documentation is for the Contact Tool's RESTful API. All requests should be made against `http://contact.activeengage.ca/api/`.

# Campaigns

## Get All Campaigns

```shell
curl http://contact.activeengage.ca/api/campaigns/read \
  -d archived=0 \
  -d sortDir=asc \
  -d sort=name
```
> Campaigns are returned as JSON like this:

```json
{
  campaigns: [
    {
      "id": 1,
      "slug": "example-campaign",
      "lockMessage": true,
      "introduction": "Intro Message<br />With HTML<br />Goes here",
      "contactText": "Would you like to receive updates?",
      "successUrl": "http://google.com"
      "language": "en",
      "sendText": "Send!",
      "tweetVia": "twitter_handle",
      "tweetMessage": "Hey this is a tweet!",
      "customization": [
        "backgroundColor": "FFFFFF",
        "primaryColor": "000000",
        "secondaryColor": "000000"
      ],
      "fullAddress": false,
      "lists": [
        {
          "id": 1,
          "list": 1,
          "name": "Federal List",
          "isPrimary" => true
        }
      ],
      "country": "CA",
      "fields": [
        {
          "id": 1,
          "name": "Occupation",
          "slug": "occupation",
          "isRequired": true
        }
      ],
      "reply": "This is a reply message\n\nIt can be multi-line!",
      "replySubject": "Thanks for your submission!",
      "replyEmail": "no-reply@example.com",
      "name": "Example Campaign",
      "subjects": [
        {
          "id": 1,
          "subject": "Attention!"
        }
      ],
      "createdAt": 1502744934,
      "copies": [
        {
          "id": 1,
          "type": "cc",
          "email": "cc@example.com"
        }
      ],
      "testMode": false,
      "testEmail": "test@example.com",
      "paragraphs": [
        {
          "id": 1,
          "section": 1,
          "paragraph": "This is my message to my representatives."
        }
      ]
    }
  ],
  type: "active",
  sortBy: "name",
  sortDir: "asc"
}
```

Returns a sorted list of all campaigns. Filterable by whether the campaigns are active or archived.

<aside class="notice">
This endpoint requires admin authentication
</aside>

### HTTP Request

`POST http://contact.activeengage.ca/api/campaigns/read`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
archived | 0 | Set to `1` to return archived campaigns, or `0` for active campaigns
sort | name | Sorting for list of campaigns. One of `name`, `id`, `created`, or `entries`.
sortDir | asc | Sorting direction. `asc` or `desc`.

### Errors

Code | Reason
---- | ------
403 | Not authenticated as an admin
404 | No campaigns found

### Return Value

A JSON object is returned with keys for `campaigns`, `type`, `sortDir`, and `sortBy`. 

Key | Type | Description
--- | ---- | -----------
campaigns | array | All found campaigns. See [Campaign Description](#campaign-description) for details about each key
sortBy | string | Field that campaign list is sorted by
sortDir | string | Direction that campaign list is sorted by

## Get Single Campaign by ID

```shell
curl http://contact.activeengage.ca/api/campaigns/1/read
```
> Campaign is returned as a JSON object

```json
{
  "id": 1,
  "slug": "example-campaign",
  "lockMessage": true,
  "introduction": "Intro Message<br />With HTML<br />Goes here",
  "contactText": "Would you like to receive updates?",
  "successUrl": "http://google.com"
  "language": "en",
  "sendText": "Send!",
  "tweetVia": "twitter_handle",
  "tweetMessage": "Hey this is a tweet!",
  "customization": [
    "backgroundColor": "FFFFFF",
    "primaryColor": "000000",
    "secondaryColor": "000000"
  ],
  "fullAddress": false,
  "lists": [
    {
      "id": 1,
      "list": 1,
      "name": "Federal List",
      "isPrimary" => true
    }
  ],
  "country": "CA",
  "fields": [
    {
      "id": 1,
      "name": "Occupation",
      "slug": "occupation",
      "isRequired": true
    }
  ],
  "reply": "This is a reply message\n\nIt can be multi-line!",
  "replySubject": "Thanks for your submission!",
  "replyEmail": "no-reply@example.com",
  "name": "Example Campaign",
  "subjects": [
    {
      "id": 1,
      "subject": "Attention!"
    }
  ],
  "createdAt": 1502744934,
  "copies": [
    {
      "id": 1,
      "type": "cc",
      "email": "cc@example.com"
    }
  ],
  "testMode": false,
  "testEmail": "test@example.com",
  "paragraphs": [
    {
      "id": 1,
      "section": 1,
      "paragraph": "This is my message to my representatives."
    }
  ]
}
```

Gets the details of a single campaign, based on the campaign ID.

<aside class="notice">
This endpoint requires admin authentication
</aside>

### HTTP Request

`GET http://contact.activeengage.ca/api/campaigns/{id}/read`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
id | int | ID of campaign to read

### Errors

Code | Reason
---- | ------
403 | Not authenticated as an admin
404 | Campaign not found

### Return Value

The campaign JSON object is returned. See [Campaign Description](#campaign-description) for details about each key.

## Get Single Campaign by Slug

```shell
curl http://contact.activeengage.ca/api/campaigns/example-campaign/read
```

> Campaign is returned as a JSON object

```json
{
  "id": 1,
  "slug": "example-campaign",
  "lockMessage": true,
  "introduction": "Intro Message<br />With HTML<br />Goes here",
  "contactText": "Would you like to receive updates?",
  "successUrl": "http://google.com"
  "language": "en",
  "sendText": "Send!",
  "tweetVia": "twitter_handle",
  "tweetMessage": "Hey this is a tweet!",
  "customization": [
    "backgroundColor": "FFFFFF",
    "primaryColor": "000000",
    "secondaryColor": "000000"
  ],
  "fullAddress": false,
  "lists": [
    {
      "districtName": "Riding",
      "repName": "Member of Parliament",
      "isPrimary" => true
    }
  ],
  "country": "CA",
  "fields": [
    {
      "id": 1,
      "name": "Occupation",
      "slug": "occupation",
      "isRequired": true
    }
  ],
  "message": "This is the email body that will be pre-filled in the form."
}
```

Gets the details of a single campaign based on it's slug. The response is suitable for a public view.

<aside class="notice">
This endpoint will return archived campaigns
</aside>

### HTTP Request

`GET http://contact.activeengage.ca/api/campaigns/{slug}/read`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
slug | string | Slug of campaign to read

### Errors

Code | Reason
---- | ------
404 | Campaign not found

### Return Value

The Campaign JSON object is returned (public view). See [Campaign Description](#campaign-description) for details about each key.

## Create a Campaign

```shell
curl http://contact.activeengage.ca/api/campaigns/create \
  -d \
```

> Campaign is returned as a JSON object

```json
{
  "id": 1,
  "slug": "example-campaign",
  "lockMessage": true,
  "introduction": "Intro Message<br />With HTML<br />Goes here",
  "contactText": "Would you like to receive updates?",
  "successUrl": "http://google.com"
  "language": "en",
  "sendText": "Send!",
  "tweetVia": "twitter_handle",
  "tweetMessage": "Hey this is a tweet!",
  "customization": [
    "backgroundColor": "FFFFFF",
    "primaryColor": "000000",
    "secondaryColor": "000000"
  ],
  "fullAddress": false,
  "lists": [
    {
      "id": 1,
      "list": 1,
      "name": "Federal List",
      "isPrimary" => true
    }
  ],
  "country": "CA",
  "fields": [
    {
      "id": 1,
      "name": "Occupation",
      "slug": "occupation",
      "isRequired": true
    }
  ],
  "reply": "This is a reply message\n\nIt can be multi-line!",
  "replySubject": "Thanks for your submission!",
  "replyEmail": "no-reply@example.com",
  "name": "Example Campaign",
  "subjects": [
    {
      "id": 1,
      "subject": "Attention!"
    }
  ],
  "createdAt": 1502744934,
  "copies": [
    {
      "id": 1,
      "type": "cc",
      "email": "cc@example.com"
    }
  ],
  "testMode": false,
  "testEmail": "test@example.com",
  "paragraphs": [
    {
      "id": 1,
      "section": 1,
      "paragraph": "This is my message to my representatives."
    }
  ]
}
```

Creates a new campaign.

<aside class="notice">This endpoint requires admin authentication</aside>

### HTTP Request

`POST http://contact.activeengage.ca/api/campaigns/create`

### Query Parameters

Parameter | Description | Rules
--------- | ----------- | -----
name | Name of campaign (internal use only) | required
backgroundColor | Background color of the form (6-char hex code) | required, size:6
primaryColor | Primary color of the form (6-char hex code) | required, size:6
secondaryColor | Secondary color of the form (6-char hex code) | required, size:6
reply | Body of reply (receipt) email | required with replySubject and replyEmail
replySubject | Subject of reply (receipt) email | required with reply and replyEmail, max:255
replyEmail | From address of reply (receipt) email | required with reply and replySubject, max:255
contactText | Message to prompt users for contact opt-in | max:255
successUrl | URL for redirection after form submission | max:255
language | 2-digit language code | required, either `en` or `fr`
sendText | Action-text for submit button | required, max:255
lockMessage | `true` to lock message so it cannot be altered, `false` to not | Either `true` or `false`
testMode | `true` to enable test-mode, false for live | Either `true` or `false`
testEmail | Email address to redirect all messages to when in test mode | email


# Campaign Description

> Sample campaign response JSON (admin view)

```json
{
  "id": 1,
  "slug": "example-campaign",
  "lockMessage": true,
  "introduction": "Intro Message<br />With HTML<br />Goes here",
  "contactText": "Would you like to receive updates?",
  "successUrl": "http://google.com"
  "language": "en",
  "sendText": "Send!",
  "tweetVia": "twitter_handle",
  "tweetMessage": "Hey this is a tweet!",
  "customization": [
    "backgroundColor": "FFFFFF",
    "primaryColor": "000000",
    "secondaryColor": "000000"
  ],
  "fullAddress": false,
  "lists": [
    {
      "id": 1,
      "list": 1,
      "name": "Federal List",
      "isPrimary" => true
    }
  ],
  "country": "CA",
  "fields": [
    {
      "id": 1,
      "name": "Occupation",
      "slug": "occupation",
      "isRequired": true
    }
  ],
  "reply": "This is a reply message\n\nIt can be multi-line!",
  "replySubject": "Thanks for your submission!",
  "replyEmail": "no-reply@example.com",
  "name": "Example Campaign",
  "subjects": [
    {
      "id": 1,
      "subject": "Attention!"
    }
  ],
  "createdAt": 1502744934,
  "copies": [
    {
      "id": 1,
      "type": "cc",
      "email": "cc@example.com"
    }
  ],
  "testMode": false,
  "testEmail": "test@example.com",
  "paragraphs": [
    {
      "id": 1,
      "section": 1,
      "paragraph": "This is my message to my representatives."
    }
  ]
}
```

> Sample campaign response JSON (public view)

```json
{
  "id": 1,
  "slug": "example-campaign",
  "lockMessage": true,
  "introduction": "Intro Message<br />With HTML<br />Goes here",
  "contactText": "Would you like to receive updates?",
  "successUrl": "http://google.com"
  "language": "en",
  "sendText": "Send!",
  "tweetVia": "twitter_handle",
  "tweetMessage": "Hey this is a tweet!",
  "customization": [
    "backgroundColor": "FFFFFF",
    "primaryColor": "000000",
    "secondaryColor": "000000"
  ],
  "fullAddress": false,
  "lists": [
    {
      "districtName": "Riding",
      "repName": "Member of Parliament",
      "isPrimary" => true
    }
  ],
  "country": "CA",
  "fields": [
    {
      "id": 1,
      "name": "Occupation",
      "slug": "occupation",
      "isRequired": true
    }
  ],
  "message": "This is the email body that will be pre-filled in the form."
}
```

This explains the keys that are returned for operations involving campaigns.

Key | Type | Description
--- | ---- | -----------
id | int | Campaign ID
slug | string | Unique slug representing the campaign
lockMessage | bool | True if the message cannot be modified by the user
introduction | string | Optional text/html that can be displayed above the form
contactText | string | Optional text to prompt the user with for followup opt-in
successUrl | string | Optional URL to redirect the user to after submission
language | string | `en` for English or `fr` for French
sendText | string | Action text for the submit button
tweetVia | string | Optional username that tweet submissions will be sent via
tweetMessage | string | Option text that will be the tweet body
customization | object | Object with keys for form colors
fullAddress | bool | `true` if form should ask for a full address, `false` if only postal code
lists | array | Lists the form sends to (see [Lists](#campaign-lists))
country | string | Campaign country either `CA` or `US`
fields | array | Custom fields the form will ask for (see [Fields](#campaign-fields))

**Admin Only**

Key | Type | Description
--- | ---- | -----------
reply | string | Optional reply (receipt) email body
replySubject | string | Optional reply (receipt) email subject
replyEmail | string | Optional reply (receipt) email send address
name | string | Campaign name, internal-use only
subjects | array | Email submission subjects (see [Subjects](#campaign-subjects))
createdAt | int | Creation date of campaign in unix-timestamp
copies | array | CC/BCCs for all submissions (see [Copies](#campaign-copies))
testMode | bool | True if campaign is in test mode (no real emails sent)
testEmail | string | Optional email to send all submissions to when in test-mode
paragraphs | array | Paragraphs for email body (see [Paragraphs](#campaign-paragraphs))

**Public Only**

Key | Type | Description
--- | ---- | -----------
message | string | Email body that is pre-filled in the form

## Campaign Lists

> Sample of each list JSON object (admin view)

```json
{
  "id": 1,
  "list": 1,
  "name": "Federal List",
  "isPrimary" => true
}
```

> Sample of each JSON object (public view)

```json
{
  "districtName": "Riding", 
  "repName": "Member of Parliament", 
  "isPrimary": true
}
```

A campaign can have one or more lists associated with it. This allows a campaign to target multiple levels, such as a provincial legislature and a city council.
  
There will always be one, and only one, primary list, but there can be zero or more secondary lists. The primary list what should be displayed in the widget for matched riding/representative.

**Admin View**

These keys are returned with admin-related endpoints.

Key | Type | Description
--- | ---- | -----------
id | int | ID of the campaign list
list | int | ID of the list
name | string | Name of the list
isPrimary | bool | `true` if the list is the primary list, `false` if its a secondary list

**Public View**

These keys are returned with public-related endpoints.

Key | Type | Description
--- | ---- | -----------
districtName | string | What districts in this list are called
repName | string | What representatives in this list are called
isPrimary | bool | `true` if the list is the primary list, `false` if its a secondary list

## Campaign Fields

> Sample of each JSON object

```json
{
  "id": 1,
  "name": "Occupation",
  "slug": "occupation",
  "isRequired": true
}
```

A campaign can have zero or more fields. Each field is a custom form field that will be included in the widget when shown to the user. Each field can be optional or required.

Key | Type | Description
--- | ---- | -----------
id | int | Field ID
name | string | Pretty-name of the field
slug | string | Field slug
isRequired | bool | `true` if the field is required, `false` if not

## Campaign Subjects

> Sample of each JSON object

```json
{
  "id": 1,
  "subject": "Attention!"
}
```
A campaign can have one or more subjects. Each subject is a possible subject line for the email. A random one will be chosen for each form submission.

Key | Type | Description
--- | ---- | -----------
id | int | Subject ID
subject | string | The subject

## Campaign Copies

> Sample of each JSON object

```json
{
  "id": 1,
  "type": "cc",
  "email": "cc@example.com"
}
```
A campaign can have zero or more copies. Each copy will be CC'd or BCC'd on each email, depending on the type.

Key | Type | Description
--- | ---- | -----------
id | int | Copy ID
type | string | Either `cc` or `bcc`
email | string | The email address

## Campaign Paragraphs

> Sample of each JSON object

```json
{
  "id": 1,
  "section": 1,
  "paragraph": "This is my message to my representatives."
}
```
A campaign can have zero or more paragraphs. Paragraphs make up the body of the email and can be used to create a pre-filled message. Each paragraph can have a section number, and when the widget is presented a random paragraph from each section will be put together and pre-filled in the form.

Key | Type | Description
--- | ---- | -----------
id | int | Paragraph ID
section | int | Number corresponding to which section paragraph represents
paragraph | string | The paragraph body