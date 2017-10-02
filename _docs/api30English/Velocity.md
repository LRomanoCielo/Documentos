---
title: Velocity
category: API CIELO ECOMMERCE (English)
order: 8
---

### What is Velocity?

Velocity is a type of fraud prevention mechanism that specifically analyzes the concept of "speed X transactional data".
It analyzes how often certain data is used and whether this data is inscribed in a list of security actions designed to prevent frauds.

Velocity offers 4 types of functionality to validate transactional data:

| Functionality             | Description                                                                                                                                          |
|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Velocity safety rules** | The Merchant defines a set of security rules that will be used to evaluate if certain transactional data is repeated in a suspicious time interval   |
| **Quarantine**            | Creation of a list of data that will be analyzed for a determined period of time before being considered valid or fraudulent                         |
| **BlackList**             | Creation of a list of data that, when identified,will prevent the transaction from being executed, avoiding the creation of a fraudulent transaction |
| **Whitelist**             | Creation of a list of data that when identified, will allow the transaction to be executed, even if there are security rules in action               |


The main functions of Velocity are:

* Auxiliar na detecção de elementos suspeitos de fraude. 
* Fonte de dados para bloquear ataques em rajada (testes de cartão, por exemplo), bem como avaliações de comportamentos suspeitos de compra. 
* Cálculos baseados em análise de velocidade de elementos rastreáveis e a incidência dos mesmos em determinados intervalos de tempo

* Assist in detecting elements/data suspected of fraud.
* Data source for blocking attacks (card tests, for example), as well as evaluations of suspicious purchase behavior.
* Calculations based on speed analysis of traceable elements and their incidence at certain time intervals.

The functionality must be enable by Cielo Support Team.

## Creating Velocity Safety Rules


The functionality must be enable by Cielo Support Team. After Activation, inform Cielo's Support Team of which `Elements of traceability` would be used as Safety rules


`Elements of traceability` are:

| ** Traceability elements ** |
|: -----------------------------------------|
| 12 first digits of credit card |
| Credit card holder name |
| Billing address zip code |
| Credit card number |
| Delivery address Zip Code |
| Buyer's Document/ID |
| Buyer's Email |
| Order number |
| Buyer's IP |

The analysis takes place over each `Elements of traceability` (ER), counting how many times (Q) the element was identified within a given period (P) 

| Variavel | Description |
| :--------: | ----------------------------- |
| **ER** | Elements of traceability |
| **Q** | Quantity |
| **P** | Period |


These variables are analyzed using the following formula:

* **ER** = Credit card number
* **Q** = Maximum of 5 hits
* **P** = 12 hours

**Rule** = Maximum of 5 card hits in 12 hour(s)


> **With this formula, the Velocity performs the following comparison:**
When receiving the 6th transaction with the same card number (ER) from the previous 5, the above rule is executed and detects that the quantity (Q) exceeded the 5 allowed in the period (P) between the date of the first transaction and the date of the 6th received, it will have the status **rejected**, the card will be set in quarantine and the Response will contain which  transaction was blocked due to the safety rule.


## Transaction with Velocity 

The Velocity works by analyzing data submitted in the standard Cielo Ecommerce API integration. 
It is not necessary to include any additional nodes to the integration for the creation of a transction, but it will be necessary to change response treatment .

When Velocity is active, the transaction response will bring a specific node called "Velocity" with the details of the analysis.

| Property                                 | Description                                               | Type   | Size |
|------------------------------------------|-----------------------------------------------------------|--------|------|
| `VelocityAnalysis.Id`                    | Analysis confirmation ID                                  | GUID   | 36   |
| `VelocityAnalysis.ResultMessage`         | Accept or Reject                                          | Text   | 25   |
| `VelocityAnalysis.Score`                 | 100                                                       | Number | 10   |
| `VelocityAnalysis.RejectReasons.RuleId`  | Rule Code - Defines which rule was used into the Analysis | Number | 10   |
| `VelocityAnalysis.RejectReasons.Message` | Description of Rule used into the Analysis                | Text   | 512  |

### Response

```json
{
  "MerchantOrderId": "2017051202",
  "Customer": {
    "Name": "Nome do Comprador",
    "Identity": "12345678909",
    "IdentityType": "CPF",
    "Email": "comprador@cielo.com.br",
    "Address": {
      "Street": "Alameda Xingu",
      "Number": "512",
      "Complement": "27 andar",
      "ZipCode": "12345987",
      "City": "São Paulo",
      "State": "SP",
      "Country": "BRA"
    },
    "DeliveryAddress": {
      "Street": "Alameda Xingu",
      "Number": "512",
      "Complement": "27 andar",
      "ZipCode": "12345987",
      "City": "São Paulo",
      "State": "SP",
      "Country": "BRA"
    }
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": "ByMerchant",
    "Capture": true,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "CardNumber": "455187******0181",
      "Holder": "Nome do Portador",
      "ExpirationDate": "12/2027",
      "SaveCard": false,
      "Brand": "Undefined"
    },
    "VelocityAnalysis": {
      "Id": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
      "ResultMessage": "Reject",
      "Score": 100,
      "RejectReasons": [
        {
          "RuleId": 49,
          "Message": "Bloqueado pela regra CardNumber. Name: Máximo de 3 Hits de Cartão em 1 dia. HitsQuantity: 3. HitsTimeRangeInSeconds: 1440. ExpirationBlockTimeInSeconds: 1440"
        }
      ]
    },
    "PaymentId": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
    "Type": "CreditCard",
    "Amount": 10000,
    "Currency": "BRL",
    "Country": "BRA",
    "Provider": "Simulado",
    "ReasonCode": 16,
    "ReasonMessage": "AbortedByFraud",
    "Status": 0,
    "ProviderReturnCode": "BP171",
    "ProviderReturnMessage": "Rejected by fraud risk (velocity)",
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/2d5e0463-47be-4964-b8ac-622a16a2b6c4"
      }
    ]
  }
}
```


## Quarantine

Quarantine is a data base that stores the values by type of `Element of traceability` with a given expiration time.

When registering a rule it is possible to specify how long the value of a particular `Element of traceability` will be taken into account in the next analysis; if the merchant wants to identify the number of times the same card number has been repeated for a 12-hour period within a 2-day interval, Velocity will not be required to perform this retroactive counting by grouping by period. In this scenario,  the application would have to perform the count for the following ranges:

* D-2 = 0h as 12h 
* D-2 = 12h as 0h 
* D-1 = 0h as 12h 
* D-1 = 12h as 0h 

With the quarantine, the application will not perform this counting by period, because when performing an analysis, it will check if there is any value of the `Element of traceability` in quarantine.


**Exemplo:** Usando a regra acima, o tempo de expiração se definido em 2 dias, a regra será analisada apenas para o período já configurado, ou seja, 12 horas para traz e irá verificar durante 2 dias se número do cartão se encontra em quarentena. 


**OBS**: Uma transação analisada, não bloqueada pela regra, mas bloqueada pela quarentena, tem o retorno informando que a mesma foi bloqueada pela quarentena.

```
{  
      "RuleId":18,
      "Message":"Bloqueado pela Quarentena - regra CardNumber. Name: Máximo de 
5 Hits de Cartão em 12 hora(s). HitsQuantity:5. HitsTimeRangeInSeconds:43200. ExpirationBlockTimeInSeconds:86400"
   }
],
"CorrelationId":"71c72be9-d16c-48d6-b949- 
68f16835a772",
"ResultMessage":"Reject",
"Score":100,
"ByPassed":false,
"TransactionId":"a221f50c-14b4-483d-bfe3- 
ea7549c148b9",
"Links":[  
   {  
      "Method":"GET",
      "Rel":"self",
      "Href":"https://apiquery.cieloecommerce.cielo.com.br/Analysis/a221f50c-14b4-483d-bfe3-ea7549c148b9"
   }
]
}

```


### Como configurar uma Quarentena 

 A configuração da Quarentena é realizada via o HD Cielo. Basta solicitar a liberação da funcionalidade Velocity informando os dados abaixo:
 
* Valor
* Data da expiração da quarentena
* Regras:

Máximo de 5 Hits de Cartão em 12 horas
Máximo de 5 Hits de Documentos em 12 horas
Máximo de 7 Hits de Cartão em 7 dias
Máximo de 7 Hits de Documentos em 7 dias







## Blacklist

A BlackList é uma tabela oferecida pelo Velocity onde se armazena valores por tipo de `elementos de rastreabilidades` que o lojista deseja **bloquear** automaticamente.

Em transação a ser analisada, caso  `elementos de rastreabilidades`/Documento que esteja na blacklist, a mesma será bloqueada, independente de existir
regra cadastra para este tipo de elemento de rastreabilidade ou não, e terá o retorno informado que a mesma foi **bloqueada pela blacklist**, ou seja, a transação não será enviada a Autorização


### Como configurar uma Blacklist

 A configuração da Blacklist é realizada via o HD Cielo. Basta solicitar a liberação da funcionalidade no Velocity informando os dados abaixo:
 
* Qual `elemento de rastreabilidade` deverá ser bloqueado / EX: Identidade
* Informar o valor do `elemento de rastreabilidade`a ser bloqueado /EX: Identidade = 21.435.787-95

É possivel realizar varios cadastros para diferentes `elementos de rastreabilidades`. Caso eles sejam reconhecidos no contrato, a transação não será enviada a Autorização






## Whitelist


A Whitelist é uma tabela oferecida pelo Velocity onde se armazena valores por tipo de `elementos de rastreabilidades` que o lojista deseja **liberar** automaticamente das regras de segurança do velocity.

Em transação a ser analisada, caso  `elementos de rastreabilidades`/Documento que esteja na Whitelist, a mesma  não será analisada pelo velocity, independente de existir
regra cadastra para este tipo de elemento de rastreabilidade ou não, sendo enviada para a autorização como uma transação normal.


### Como configurar uma Whitelist

 A configuração da Whitelist é realizada via o HD Cielo. Basta solicitar a liberação da funcionalidade no Velocity informando os dados abaixo:
 
* Qual `elemento de rastreabilidade` deverá ser incluso na Whitelist / EX: Identidade
* Informar o valor do `elemento de rastreabilidade`a ser liberado /EX: Identidade = 21.435.787-95

É possivel realizar varios cadastros para diferentes `elementos de rastreabilidades`. Caso eles sejam reconhecidos no contrato, a transação será enviada a Autorização sem ser analisada pelo Velocity






## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 28/08/2017 | ✓ Versão inicial                                                                                               |


