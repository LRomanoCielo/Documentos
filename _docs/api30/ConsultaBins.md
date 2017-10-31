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



### Caso de Uso

Este é um exemplo de como usar a consulta bin para identificar o melhor perfil de comprador e ajudar sua conversão.

O Consulta bin é uma ferramenta da Cielo que permite identificar:

* Status: Valido ou invalido
* Origem do cartão: emitido no Brasil ou estrangeiro.
* O tipo do Cartão: Crédito/Débito ou ambos.
* A bandeira do cartão: Todas suportadas pela Cielo

Essas informações permitem tomar ações no momento do checkout para melhorar a conversão da loja.

Veja um exemplo de uso: **Consulta Bins + recuperação de carrinho**

Um marketplace chamada Submergível possui uma gama de meios de pagamento disponíveis para que suas lojas possam oferecer ao comprador, mas mesmo com toda essa oferta, ela continua com uma taxa de conversão que 

Conhecendo a função de consulta Bins da API Cielo Ecommerce, como ela poderia evitar a perda de carrinhos?

Ela pode aplicar a Consulta bins e 3 cenários!

1. Impedir erros com tipo de cartão
2. Oferecer recuperação de carrinhos online
3. Alertar sobre cartões internacionais

Impedir erros com tipo de cartão

O Submergível pode usar a consulta bins no carrinho para identificar 2 dos principais erros no preenchimento de formulários de pagamento: 

* **Bandeira errada** e confundir cartão de crédito com débito ○ Bandeiras erradas: Ao preencher o formulário de pagamento, é possível realizar uma consulta e já definir a bandeira correta. Esse é um método muito mais seguro do que sebasear em algoritmos no formulário, pois a base de bins consultada é a da bandeira emissora do cartão.
* **Confusões com cartões:** Ao preencher o formulário de pagamento, é possível realizar uma consulta e avisar ao consumidor se ele está usando um cartão de  débito quando na verdade deveria usar um  de débito

**Oferecer recuperação de carrinhos online**
* O Submergível pode usar a consulta bins no carrinho para oferecer um novo meio de pagamento caso a transação falhe na primeira tentativa.
* Realizando uma consulta no momento de preenchimento do formulário de pagamento, caso o cartão seja múltiplo (Crédito e Débito), o Submergível pode reter os dados do cartão, e caso a transação de crédito falhe, ele pode oferecer automaticamente ao consumidor uma transação de débito com o mesmo cartão.

**Alertar sobre cartões internacionais**
O Submergível pode usar a consulta bins no carrinho para alertar compradores internacionais, que caso o cartão não esteja habilitado para transacionar no Brasil, a transação será negada


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

```http
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










