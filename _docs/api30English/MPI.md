---
title: External MPI 
category: API CIELO ECOMMERCE (English)
order: 3
---


### What is "External MPI"?

**MPI** is a bank authentication process, where the Bank validates the a buyer's identity during the transaction flow. 
The **External MPI** is a intgration that allows Merchants to use their own system to check and validate Buyer's Identities



### Integration

In order to use external MPI, retailers that integrate the [ECOMMERCE CIELO API] (https://developercielo.github.io/Webservice-3.0/) should include an additional node in the authorization request

> Only Merchants integrated into the CIELO ECOMMERCE API  will be able to use this feature. This feature is not available in `SANDBOX`.


Required fields for the use of **External MPI**

| Field     | Description                                                           | Type | Size       | Required |
|-----------|-----------------------------------------------------------------------|------|------------|----------|
| Cavv      | Cardholder authentication verification value - Cryptographic Key      | Text | 255        | Yes      |
| XId       | Transaction identifier returned by the Bank                           | Text | 255        | Yes      |
| Eci       | Eletronic Commerce Indicator - Represents how secure a transaction is | Text | 10         | Yes      |


### Request

Request - **POST**:
```
{  
   "MerchantOrderId":"2014111707",
   "Customer":{  
      "Name":"Paulo Henrique",
      "Email":"paulohenriquesi@live.com",
      "Birthdate":"1991-01-02",
      "Address":{  
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      }
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Currency":"BRL",
     "Country":"BRA",
     "Carrier":"Cielo",
     "ServiceTaxAmount":0,
     "ReturnUrl":"https://www.braspag.com.br",
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":true,
     "Authenticate":true,
     "ExternalAuthentication":{
       "Cavv":"AAABB2gHA1B5EFNjWQcDAAAAAAB=",
       "Xid":"Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
       "Eci":"5"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     }
   }
}
```


### Response

The returned fields are the same as those returned in a standard transaction, which can be viewed in [ECOMMERCE CIURE API] documentation (https://developercielo.github.io/Webservice-3.0/)


Response
```
{
    "MerchantOrderId": "2014111707",
    "Customer": {
        "Name": "Paulo Henrique",
        "Email": "paulohenriquesi@live.com",
        "Birthdate": "1991-01-02",
        "Address": {
            "Street": "Rua Júpter",
            "Number": "174",
            "Complement": "AP 201",
            "ZipCode": "21241140",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": true,
        "Authenticate": true,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Master"
        },
        "Tid": "10447480686GS8SUBE9B",
        "ProofOfSale": "216019",
        "ReturnUrl": "https://www.braspag.com.br",
        "Provider": "Cielo",
        "Eci": "5",
        "ExternalAuthentication": {
            "Cavv": "AAABB2gHA1B5EFNjWQcDAAAAAAB=",
            "Xid": "Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
            "Eci": "5"
        },
        "VelocityAnalysis": {
            "Id": "c6fce3bc-24af-4768-a63e-86b22ed0ea36",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "PaymentId": "c6fce3bc-24af-4768-a63e-86b22ed0ea36",
        "Type": "CreditCard",
        "Amount": 100,
        "ReceivedDate": "2017-06-26 16:28:52",
        "Currency": "BRL",
        "Country": "BRA",
        "ReturnCode": "KA",
        "ReturnMessage": "Autorizacao negada",
        "Status": 3,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/c6fce3bc-24af-4768-a63e-86b22ed0ea36"
            }
        ]
    }
}
```



### Update history:

| Version  | Date         | Description     |
| -------- | ------------ | --------------- |
| 1.0      | 20/09/2016   | First Version   |
| 1.1      | 02/10/2017   | English Version |

