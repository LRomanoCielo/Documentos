---
title: Zero Auth
category: API CIELO ECOMMERCE
order: 2
---
### O QUE É O ZERO AUTH

O **Zero Auth** é uma ferramenta de validação de cartões da API Cielo. A validação permite que o lojista saiba se o cartão é valido ou não antes de enviar a transação para autorização, antecipando o motivo de uma provável não autorização.

O **Zero Auth** pode ser usado de 3 maneiras:

1. **Padrão** - Envio de um cartão padrão, sem tokenização ou analises adicionais
2. **Com cartão Tokenizado** - Envio de um TOKEN 3.0 para analise
3. **Com AVS** - Utilizando o AVS Cielo para validar o cartão


É importante destacar que o Zero Auth **não retorna ou analisa** os seguintes itens:

1. Limite de crédito do cartão
2. Informações sobre o portador
3. Não aciona a base bancaria (dispara SMS so portador)


### Integração

Para realizar a consulta ao Zero Auth, o lojista deverá enviar uma requisição `POST` para a API Cielo Ecommerce, simulando uma transação. O `POST` deverá ser realizado nas seguintes URL: 

> Sandbox: https://`apisandbox`.cieloecommerce.cielo.com.br/1/`zeroauth`

> Produção: https://`api`.cieloecommerce.cielo.com.br/1/`zeroauth`

Cada tipo de validação necessita de um contrato tecnico diferente. Eles resultarão em _responses diferenciados_.

Abaixo, a listagem de campos da Requisição:

|Paramêtro|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|CardNumber|Número do Cartão do Comprador|Texto|16|sim|
|Holder|Nome do Comprador impresso no cartão.|Texto|25|não|
|ExpirationDate|Data de e validade impresso no cartão.|Texto|7|sim|
|SecurityCode|Código de segurança impresso no verso do cartão.|Texto|4|não|
|SaveCard|Booleano que identifica se o cartão será salvo para gerar o CardToken.|Boolean|---|Não|
|Brand|Bandeira do cartão: <br>Visa<br>Master<br>Amex<br>Elo<br>Aura<br>JCB<br>Diners<br>Discover|Texto|10|não|
|CardToken|Token do cartão na 3.0|GUID|36|Condicional|



Sobre o Avs, nó contendo os dados do endereço de cobrança do portador do cartão

|Paramêtro|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|Avs.Cpf|CPF do portador|texto|11|Não|
|Avs.ZipCode|CEP do endereço de cobrança do portador|texto|8|Não|
|Avs.Street|Logradouro do endereço de cobrança do portador|texto|50|Não|
|Avs.Number|Número do endereço de cobrança do portador|texto|6|Não|
|Avs.Complement|Complemento do endereço de cobrança do portador|texto|30|Não|
|Avs.District|Bairro do endereço de cobrança do portador|texto|20|Não|


## Requisição


Conteudo do **POST - PADRÃO**
```
{
    "CardNumber":"1234123412341231",
    "Holder":"Alexsander Rosa",
    "ExpirationDate":"12/2021",
    "SecurityCode":"123",
    "SaveCard":"false",
    "Brand":"Visa"
}
```

Conteudo do **POST - COM TOKEN**
```
{
  "CardToken":"23712c39-bb08-4030-86b3-490a223a8cc9",
    "SaveCard":"false",
    "Brand":"Visa"
}
```

Conteudo do **POST - COM AVS**
```
{
    "CardNumber":"1234123412341231",
    "Holder":"Teste Holder",
    "ExpirationDate":"12/2021",
    "SecurityCode":"123",
    "SaveCard":"false",
    "Brand":"Visa",
    "Avs":{
        "Cpf": "12387719719",
        "ZipCode": "20241180",
        "Street": "Rua da Glória",
        "Number": "214",
        "Complement": "ap 203",
        "District": "Rio de Janeiro"
    }
}
```
## Resposta

A resposta sempre retorna se o cartão pode ser autorizado no momento. Essa informação apenas significa que o _cartão está valido a transacionar_, mas não necessariamente indica que um determinado valor será autorizado.

Abaixo os campos retornados após a validação:


|Paramêtro|Descrição|Tipo|Tamanho|
|---|---|---|---|
|Valid|Situação do cartão:<br> **True** – Cartão válido<BR>**False** – Cartão Inválido|Boolean|---|
|ReturnCode|Código de retorno|texto|2|
|ReturnMessage|Mensagem de retorno|texto|255|



Response: **POSITIVA - Cartão Válido**

```
{
        "Valid": true,
        "ReturnCode": “00”,
        "ReturnMessage", “Transacao autorizada”
}
```
Response: **NEGATIVA - Cartão Inválido**

```
{
       "Valid": false,
       "ReturnCode": "57",
       "ReturnMessage": "Autorizacao negada"
}
```

Response: **NEGATIVA - Cartão com bandeira inválida**

```
  {    
      "Code": 57,     
      "Message": "Bandeira inválida"   
  }
```








> _Consulte <https://developercielo.github.io/Webservice-3.0/#códigos-de-retorno-das-vendas> para visualizar a descrição dos códigos de retorno_



