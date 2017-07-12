---
title: MPI Externo
category: API CIELO ECOMMERCE
order: 3
---

### O QUE É O MPI EXTERNO

O **MPI** é um processo de autenticação bancário, onde o Banco valida a identidade do comprador durante o fluxo transacional.  O Banco retorna à loja o resultado da validação de identidade, confirmando ou não a autenticação..

### Integração

Para utilizar o MPI externo, os lojistas que integrarem a [API CIELO ECOMMERCE](https://developercielo.github.io/Webservice-3.0/) deverão incluir um nó adicional na requisição de autorização

> Somente lojistas integrados a API CIELO ECOMMERCE em Produção poderão utilizar esta feature. Ela não se encontra disponivel em `SANDBOX`


Abaixo, os campos necessarios para o uso do **MPI Externo**

| Paramêtro | Descrição                                                               | Tipo  | Tamanho | Obrigatório |
|-----------|-------------------------------------------------------------------------|-------|---------|-------------|
| Cavv      | Cardholder authentication verification value – Chave criptográfica      | Texto | 255     | Sim         |
| XId       | Identificador de transação retornado pelo Banco                         | Texto | 255     | Sim         |
| Eci       | Eletronic Commerce Indicator - Representa o quão segura é uma transação | Texto | 10      | Sim         |


### Requisição

Conteudo de **POST**:
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


### Resposta

Os parametros retornados são os mesmos respondidos em uma transação padrão, que pode ser visualizado na documentação da [API CIELO ECOMMERCE](https://developercielo.github.io/Webservice-3.0/)

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



### Histórico de atualizações:

| Versão | Data       | Descrição      |
|--------|------------|----------------|
| 1.0    | 20/09/2016 | Versão inicial |



