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

| Entidade | Descri&ccedil;&atilde;o |
| --- | --- |
| Marketplace | Dono do carrinho e da Transa&ccedil;&atilde;o.
<br>Possui Sellers que fornecem o contudo do Carrinho.
<br>Realiza a cobran&ccedil;a de uma Taxa sobre a venda do Seller |
| Seller | Lojas do Marketplace que fornecem os produtos que formam o carrinho.
<br>Um Marketplace possui inumeros Seller.
<br>Recebem parte da venda, descontado o valor da taxa do MarketPlace |
| Braspag | Responsavel pela auytoriza&ccedil;&atilde;o da transa&ccedil;&atilde;o
<br>Realiza a cobran&ccedil;a da Taxa definida pelo Marketplace, retirando esse valor da transa&ccedil;&atilde;o
<br>Deposita o valor da Transa&ccedil;&atilde;o na conta do Seller
<br>Deposita o valor da taxa cobrada pelo Marketplace so Seller |

O Fluxo transacional de autoriza&ccedil;&atilde;o e retirada de ocorre como na imagem abaixo:

![](/uploads/versions/Split---x----1398-720x---.png)