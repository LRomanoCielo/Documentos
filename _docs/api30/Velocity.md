---
title: Velocity
category: API CIELO ECOMMERCE
order: 6
---
### O QUE É O Velocity

O **AVS** é um serviço para transações online onde é realizada uma validação cadastral através do batimento dos dados do endereço informado pelo comprador (endereço de entrega da fatura) na loja virtual, com os dados cadastrais do banco emissor do cartão.
Isso auxilia na redução do risco de chargeback. Deve ser utilizada para análise de vendas, auxiliando na decisão de captura da transação.


### Integração

Para realizar uma transação utilizando o **AVS**, o lojista deverá enviar uma requisição `POST` para a API Cielo Ecommerce,criando uma  transação que contenha o nó **AVS** dentro do nó `Payment.CreditCard`.

Vale Destacar que o AVS deve ser utilizado valendo-se das regras abaixo:


**Regras do AVS**

* Produtos permitidos: somente crédito.
* Disponível apenas para as bandeiras **Visa, Mastercard e Amex**.
* O retorno da consulta ao AVS é separado em dois itens: CEP e endereço.
* Cada um deles pode ter os seguintes valores: 

| Valor | Descrição                                                              |
|:-----:|------------------------------------------------------------------------|
| C     | Confere                                                                |
| N     | Não confere                                                            |
| I     | Indisponível                                                           |
| T     | Temporariamente indisponível                                           |
| X     | Serviço não suportado para esta Bandeira                               |
| E     | Dados enviados incorretos. Verificar se todos os campos foram enviados |


* É necessário que todos os campos contidos no nó AVS sejam preenchidos para que a analise seja realizada.
* Quando o campo não for aplicável (exemplo: complemento), deve ser enviada preenchido com NULL ou N/A
* Necessário habilitar a opção do AVS no cadastro. Para habilitar a opção AVS no cadastro ou consultar os bancos participantes, entre em contato com o Suporte Cielo eCommerce


Conteudo do **Nó AVS**


| Paramêtro      | Descrição                                       | Tipo  | Tamanho | Obrigatório |
|----------------|-------------------------------------------------|-------|:-------:|:-----------:|
| Avs.Cpf        | CPF do portador                                 | texto | 11      | Não         |
| Avs.ZipCode    | CEP do endereço de cobrança do portador         | texto | 8       | Não         |
| Avs.Street     | Logradouro do endereço de cobrança do portador  | texto | 50      | Não         |
| Avs.Number     | Número do endereço de cobrança do portador      | texto | 6       | Não         |
| Avs.Complement | Complemento do endereço de cobrança do portador | texto | 30      | Não         |
| Avs.District   | Bairro do endereço de cobrança do portador      | texto | 20      | Não         |



Conteudo do **POST - COM AVS**
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








