---
title: AVS
category: API CIELO ECOMMERCE
order: 5
---
### O QUE É O AVS

O **AVS** é um serviço para transações online onde é realizada uma validação cadastral através do batimento dos dados do endereço informado pelo comprador (endereço de entrega da fatura) na loja virtual, com os dados cadastrais do banco emissor do cartão.
Isso auxilia na redução do risco de chargeback. Deve ser utilizada para análise de vendas, auxiliando na decisão de captura da transação.


### Integração

Para realizar uma transação utilizando o **AVS**, o lojista deverá enviar uma requisição `POST` para a API Cielo Ecommerce,criando uma  transação que contenha o nó **AVS** dentro do nó `Payment.CreditCard`.

Vale Destacar que o AVS deve ser utilizado valendo-se das regras abaixo:


**Regras do AVS**

* Disponível apenas para as bandeiras **Visa, Mastercard e Amex**.
* Produtos permitidos: somente crédito.
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
* Quando o campo não for aplicável (exemplo: complemento), deve ser enviada preenchia com NULL ou N/A
* Necessário habilitar a opção do AVS no cadastro. Para habilitar a opção AVS no cadastro ou consultar os bancos participantes, entre em contato com o Suporte Cielo eCommerce


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

Response: **POSITIVA - Com AVS**

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

| Paramêtro            | Descrição                                                             | Tipo    | Tamanho |
|----------------------|-----------------------------------------------------------------------|---------|---------|
| Valid                | Situação do cartão:<br> **True** – Cartão válido<BR>**False** – Cartão Inválido | Boolean | ---     |
| ReturnCode           | Código de retorno                                                     | texto   | 2       |
| ReturnMessage        | Mensagem de retorno                                                   | texto   | 255     |
| AvsCepReturnCode     | Situação do CEP enviado:<br><br>**C** - Confere<br>**N** - Não confere<br>**I** - Indisponível<br>**T** - Temporariamente indisponível<br>**X** - Serviço não suportado para esta Bandeira<br>**E** - Dados enviados incorretos. Verificar se todos os campos foram enviados<br> | Texto   | 1       |
| AvsAddressReturnCode | Analise do endereço enviado:<br><br>**C** - Confere<br>**N** - Não confere<br>**I** - Indisponível<br>**T** - Temporariamente indisponível<br>**X** - Serviço não suportado para esta Bandeira<br>**E** - Dados enviados incorretos. Verificar se todos os campos foram enviados<br> | Texto   | 1       |



## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 16/09/2016 | ✓ Versão inicial                                                                                               |
| 1.1    | 25/10/2016 | ✓ Atualização de parâmetros                                                                                    |
| 1.2    | 05/01/2017 | ✓ Inclusão da tabela de parâmetros obrigatórios <br> ✓ Inclusão do contrato sem AVS <br>✓ Inclusão do contrato com Cardtoken 
| 1.3    | 15/05/2017 | ✓ Complemento da tabela com os campos do contrato de requisição <br> ✓ Alteração da tabela com os campos do contrato de resposta 
| 1.4    | 31/05/2017 | ✓ Adicionado o tópico de bandeiras suportadas <br> ✓ Indicação que tokens 1.5 não são suportados <br>✓ Resposta para cartões com bandeira não suportada <br>✓ Resposta para requisições que resultem em erro e consequentemente na não validação do cartão.<br> ✓ Atualização do tamanho dos campos do contrato de requisição  |
| 1.5    | 07/06/2017 | ✓ Adicionada URL de produção <br> ✓ Adicionado código de retorno 00 para consultas que retornem cartões válidos.|
| 1.6    | 03/07/2017 | ✓ Adicionada retorno do AVS <br> ✓ Adicionado regras do AVS                                                    |







> Sandbox: https://`apisandbox`.cieloecommerce.cielo.com.br/1/`zeroauth`

> Produção: https://`api`.cieloecommerce.cielo.com.br/1/`zeroauth`

Cada tipo de validação necessita de um contrato tecnico diferente. Eles resultarão em _responses diferenciados_.

Abaixo, a listagem de campos da Requisição:

| Paramêtro      | Descrição                                                                                                             | Tipo    | Tamanho | Obrigatório |
|----------------|-----------------------------------------------------------------------------------------------------------------------|---------|---------|:-----------:|
| CardType       | Define o tipo de cartão utilizados:<br><br>*CreditCard*<br>*DebitCard*<br><br>Se não enviado, CreditCard como default | Texto   | 255     | Não         |
| CardNumber     | Número do Cartão do Comprador                                                                                         | Texto   | 16      | sim         |
| Holder         | Nome do Comprador impresso no cartão.                                                                                 | Texto   | 25      | não         |
| ExpirationDate | Data de e validade impresso no cartão.                                                                                | Texto   | 7       | sim         |
| SecurityCode   | Código de segurança impresso no verso do cartão.                                                                      | Texto   | 4       | não         |
| SaveCard       | Booleano que identifica se o cartão será salvo para gerar o CardToken.                                                | Boolean | ---     | Não         |
| Brand          | Bandeira do cartão: <br><br>Visa<br>Master<br>| Texto   | 10      | não         |
| CardToken      | Token do cartão na 3.0                                                                                                | GUID    | 36      | Condicional |





