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

Basta solicitar ao HD Cielo que a funcionalidade seja ativada em seu cadastro e que as regras seja definidas sobre os "Elementos de rastreabilidade"

"Elementos de rastreabilidade" são:


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


A análise ocorre em cima de cada elemento de rastreabilidade (ER), contando quantas vezes (Q) o elemento foi identificado dentro de um determinado período (P)
 
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
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/2d5e0463-47be-4964-b8ac-622a16a2b6c4"
      }
    ]
  }
}
```


## Quarentena 






## Blacklist





## Whitelist




















## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 22/08/2017 | ✓ Versão inicial                                                                                               |








