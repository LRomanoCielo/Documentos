---
title: Velocity
category: API CIELO ECOMMERCE
order: 6
---
### O que é Velocity

O Velocity é um tipo de mecanismo de prevenção à tentativas de fraude, que analisa especificamente o conceito de "velocidade X dados transacionais". 
Ela analisa a frequência que determinados dados são utilizados e se esse dados estão inscritos em uma lista de comportamentos passiveis de ações de segurança.


O Velocity oferece 4 tipos de funcionalidades para validar dados transacionais:

| Funcionalidade                   | Descrição                                                                                                                                          |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| **Regras de segurança velocity** | O Lojista defini um grupo de regras de segurança que vão avaliar se determinados dados transacionais se repetem em um intervalo de tempo suspeito  |
| **Quarentena**                   | Criação de uma lista de dados que serão analisados por um periodo de tempo determinado antes de serem considerados validos ou fraudulentos         |
| **BlackList**                    | Criação de uma lista de dados que ao serem identificados impedem a transação de ser executada, evitando a criação de uma transação fraudulenta     |
| **Whitelist**                    | Criação de uma lista de dados que ao serem identificados permitem que a transação de seja executada, mesmo que existam regras de segurança em ação |

As principais funções do Velocity são:

* Auxiliar na detecção de elementos suspeitos de fraude. 
* Fonte de dados para bloquear ataques em rajada (testes de cartão, por exemplo), bem como avaliações de comportamentos suspeitos de compra. 
* Cálculos baseados em análise de velocidade de elementos rastreáveis e a incidência dos mesmos em determinados intervalos de tempo


A funcionalidade deve ser contratada à parte, e posteriormente habilitada em sua loja pela equipe de Suporte Cielo Ecommerce. 



## Criando Regras de segurança Velocity 

Basta solicitar ao HD Cielo que a funcionalidade seja ativada em seu cadastro e que as regras seja definidas sobre os `Elementos de rastreabilidade`

`Elementos de rastreabilidade` são:


| **Elementos de Rastreabilidade**          |
|:-----------------------------------------:|
| 12 primeiros dígitos do cartão de crédito |
| Nome do portador do cartão de crédito     |
| CEP do endereço de cobrança               |
| Número do cartão de crédito               |
| CEP do endereço de entrega                |
| Documento do comprador                    |
| E-mail do comprador                       |
| Número do pedido                          |
| IP do comprador                           |


A análise ocorre em cima de cada `elemento de rastreabilidade` (ER), contando quantas vezes (Q) o elemento foi identificado dentro de um determinado período (P)
 
| Variavel | Descrição                   |
|----------|-----------------------------|
| **ER**   | Elemento de Rastreabilidade |
| **Q**    | Quantidade                  |
| **P**    | Período                     |


Essas variaveis são analisadas seguindo a seguinte formula:

* **ER** = Número do cartão de crédito
* **Q** = Máximo de 5 hits 
* **P** = 12 horas 

**Regra** = Máximo de 5 hits de cartão em 12 hora(s) 
 
> **Com isso, o Velocity executa a seguinte comparação:**
Ao receber a 6ª transação com o mesmo número de cartão (ER) das outras 5 anteriores, a regra acima ao ser executada e detectar que a quantidade (Q) excedeu as 5 permitidas no período (P) entre a data da primeira transação e a data da 6ª recebida, a mesma terá o **status de rejeitada**, o cartão irá para quarentena e a resposta terá o conteúdo de que a transação foi bloqueada devido a regra. 


## Transação com Velocity 

O Velocity funciona analisando dados enviados na integração padrão da API Cielo Ecommerce. Não é necessario incluir nenhuma nós adicional a integração da loja para a criação da venda, mas será necessario alterar a forma como os dados são recebidos response.

Quando o Velocity está ativo, a resposta da transação trará um nó específico chamado "Velocity", com os detalhes da análise.


Dados dos retornados pelo Velocity

| Propriedade                              | Descrição                         | Tipo   | Tamanho |
|------------------------------------------|-----------------------------------|--------|---------|
| `VelocityAnalysis.Id`                    | Identificador da análise efetuada | GUID   | 36      |
| `VelocityAnalysis.ResultMessage`         | Accept ou Reject                  | Texto  | 25      |
| `VelocityAnalysis.Score`                 | 100                               | Número | 10      |
| `VelocityAnalysis.RejectReasons.RuleId`  | Código da Regra que rejeitou      | Número | 10      |
| `VelocityAnalysis.RejectReasons.Message` | Descrição da Regra que rejeitou   | Texto  | 512     |


### Resposta

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


## Quarentena 


A Quarentena é  uma tabela que armazena os valores por tipo de `Elementos de rastreabilidade` com um determinado tempo de expiração.

Ao cadastrar uma regra é possível especificar quanto tempo o valor de um determinado `Elemento de rastreabilidade` irá ser levado em consideração nas próximas análises, ou seja, se o lojista quiser identificar a quantidade de vezes que o mesmo número de cartão se repetiu para um período de 12 horas dentro de um intervalo de 2 dias, não será necessário o Velocity realizar esta contagem retroativa agrupando por período. Neste cenário por exemplo, a aplicação teria que realizar a contagem para os seguintes intervalos: 

* D-2 = 0h as 12h 
* D-2 = 12h as 0h 
* D-1 = 0h as 12h 
* D-1 = 12h as 0h 

Com a quarentena, a aplicação não irá realizar essa contagem retroativa por período, pois ao realizar uma análise, será  verifica se existe algum valor do `Elemento de rastreabilidade` em quarentena. 

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
| 1.0    | 22/08/2017 | ✓ Versão inicial                                                                                               |








