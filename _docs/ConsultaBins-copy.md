---
title: Consulta Bins
category: API CIELO ECOMMERCE
order: 1
---


### **O que é “CONSULTA BINS”**

A “**Consulta de Bins**”  é um serviço de **pesquisa de dados do cartão**, de crédito ou débito, que os lojistas da API Cielo Ecommerce podem utilizar para validar se os dados preenchidos em tela sobre o cartão são válidos.

A “**consulta de Bins”** pe capaz retornar os seguintes dados sobre o cartão:

* **Bandeira do cartão**
* **Tipo de cartão:**Crédito, Débito ou Múltiplo (Crédito e Débito)
* **Nacionalidade do cartão:** estrangeiro ou nacional

Para mais informações sobre a API Cielo Ecommerce (Sandbox e Produção), acesse: <https://developercielo.github.io/Webservice-3.0/>


### Integração


Basta realizar um `GET` enviado o BIN a nossa URL de consulta:

> https://`apiquerysandbox`.cieloecommerce.cielo.com.br/1/cardBin/`BIN`



### Requisição
**Consulta**:

```
https://apiquerysandbox.cieloecommerce.cielo.com.br/1/cardBin/420020
```

### Resposta
**Response**

```
{
"Status": "00",
"Provider": "Visa",
"CardType": "Credit",
"ForeignCard": "true"
}
```

| Paramêtro       | Tipo  | Tamanho | Descrição                                                                                                                                                                                  |
|-----------------|-------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Status**      | Texto | 2       | Status da requisição de análise de Bins: <br><br> 00 – Analise autorizada <br> 01 – Bandeira não suportada <br> 02 – Cartão não suportado na consulta de bin <br> 73 – Afiliação bloqueada |
| **Provider**    | Texto | 255     | Bandeira do cartão                                                                                                                                                                         |
| **CardType**    | Texto | 20      | Tipo do cartão em uso : <br><br> Credito <br> Debito <br>Multiplo                                                                                                                          |
| **ForeingCard** | Texto | 255     | Se o cartão é emitido no exterior (False/True)                                                                                                                                             |


### Histórico de atualizações

| Versão | Data       | Descrição                           |
|--------|------------|-------------------------------------|
| 1.0    | 19/09/2016 | Versão inicial                      |
| 1.1    | 14/02/2017 | Alteração do campo Brand e Provider |









