---
title: Bin Query
category: API CIELO ECOMMERCE (English)
order: 1
---

### ** What is "BIN Query" **


"** Bins Query **" is a ** credit or debit card ** search service that Cielo Ecommerce API merchants can use to validate if the card provided by the costumer is valid 

The "** Bins query" ** returns the following data about the card:

* ** Cards Brand ** - Mastercard and Visa
* ** Card Type: ** Credit, Debit or Multiple (Credit and Debit)
* ** Card Nationality : ** foreign or national (Brazilian)

For more information on Cielo Ecommerce API (Sandbox and Production), please visit: <https://developercielo.github.io/Webservice-3.0/>


### Integration

Just perform a `GET` sending the BIN to our endpoint (query URL) :

> https: // `apiquerysandbox`.cieloecommerce.cielo.com.br / 1 / cardBin /` BIN`

### Requisition

**Request**:


```
https://apiquerysandbox.cieloecommerce.cielo.com.br/1/cardBin/420020
```

### Response

```
{
"Status": "00",
"Provider": "Visa",
"CardType": "Credit",
"ForeignCard": "true"
}
```


| Field             | Type | Contact Us | Description                                                                                                                                                      |
|-------------------|------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ** Status **      | Text | 2          | Bins Analysis Request Status: <br> <br> 00 - Authorized Analysis <br> 01 - Unsupported Brand <br> 02 - Unsupported Bin in Bin Query <br> 73 - Blocked Membership |
| ** Provider **    | Text | 255        | Greeting Card                                                                                                                                                    |
| ** CardType **    | Text | 20         | Type of card in use: <br> <br> Credito <br> Debito <br> Multiplo                                                                                                 |
| ** ForeingCard ** | Text | 255        | If the card is a Foreing Card (False / True)                                                                                                                     |


### Update History

| Version | Date       | Description                    |
|---------|------------|--------------------------------|
| 1.0     | 19/09/2016 | Home                           |
| 1.1     | 02/14/2017 | Changing "Brand" to "Provider" |