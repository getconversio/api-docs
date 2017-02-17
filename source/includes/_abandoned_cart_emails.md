# Abandoned Cart Emails

Endpoints for viewing data of emails triggered by abandoned carts, and their customer interaction stats. For the Abandoned Carts themselves, go to [the previous section](#abandoned-carts).

## View Abandoned Cart Template

Returns an Abandoned Cart Template. This contains the basis for the content of each of its emails, as well as the trigger delay for an email to be sent.

```shell
# EXAMPLE REQUEST
$ curl "https://app.conversio.com/api/v1/abandoned-carts/templates/57b5aa3b046abfb053d80b52 \
  -H "X-ApiKey: YOUR_API_KEY"
```

> EXAMPLE RESPONSE

```json
{
  "data": {
    "id": "57b5aa3b046abfb053d80b68",
    "campaignId": "57b5aa3b046abfb053d89b69",
    "subject": "You forgot some goodies!",
    "title": "First Abandoned Cart Email",
    "rule": "delay",
    "period": 0,
    "periodHours": 1,
    "contentModules": []
  }
}
```

### Show Abandoned Cart Template [GET]

Two possible endpoints:

1. `https://app.conversio.com/api/v1/abandoned-carts/templates/{TEMPLATE_ID}`
2. `https://app.conversio.com/api/v1/abandoned-carts/campaigns/{CAMPAIGN_ID}/templates/{TEMPLATE_ID}`

_OAuth Scopes_: read_abandoned_cart_template, write_abandoned_cart_template

<aside class="success">
  Response 200
</aside>

<aside class="warning">
  Response 404 - Email not found. One of the following happened:
  <ul>
    <li>The given TEMPLATE_ID does not belong to an existing template, if provided</li>
    <li>The given CAMPAIGN_ID does not belong to an existing email, if provided</li>
    <li>The requested template belongs to another shop than authenticated</li>
    <li>The requested template belongs to another campaign than indicated</li>
  </ul>
</aside>

### Response Body

The response body includes a `data` key which contains an object representation of the requested abandoned cart template:

|Key                  |Details|
|--------------------:|-----------|
|**id:**              |**string**|
|                     |The ID for this email.|
|**campaignId:**      |**string**|
|                     |The ID for this template"s Campaign.|
|**subject:**         |**string**|
|                     |The subject used in the emails triggered by this template. May have user variables (e.g.: {firstName}).|
|**title:**           |**string**|
|                     |The template"s title, not customer-facing.|
|**rule:**            |**string**|
|                     |Rule for triggering an email from this template. Currently only "delay" is supported, which is a delay from a cart being abandoned.|
|**period:**          |**number**|
|                     |How many days must pass after the cart is abandoned to trigger an email from this template. Accumulates with `periodHours`.|
|**periodHours:**     |**string**|
|                     |How many hours must pass after the cart is abandoned to trigger an email from this template. Accumulates with `period`.|
|**contentModules:**  |**array**|
|                     |An array of the Content Modules in a template. These represent the content (copy, images, discounts, etc.) in a template. More details below.|

#### Content Modules

```json
{
  "contentModules": [
    {
      "position": 0,
      "type": "text",
      "context": {
        "body": "You left all of this stuff behind!"
      }
    },
    {
      "position": 1,
      "type": "abandonedCart",
      "context": {
        "quantity": "Quantity",
        "description": "Description",
        "unitPrice": "Unit Price",
        "total": "Total",
        "totalOrder": "Total",
        "buttonColor": "#1990FF",
        "primaryColor": "#1990FF",
        "secondaryColor": "#0076E4",
        "mainBackgroundColor": "#f8f8f8",
        "actionTitle": "Get All My Stuff!"
      }
    },
    {
      "position": 2,
      "type": "discountCoupon",
      "context": {
        "title": "You Stuff, At a Discount!",
        "description": "{{firstName}}, there"s no way you"re leaving all that stuff in the cart. Grab this discount and check-out now!",
        "actionTitle": "Start Shopping",
        "expiresOn": "Expires On",
        "primaryColor": "#1990FF",
        "amount": 10,
        "expiryPeriod": 1,
        "emailLimit": false,
        "couponType": 2,
        "backgroundColor": "#f8f8f8",
        "onePerUser": true,
        "usageLimit": 1
      }
    }
  ]
}
```

Each module has a `position` and a `type`. The 0-based position indicates where in the email the module appears (starting from top) and the type defines the kind of content.

The module"s content is in the `context` key. This object and its keys depend on the module"s type. These map directly to the content & settings in the Template editor, so we recommend referencing that in case any values are unclear.

## View Abandoned Cart Email

Returns an Abandoned Cart Email, along with its action timestamps.

```shell
# EXAMPLE REQUEST
$ curl "https://app.conversio.com/api/v1/abandoned-carts/emails/57b5aa3b046abfb053d80b52 \
  -H "X-ApiKey: YOUR_API_KEY"
```

> EXAMPLE RESPONSE

```json
{
  "data": {
    "id": "57b5aa3b046abfb053d80b68",
    "templateId": "57b5aa3b046abfb053d89b69",
    "abandonedCartId": "57b5aa3b046abf1253d89b33",
    "to": "a@customer.com",
    "status": "sent",
    "sentAt": "2016-11-20T00:00:00.000Z",
    "deliveredAt": "2016-11-20T00:00:15.000Z",
    "openedAt": "2016-11-20T01:00:00.000Z",
    "clickedAt": "2016-11-20T01:01:10.000Z"
  }
}
```

### Show Abandoned Cart Email [GET]

Three possible endpoints:

1. `https://app.conversio.com/api/v1/abandoned-carts/emails/{EMAIL_ID}`
2. `https://app.conversio.com/api/v1/abandoned-carts/templates/{TEMPLATE_ID}/emails/{EMAIL_ID}`
3. `https://app.conversio.com/api/v1/abandoned-carts/campaigns/{CAMPAIGN_ID}/templates/{TEMPLATE_ID}/emails/{EMAIL_ID}`

_OAuth Scopes_: read_abandoned_cart_email, write_abandoned_cart_email

<aside class="success">
  Response 200
</aside>

<aside class="warning">
  Response 404 - Email not found. One of the following happened:
  <ul>
    <li>The given EMAIL_ID does not belong to an existing email</li>
    <li>The given TEMPLATE_ID does not belong to an existing template, if provided</li>
    <li>The given CAMPAIGN_ID does not belong to an existing campaign, if provided</li>
    <li>The requested email belongs to another shop than authenticated</li>
    <li>The requested email belongs to another template than indicated</li>
    <li>The requested email belongs to another campaign than indicated</li>
    <li>The requested email's template belongs to another campaign than indicated</li>
  </ul>
</aside>

### Response Body

The response body includes a `data` key which contains an object representation of the requested abandoned cart email:

|Key                  |Details|
|--------------------:|-----------|
|**id:**              |**string**|
|                     |The ID for this email.|
|**templateId:**      |**string**|
|                     |The ID for this email's template.|
|**abandonedCartId:** |**string**|
|                     |The ID for the cart that triggered this email.|
|**to:**              |**string**|
|                     |The email address to which this email was sent.|
|**status:**          |**string**|
|                     |The email's status. One of "created", "rendered", "rejected" or "sent".|
|**sentAt:**          |**string**|
|                     |When this email was sent. **ISO 8601** encoded date.|
|**deliveredAt:**     |**string, optional**|
|                     |When this email was delivered. **ISO 8601** encoded date. Can be missing if not delivered.|
|**openedAt:**        |**string, optional**|
|                     |When this email was last opened. **ISO 8601** encoded date. Can be missing if never delivered.|
|**clickedAt:**       |**string, optional**|
|                     |When this email was last clicked. **ISO 8601** encoded date. Can be missing if no link was ever clicked.|
