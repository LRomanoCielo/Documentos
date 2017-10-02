---
title: AVS
category: API CIELO ECOMMERCE (English)
order: 7
---
### What is AVS?


The **AVS** is a system that analyzes and compares the billing address informed by the buyer to the billing address registered at the bank that issued the credit card. 
This helps in reducing the risk of chargeback. It should be used for sales analysis, assisting in the decision to capture the transaction.

### Integration

To perform a transaction using **AVS**, the merchant must send a POST request to the Cielo Ecommerce API, creating a transaction that contains the **AVS** node within the `Payment.CreditCard` node.

It is worth emphasizing that AVS should be used using the fallow rules:

**AVS Rules**

* Products allowed: only credit card.
* Available only for the **Visa, MasterCard and Amex** brands.
* The return of the query to the AVS is separated into two items: ZIP code and address.
* Each of them can have the following values:

 
| Value | Description                                                  |
|:-----:|--------------------------------------------------------------|
|     C | Check                                                        |
|     N | Do not check                                                 |
|     I | Not Available                                                |
|     T | Temporarily unavailable                                      |
|     X | Unsupported Service for this brand                           |
|     E | Incorrect data sent. Check if all fields have been submitted |

* All fields contained in the AVS node must be send for analysis to take place.
* When the field is not applicable, it should be sent filled with **NULL or N/A**
* It is necessary to enable the AVS option in the MerchantID. To enable the AVS option or consult the participating banks, contact Support eCommerce Cielo

**AVS node content**

| Field            | Description                                      | Type    | Size       | Required       |
| ---------------- | ------------------------------------------------ | ------- | : -------: | : -----------: |
| Avs.Cpf          | Buyer's CPF                                      | text    | 11         | No             |
| Avs.ZipCode      | Zip code - billing address                       | text    | 8          | No             |
| Avs.Street       | Address of the Buyer's billing address           | text    | 50         | No             |
| Avs.Number       | Buyer's Billing address number                   | text    | 6          | No             |
| Avs.Complement   | Supplement - Buyer's billing address             | text    | 30         | No             |
| Avs.District     | Neighborhood/District - Buyer's billing address  | text    | 20         | No             |




**POST - WITH AVS**
```
{
   "MerchantOrderId":"2014111703",
   "Payment":{
     "Type":"CreditCard",
     "Amount":15700,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "Avs":{
        	"Cpf": "10939107716",
        	"ZipCode": "24320570",
        	"Street": "Estrada Caetano Monteiro",
        	"Number": "391",
        	"Complement": "Bl",
        	"District": "Niteroi"
    	}
     }
   }
}
```

Response: **AVS**

```
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "455187******0181",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa",
            "Avs": {
                "Cpf": "10939107716",
                "ZipCode": "24320570",
                "Street": "Estrada Caetano Monteiro",
                "Number": "391",
                "Complement": "Bl",
                "District": "Niteroi",
                "Status": 9,
                "ReturnCode": "I"
            }
        },
        "Tid": "10447480686IHEHPA33B",
        "ProofOfSale": "279003",
        "SoftDescriptor": "123456789ABCD",
        "Provider": "Cielo",
        "Eci": "7",
        "VelocityAnalysis": {
            "Id": "467a37d0-435e-4729-b1b4-6a2c4ea7360e",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "PaymentId": "467a37d0-435e-4729-b1b4-6a2c4ea7360e",
        "Type": "CreditCard",
        "Amount": 15700,
        "ReceivedDate": "2017-08-22 15:45:06",
        "Currency": "BRL",
        "Country": "BRA",
        "ReturnCode": "14",
        "ReturnMessage": "Autorizacao negada",
        "Status": 3,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/467a37d0-435e-4729-b1b4-6a2c4ea7360e"
            }
        ]
    }
}
```





Conteudo do Response

| Paramêtro      | Descrição                                       | Tipo  | Tamanho |
|----------------|-------------------------------------------------|-------|:-------:|
| Avs.Cpf        | CPF do portador                                 | texto | 11      | 
| Avs.ZipCode    | CEP do endereço de cobrança do portador         | texto | 8       | 
| Avs.Street     | Logradouro do endereço de cobrança do portador  | texto | 50      |
| Avs.Number     | Número do endereço de cobrança do portador      | texto | 6       | 
| Avs.Complement | Complemento do endereço de cobrança do portador | texto | 30      |
| Avs.District   | Bairro do endereço de cobrança do portador      | texto | 20      |
| AvsCepReturnCode     | Situação do CEP enviado:<br><br>**C** - Confere<br>**N** - Não confere<br>**I** - Indisponível<br>**T** - Temporariamente indisponível<br>**X** - Serviço não suportado para esta Bandeira<br>**E** - Dados enviados incorretos. Verificar se todos os campos foram enviados<br> | Texto   | 1       |
| AvsAddressReturnCode | Analise do endereço enviado:<br><br>**C** - Confere<br>**N** - Não confere<br>**I** - Indisponível<br>**T** - Temporariamente indisponível<br>**X** - Serviço não suportado para esta Bandeira<br>**E** - Dados enviados incorretos. Verificar se todos os campos foram enviados<br> | Texto   | 1       |



## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 22/08/2017 | ✓ Versão inicial                                                                                               |








