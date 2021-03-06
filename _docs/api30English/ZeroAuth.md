---
title: Zero Auth
category: API CIELO ECOMMERCE (English)
order: 2
---
### WHAT IS ZERO AUTH?

**Zero Auth** is the Cielo  Card Validation tool. It allows the merchant to know whether or not the card is valid before any bank authorization. It anticipates the reason for a possible non-authorization.

The **Zero Auth** can be used in 3 ways:

1. **Standard** - Sending a standard card without tokenization or additional analysis
2. **With Tokenized Card** - Sending a TOKEN 3.0 for analysis
3. **With AVS** - Using Cielos AVS to validate the card.


It is important to note that Zero Auth **does not return or analyzes** the following items:

1. The credit card limits
2. Information about the costumer
3. Does not trigger any kind of bank response (like warnings SMS's to the costumer)


Zero Auth supports the following creditcard brands:

* Visa
* MasterCard


### Use case


This is an example of how to use zero auth to improve your sales conversion

Zero Auth is a tool from Cielo that allows you to check if a card is valid to  purchase before the order is finalized. 
It does this by simulating an authorization, but without affecting the credit limit or alerting the card holders about the test.

It does not inform the limit or characteristics of the card, but simulates a Cielo authorization, validating data such as:

1. If the card is valid with the issuing bank
2. If the card has an available limit
3. If the card works in Brazil
4. If the card number is correct
5. If the CVV is valid

Zero Auth also works with Card tokens created by the Cielo Ecommerce Api

Here's an example of usage:

**Zero auth as card validator**

A Streaming company called FlixNet has a subscription service, where in addition to making a recurrence, it has saved cards and receives new subscriptions daily.

All of these steps require that a transaction to be performed so new users can get access to the company library of movies. This flow raises the cost of FlixNet if transactions are not authorized.

How could FlixNet reduce this cost? Validating the card before sending it to authorized.

FlixNet uses Zero Auth at 2 different times:

* **Registration**: "You must include a card to earn 30 free days in the first month".

The problem is that at the end of this period, if the card is invalid, the new registered card exists, but it does not work, because the saved card is invalid. FlixNet solved this problem by testing the card with Zero Auth at registration, so it already knows if the card is valid and releases the account creation. If the card is not accepted, FlixNet may suggest the use of another card.

* **Recurrence**: every month, before it charges their clients Subscription, Flixnet tests the card with zero auth, so knowing if it will be authorized or not. This helps FlixNet to predict which cards will be denied, already alerting the client to update the payment method to be used.


### Integration

In order to use Zero Auth, the merchant must send a `POST` request to the Cielo Ecommerce API, simulating a transaction. 
The `POST` must be done at the following URLs:

> Sandbox: https: // `apisandbox`.cieloecommerce.cielo.com.br / 1 /` zeroauth`

> Production: https: // `api`.cieloecommerce.cielo.com.br / 1 /` zeroauth`

Each type of validation requires a different technical contract. They will result in differentiated responses.

Below is the list of Requisition fields:


| Field              | Description                                                                                                         | Type    | Size |   Required  |
|--------------------|---------------------------------------------------------------------------------------------------------------------|---------|------|:-----------:|
| **CardType**       | Defines the type of card used: <br> <br> *CreditCard* <br> *DebitCard* <br> <br> If not sent, CreditCard as default | Text    | 255  |      No     |
| **CardNumber**     | Buyer Card Number                                                                                                   | Text    | 16   |     sim     |
| **Holder**         | Buyer's name printed on the card.                                                                                   | Text    | 25   |     not     |
| **ExpirationDate** | Expiration Date printed on the card.                                                                                | Text    | 7    |     sim     |
| **SecurityCode**   | Security code printed on the card.                                                                                  | Text    | 4    |     not     |
| **SaveCard**       | Boolean that identifies whether the card will be saved to generate the CardToken.                                   | Boolean | ---  |      No     |
| **Brand**          | Card Flag: <br> <br> Visa and Master <br>                                                                           | Text    | 10   |     not     |
| **CardToken**      | Card Token 3.0                                                                                                      | GUID    | 36   | Conditional |


## Request

**STANDARD**

```
{
    "CardNumber":"1234123412341231",
    "Holder":"Alexsander Rosa",
    "ExpirationDate":"12/2021",
    "SecurityCode":"123",
    "SaveCard":"false",
    "Brand":"Visa"
}
```
**With Tokenized Card**

```
{
  "CardToken":"23712c39-bb08-4030-86b3-490a223a8cc9",
    "SaveCard":"false",
    "Brand":"Visa"
}
```

## Response

The response always returns if the card can be authorized at the moment. 
This information only means that the card is able to be authorized, but does not necessarily indicate that a certain amount will be authorized.

Fields returned after validation:

| Field         | Description                                                               | Type    | Size |
|---------------|---------------------------------------------------------------------------|---------|------|
| Valid         | Card Status: <br> **True** - Valid Card <BR> **False** - Invalid Card | Boolean | ---  |
| ReturnCode    | Return code                                                               | text    | 2    |
| ReturnMessage | Return message                                                            | text    | 255  |

> See more about Return Codes and Return Messages on https://developercielo.github.io/Webservice-3.0/english.html#sales-return-codes

> The return code **85 represents success in Zero Auth**, the other codes are defined according to the documentation above .


Response: **POSITIVE - Valid Card**
```
{
        "Valid": true,
        "ReturnCode": “00”,
        "ReturnMessage", “Transacao autorizada”
}
```

Response: **NEGATIVE - Card with flag invalid**

```
  {    
      "Code": 57,     
      "Message": "Bandeira inválida"   
  }
```

If there is any error in request, where it is not possible to validate the card, the service will return error:
* 500 - Internal Server Error







## Zero Auth with AVS

**AVS** is a service  where a validation is performed through the data of the address informed by the buyer
The validation will compare the informations with the data of the bank responsable by the card.
This helps reduce the risk of chargeback.
It should be used for sales analysis, assisting in the decision to capture the transaction.




**AVS Rules**

* Available only for Visa, Mastercard and AmEx.
* Products allowed: only Credit Card.
* The return of the query to the AVS is separated into two items: ZIP code and address.
* Each of them have the following values:




| Value | Description |
|:------: |---------------- |
| C | Check |
| N | Do not check |
| I | Not Available |
| T | Temporarily Not Available  |
| X | Unsupported Service for this Card brand|
| E | Incorrect data sent. Check if all fields have been sent |


* All fields contained in the AVS node must be filled in for analysis to take place.

* When the field is not applicable, it should be submitted filled with NULL or N / A

* It is necessary to enable the AVS option on the Cielo Ecommerce API system. To enable the AVS option, contact Cielo eCommerce Support 


**POST - WITH AVS**

```
{
    "CardType": "CreditCard",
    "CardNumber":"1234123412341231",
    "Holder":"Teste Holder",
    "ExpirationDate":"12/2021",
    "SecurityCode":"123",
    "SaveCard":"false",
    "Brand":"Visa",
    "Avs":{
        "Cpf": "12387719719",
        "ZipCode": "20241180",
        "Street": "Rua da Glória",
        "Number": "214",
        "Complement": "ap 203",
        "District": "Rio de Janeiro"
    }
}
```

Response: **POSITIVE - With AVS**

```
{
       "Valid": true,
       "ReturnCode": "85",
       "ReturnMessage": "Transacao autorizada",
       "AvsCepReturnCode": "I",
       "AvsAddressReturnCode": "I"
}
```

Response Content

| Field         | Description                                                               | Type    | Size       |
|---------------|---------------------------------------------------------------------------|---------|------------|
| Valid         | Card Status: <br> **True** - Valid Card <BR> **False** - Invalid Card     | Boolean | ---        |
| ReturnCode    | Return code                                                               | text    | 2          |
| ReturnMessage | Return message                                                            | text    | 255        |
| AvsCepReturnCode | Status of the Zipcode (CEP) sent: <br> <br> **C** - Check  <br> **N** - Do not check out <br>**I** - Not available <br>**T** - Temporarily out of stock <br> **X** - Service not supported for this Flag <br> **E** - Incorrect data sent. Check that all fields have been submitted | Text | 1 |
| AvsAddressReturnCode | Analysis of the address sent:<br> <br> **C** - Check  <br> **N** - Do not check out <br>**I** - Not available <br>**T** - Temporarily out of stock <br> **X** - Service not supported for this Flag <br> **E** - Incorrect data sent. Check that all fields have been submitted | Text | 1 |



## Update History:

| Version | Date | Description |
|-|-|-|
| 1.0 | 16/09/2016 | ✓ Initial release |
| 1.1 | 10/25/2016 | ✓ Fields update |
| 1.2 | 05/01/2017 | ✓ Inclusion of the mandatory parameter table <br> ✓ Inclusion of the contract without AVS <br> ✓ Inclusion of the contract with Cardtoken
| 1.3 | 05/15/2017 | ✓ Complement of the table with the fields of the requisition contract <br> ✓ Alteration of the table with the fields of the contract of response
| 1.4 | 05/31/2017 | ✓ Added the supported Card Brands topic <br> ✓ Indication that tokens 1.5 are not supported <br> ✓ Response to cards with unsupported  Brand<br> ✓ Response to requests that result in an error and consequently to non-validation of the card. > ✓ Update of the size of the fields of the requisition contract |
| 1.5 | 06/07/2017 | ✓ Added production URL <br> ✓ Added return code 00 for queries that return valid cards.|
| 1.6 | 07/03/2017 | ✓ Added AVS Return <br> ✓ Added AVS Rules |
