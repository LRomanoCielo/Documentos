---
title: Velocity
category: API CIELO ECOMMERCE
order: 6
---
### O que é Velocity

O Velocity é um produto que identifica os comportamentos suspeitos de fraude. A ferramenta tem o intuito de auxiliar na análise de fraude por um custo bem menor que uma ferramenta mais tradicional de mercado. Ela é uma aliada na avaliação de comportamentos suspeitos de compra, pois os cálculos serão baseados em elementos de rastreabilidade.

* Auxiliar na detecção de suspeitas de fraude. 
* Aliada para bloquear ataques em rajada (testes de cartão, por exemplo), bem como avaliações de comportamentos suspeitos de compra. 
* Cálculos baseados em análise de velocidade de elementos rastreáveis e a incidência dos mesmos em determinados intervalos de tempo






## Transação com Velocity 

O Velocity Check é um tipo de mecanismo de prevenção às tentativas de fraude, que analisa especificamente o conceito de "velocidade". Ele analisa a frequência de elementos de rastreabilidade tais como Número do Cartão, CPF, CEP de entrega, entre outros. A funcionalidade deve ser contratada à parte, e posteriormente habilitada em sua loja. Quando o Velocity está ativo, a resposta da transação trará um nó específico chamado "Velocity", com os detalhes da análise.

No caso da rejeição pela regra de Velocity, o ProviderReasonCode será BP171 - Rejected by fraud risk (velocity, com ReasonCode 16 - AbortedByFraud 

### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/sales/{PaymentId}/void?amount=xxx</span></aside>

```json
{
   "MerchantOrderId":"2017051202",
   "Customer":{
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "IpAdress":"127.0.01",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA"
      },
	  "DeliveryAddress": {
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA"
	    }
   },
   "Payment":{
     "Provider":"Simulado",
     "Type":"CreditCard",
     "Amount":10000,
	 "Capture":true,
     "Installments":1,
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Nome do Portador",
         "ExpirationDate":"12/2027",
         "SecurityCode":"123",
         "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request PUT "https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/void?amount=xxx"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
{
   "MerchantOrderId":"2017051202",
   "Customer":{
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "IpAdress":"127.0.01",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA"
      },
	  "DeliveryAddress": {
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA"
	    }
   },
   "Payment":{
     "Provider":"Simulado",
     "Type":"CreditCard",
     "Amount":10000,
	 "Capture":true,
     "Installments":1,
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Nome do Portador",
         "ExpirationDate":"12/2027",
         "SecurityCode":"123",
         "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|-----------|----|-------|-----------|---------|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Braspag|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Braspag|
|`RequestId`|Guid|36|Não|Identificador do Request definido pela loja, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido|
|`Customer.Name`|Texto|255|Sim|Nome do comprador|
|`Customer.Identity`|Texto |14 |Sim|Número do RG, CPF ou CNPJ do Cliente| 
|`Customer.IdentityType`|Texto|255|Sim|Tipo de documento de identificação do comprador (CPF ou CNPJ)|
|`Customer.Email`|Texto|255|Sim|Email do comprador|
|`Customer.IpAddress`|Texto|255|Sim|Ip do comprador|
|`Customer.Address.Street`|Texto|255|Não|Endereço de contato do comprador|
|`Customer.Address.Number`|Texto|15|Não|Número endereço de contato do comprador|
|`Customer.Address.Complement`|Texto|50|Não|Complemento do endereço de contato do Comprador|
|`Customer.Address.ZipCode`|Texto|9|Sim|CEP do endereço de contato do comprador|
|`Customer.Address.City`|Texto|50|Não|Cidade do endereço de contato do comprador|
|`Customer.Address.State`|Texto|2|Não|Estado do endereço de contato do comprador|
|`Customer.Address.Country`|Texto|35|Não|Pais do endereço de contato do comprador|
|`Customer.Address.District`|Texto |50 |Não|Bairro do Comprador. |
|`Customer.DeliveryAddress.Street`|Texto|255|Não|Endereço do comprador|
|`Customer.DeliveryAddress.Number`|Texto|15|Não|Número do endereço de entrega do pedido|
|`Customer.DeliveryAddress.Complement`|Texto|50|Não|Complemento do endereço de entrega do pedido|
|`Customer.DeliveryAddress.ZipCode`|Texto|9|Sim|CEP do endereço de entrega do pedido|
|`Customer.DeliveryAddress.City`|Texto|50|Não|Cidade do endereço de entrega do pedido|
|`Customer.DeliveryAddress.State`|Texto|2|Não|Estado do endereço de entrega do pedido|
|`Customer.DeliveryAddress.Country`|Texto|35|Não|Pais do endereço de entrega do pedido|
|`Customer.DeliveryAddress.District`|Texto |50 |Não|Bairro do Comprador. |
|`Payment.Provider`|Texto|15|Sim|Nome da provedora de Meio de Pagamento|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos)|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas|
|`CreditCard.CardNumber`|Texto|16|Sim|Número do Cartão do comprador|
|`CreditCard.Holder`|Texto|25|Sim|Nome do Comprador impresso no cartão|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão, no formato MM/AAAA|
|`CreditCard.SecurityCode`|Texto|4|Sim|Código de segurança impresso no verso do cartão|
|`CreditCard.Brand`|Texto|10|Sim |Bandeira do cartão|

### Resposta

```json
{
  "MerchantOrderId": "2017051202",
  "Customer": {
    "Name": "Nome do Comprador",
    "Identity": "12345678909",
    "IdentityType": "CPF",
    "Email": "comprador@braspag.com.br",
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
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/2d5e0463-47be-4964-b8ac-622a16a2b6c4"
      }
    ]
  }
}
```

```shell
{
  "MerchantOrderId": "2017051202",
  "Customer": {
    "Name": "Nome do Comprador",
    "Identity": "12345678909",
    "IdentityType": "CPF",
    "Email": "comprador@braspag.com.br",
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
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/2d5e0463-47be-4964-b8ac-622a16a2b6c4"
      }
    ]
  }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|-----------|---------|----|-------|-------|
|`VelocityAnalysis.Id`|Identificador da análise efetuada|GUID|36|
|`VelocityAnalysis.ResultMessage`|Accept ou Reject|Texto|25|
|`VelocityAnalysis.Score`|100|Número|10|
|`VelocityAnalysis.RejectReasons.RuleId`|Código da Regra que rejeitou|Número|10|
|`VelocityAnalysis.RejectReasons.Message`|Descrição da Regra que rejeitou|Texto|512|

























## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 22/08/2017 | ✓ Versão inicial                                                                                               |








