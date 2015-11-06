# Accounts

Uphold supports accounts to allow users to deposit value into a specific card from an external source (debit/credit card or wire transfer) or withdraw to an external source (wire transfer).

Whenever a deposit is made into an Uphold card, it will be automatically converted into the value determined by the card's denomination. Likewise, when a withdrawal is made, the currency will be converted to the currency of the destination account.

## List Accounts

```bash
curl "https://api.uphold.com/v0/me/accounts" \
  -H "Authorization: Bearer <token>"
```

> The above command returns the following JSON:

```json
[
  {
    "currency": "USD",
    "id": "bfef7422-9f3c-47e0-4d4b-569d92d29a5c",
    "label": "My Chase card",
    "status": "ok",
    "type": "card"
  },
  {
    "currency": "EUR",
    "id": "07c102bb0-f129-591b-96e4-6ea26466e012",
    "label": "My checking account",
    "status": "ok",
    "type": "sepa"
  }
]
```

Retrieves a list of accounts for the current user.

### Request

`GET https://api.uphold.com/v0/me/accounts`

### Response

Returns an array of the current user's accounts.

## Get Account Details

```bash
curl "https://api.uphold.com/v0/me/accounts/07c102bb0-f129-591b-96e4-6ea26466e012" \
  -H "Authorization: Bearer <token>"
```

> The above command returns the following JSON:

```json
{
  "currency": "EUR",
  "id": "07c102bb0-f129-591b-96e4-6ea26466e012",
  "label": "My checking account",
  "status": "ok",
  "type": "sepa"
}
```

Retrieves the details about a specific account.

### Request

`GET https://api.uphold.com/v0/me/accounts/:id`

<aside class="notice">`:id` must be an account ID and it must be owned by the user performing the API call.</aside>

### Response

Returns the details associated with the account ID provided.

## Create Account

> Creating a `sepa` account:

```bash
curl https://api.uphold.com/v0/me/accounts \
  -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
        "bic": "FOOBARBI",
        "billing": {
          "country": "BE"
        },
        "currency": "EUR",
        "iban": "BE68539007547034",
        "label": "My bank account",
        "type": "sepa"
      }'
```

> Creating a `card` account:

```bash
curl https://api.uphold.com/v0/me/accounts \
  -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
        "accountType": "visa",
        "billing": {
          "address": {
            "city": "New York",
            "line1": "Foo st.",
            "line2": "3rd floor",
            "zipCode": "90210"
          },
          "country": "US",
          "state": "US-NY"
        },
        "currency": "USD",
        "expiryDate": {
          "month": "11",
          "year": "2020"
        },
        "number": "12345678909876543",
        "label": "My credit card",
        "securityCode": "123",
        "type": "sepa"
      }'
```

### Request

`POST https://api.uphold.com/v0/me/accounts`

Required fields for all account types:

Parameter | Default |  Description
--------- | ----------- | -----------
billing | | An object with the `address`, `country` and `state` fields that must match the account details.
currency | | The currency that matches the account. Uphold will automatically convert to this currency to reduce any exchange fees for the user.
label | | The display name of the account.
type | | The type of the account. Possible values: `card`, `sepa`.

Required fields when creating an account of type `card`:

Parameter | Default |  Description
--------- | ----------- | -----------
accountType | | The debit/credit card type. Possible values: `mastercard`, `visa`.
expiryDate | | An object that represents debit/credit card expiry date with fields `month` and `year`.
number | | The debit/credit card number.
securityCode | | The debit/credit card security code.

Required fields when creating an account of type `sepa`:

Parameter | Default |  Description
--------- | ----------- | -----------
bic | | The Bank Identifier Code for the account.
iban | | The International Bank Account Number for the account.

<aside class="notice">
  <strong>Important notice</strong>: in order to accept credit card information from your users you will need to be <i><a href="https://pcisecuritystandards.org">PCI compliant</a></i>.

  <p>It is paramount that you do not request credit card information from your users unless you are adequately compliant or you may <a href="https://pcicomplianceguide.org/pci-faqs-2/#11">incur legal and financial penalties.</a></p>
</aside>

### Response

Returns a fully formed [Account Object](#account-object) representing the account created.

## Update Account

```bash
curl https://api.uphold.com/v0/me/accounts/07c102bb0-f129-591b-96e4-6ea26466e012 \
  -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{ "label": "My Updated Account" }'
```

### Request

`PATCH https://api.uphold.com/v0/me/accounts/:id`

Parameter | Description
--------- | -----------
label | The display name of the account.

### Response

Returns a fully formed [Account Object](#account-object) representing the updated account.

## Delete Account

```bash
curl https://api.uphold.com/v0/me/accounts/07c102bb0-f129-591b-96e4-6ea26466e012 \
  -X DELETE \
  -H "Authorization: Bearer <token>"
```

### Request

`DELETE https://api.uphold.com/v0/me/accounts/:id`

### Response

Returns an HTTP status code of 204 to indicate that the account was deleted.
