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


O Zero Auth suporta as seguintes bandeiras:

* Visa
* MasterCard


### Integração

Para realizar a consulta ao Zero Auth, o lojista deverá enviar uma requisição `POST` para a API Cielo Ecommerce, simulando uma transação. O `POST` deverá ser realizado nas seguintes URL: 

> Sandbox: https://`apisandbox`.cieloecommerce.cielo.com.br/1/`zeroauth`

> Produção: https://`api`.cieloecommerce.cielo.com.br/1/`zeroauth`

Cada tipo de validação necessita de um contrato tecnico diferente. Eles resultarão em _responses diferenciados_.

Abaixo, a listagem de campos da Requisição:

|Paramêtro|Descrição|Tipo|Tamanho|Obrigatório|
|---------|---------|----|-------|-----------|
|CardType|Define o tipo de cartão utilizados:<br><br>*CreditCard*<br>*DebitCard*<br><br>Se não enviado, CreditCard como default|Texto|255|Não|
|CardNumber|Número do Cartão do Comprador|Texto|16|sim|
|Holder|Nome do Comprador impresso no cartão.|Texto|25|não|
|ExpirationDate|Data de e validade impresso no cartão.|Texto|7|sim|
|SecurityCode|Código de segurança impresso no verso do cartão.|Texto|4|não|
|SaveCard|Booleano que identifica se o cartão será salvo para gerar o CardToken.|Boolean|---|Não|
|Brand|Bandeira do cartão: <br><br>Visa<br>Master<br>Amex<br>Elo<br>Aura<br>JCB<br>Diners<br>Discover<br>|Texto|10|não|
|CardToken|Token do cartão na 3.0|GUID|36|Condicional|



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

Caso ocorra algum erro no fluxo, onde não seja possível validar o cartão, o serviço irá retornar erro: 
* 500 – Internal Server Erro


> Consulte <https://developercielo.github.io/Webservice-3.0/#códigos-de-retorno-das-vendas> para visualizar a descrição dos códigos de retorno. 
> O código de retorno **85 representa sucesso no Zero Auth**, os demais códigos são definidos de acordo com a documentação acima.


## Zero Auth com AVS

O **AVS** é um serviço para transações online onde é realizada uma validação cadastral através do batimento dos dados do endereço informado pelo comprador (endereço de entrega da fatura) na loja virtual, com os dados cadastrais do banco emissor do cartão.
Isso auxilia na redução do risco de chargeback. Deve ser utilizada para análise de vendas, auxiliando na decisão de captura da transação.

**Regras do AVS**

* Disponível apenas para as bandeiras Visa, Mastercard e AmEx.
* Produtos permitidos: somente crédito.
* O retorno da consulta ao AVS é separado em dois itens: CEP e endereço.
* Cada um deles pode ter os seguintes valores: 

|Valor|Descrição|
|---|-------|
|C|Confere|
|N|Não confere|
|I|Indisponível|
|T|Temporariamente indisponível|
|X|Serviço não suportado para esta Bandeira|
|E|Dados enviados incorretos. Verificar se todos os campos foram enviados|


* É necessário que todos os campos contidos no nó AVS sejam preenchidos para que a analise seja realizada.
* Quando o campo não for aplicável (exemplo: complemento), deve ser enviada preenchia com NULL ou N/A
* Necessário habilitar a opção do AVS no cadastro. Para habilitar a opção AVS no cadastro ou consultar os bancos participantes, entre em contato com o Suporte Cielo eCommerce


|Paramêtro|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|Avs.Cpf|CPF do portador|texto|11|Não|
|Avs.ZipCode|CEP do endereço de cobrança do portador|texto|8|Não|
|Avs.Street|Logradouro do endereço de cobrança do portador|texto|50|Não|
|Avs.Number|Número do endereço de cobrança do portador|texto|6|Não|
|Avs.Complement|Complemento do endereço de cobrança do portador|texto|30|Não|
|Avs.District|Bairro do endereço de cobrança do portador|texto|20|Não|

Conteudo do **POST - COM AVS**
```
{
    "CardType": "CreditCard",
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

Response: **POSITIVA - Com AVS**

```
{
       "Valid": true,
       "ReturnCode": "85",
       "ReturnMessage": "Transacao autorizada",
       "AvsCepReturnCode": "I",
       "AvsAddressReturnCode": "I"
}
```



## Histórico de Atualizações:

|Versão|Data |Descrição    |
|------|-----|-------------|
|1.0|16/09/2016|✓ Versão inicial| 
|1.1|25/10/2016|✓ Atualização de parâmetros |
|1.2|05/01/2017|✓ Inclusão da tabela de parâmetros obrigatórios <br> ✓ Inclusão do contrato sem AVS <br>✓ Inclusão do contrato com Cardtoken  |
|1.3|15/05/2017|✓ Complemento da tabela com os campos do contrato de requisição <br> ✓ Alteração da tabela com os campos do contrato de resposta |
|1.4|31/05/2017|✓ Adicionado o tópico de bandeiras suportadas <br> ✓ Indicação que tokens 1.5 não são suportados <br>✓ Resposta para cartões com bandeira não suportada <br>✓ Resposta para requisições que resultem em erro e consequentemente na não validação do cartão.<br> ✓ Atualização do tamanho dos campos do contrato de requisição|
|1.5|07/06/2017|✓ Adicionada URL de produção <br> ✓ Adicionado código de retorno 00 para consultas que retornem cartões válidos.|
