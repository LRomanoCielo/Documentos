---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 4
---
### O que é o Split de pagamentos ?

O Split de pagamentos é uma funcionalidade Braspag que permite a divisão de uma transações em diferentes lojas de maneira organizadada e hierarquizada.

O Split é uma estrutura transacional muito utilizada em MarketPlaces onde o carrinho é formado por produtos de diferentes fornecedores que receberarão partes do pagamento em contas separadas.


### Como funciona o Split de Pagamentos.


O Split funciona como parte da integração transacional da Braspag.

Nesse modelo de integração existem  3 entidades:

| Entidade | Descrição |
|----------|-----------|
| Marketplace | Dono do carrinho e da Transação. <br> Possui Sellers que fornecem o contudo do Carrinho.<br> Realiza a cobrança de uma Taxa sobre a venda do Seller <br>|
| Seller | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<br> Um Marketplace possui inumeros Seller. <br> Recebem parte da venda, descontado o valor da taxa do MarketPlace |
| Braspag | Repson|