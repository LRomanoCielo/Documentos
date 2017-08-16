---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 4
---


### O que &eacute; o Split de pagamentos ?

O Split de pagamentos &eacute; uma funcionalidade Braspag que permite a divis&atilde;o de uma transa&ccedil;&otilde;es em diferentes lojas de maneira organizadada e hierarquizada.

O Split &eacute; uma estrutura transacional muito utilizada em MarketPlaces onde o carrinho &eacute; formado por produtos de diferentes fornecedores que receberar&atilde;o partes do pagamento em contas separadas.

### Como funciona o Split de Pagamentos.

O Split funciona como parte da integra&ccedil;&atilde;o transacional da Braspag.

Nesse modelo de integra&ccedil;&atilde;o existem 3 entidades:

| Entidade    | Descrição                                                                                                                                                                                |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Marketplace | Dono do carrinho e da Transação. <br> Possui Sellers que fornecem o contudo do Carrinho.<br> Realiza a cobrança de uma Taxa sobre a venda do Seller <br>                                 |
| Seller      | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<br> Um Marketplace possui inumeros Seller. <br> Recebem parte da venda, descontado o valor da taxa do MarketPlace   |
| Braspag     | Responsavel pela auytorização da transação <br> Realiza a cobrança da Taxa definida pelo Marketplace, retirando esse valor da transação <br> Deposita o valor da Transação na conta do Seller <br> Deposita o valor da taxa cobrada pelo Marketplace so Seller |

O Fluxo transacional de autoriza&ccedil;&atilde;o e retirada de ocorre como na imagem abaixo:

![](/uploads/versions/Split---x----1398-720x---.png)

Descrevendo os Passos: 

1. O Marketplace cria o carrinho contendo dados sobre Sellers e a Taxa que deseja Cobrar
2. A Taxa do Marketplace é na verdade constituidade do Custo da operação e a margem que o Marketplace deseja obter.

> Na integração braspag, é possivel que dentro de um carrinho, o MarketPlace possa cobrar taxas diferentes dependendo o Seller.

3. A Braspag identifica qual o va