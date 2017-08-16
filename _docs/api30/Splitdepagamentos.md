---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 4
---
### O que é o Split de pagamentos ?

O Split de pagamentos é uma funcionalidade Braspag que permite a divisão de uma transações em diferentes lojas de maneira organizadada e hierarquizada.

O Split é uma estrutura transacional muito utilizada em Market Places onde o carrinho é formado por produtos de diferentes fornecedores que receberarão partes do pagamento em contas separadas.


### Como funciona o Split de Pagamentos.


O Split funciona como parte da integração transacional da Braspag.

Nesse modelo de integração existem  3 entidades:

| Entidade | Descrição |
|----------|-----------|
| Market place | Dono do carrinho e da Transação.|
| Seller | Lojas do Marketplace que fornecem os produtos que formam o carrinho|
| Braspag | Repson|