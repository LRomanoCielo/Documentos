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

| Entidades   | Descrição                                                                                                                                                                                |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Marketplace | Dono do carrinho e da Transação. <br> Possui Sellers que fornecem o contudo do Carrinho.<br> Realiza a cobrança de uma Taxa sobre a venda do Seller <br>                                 |
| Seller      | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<br> Um Marketplace possui inumeros Seller. <br> Recebem parte da venda, descontado o valor da taxa do MarketPlace   |
| Braspag     | Responsavel pela auytorização da transação <br> Realiza a cobrança da Taxa definida pelo Marketplace, retirando esse valor da transação <br> Deposita o valor da Transação na conta do Seller <br> Deposita o valor da taxa cobrada pelo Marketplace so Seller |

O Fluxo transacional de autoriza&ccedil;&atilde;o e retirada de ocorre como na imagem abaixo:

![](/uploads/versions/Split---x----1398-720x---.png)


Descrevendo os Passos: 

1. O Marketplace cria o carrinho contendo dados sobre Sellers e a Taxa que deseja Cobrar
2. A Taxa do Marketplace é na verdade constituidade do Custo da operação e a margem que o Marketplace deseja obter.
3. A Braspag identifica e realiza a partilha dos valores.
4. O Seller recebe o valor da transação, menos o valor da Taxa de MKP.
5. A Braspag retira sua taxa de operação do Valor contindo no MKP. Esse valor é calculado sobre o valor total da transação e não sobre a participação do MKP na venda
6. O Marketplace recebe o valor restante da participação gerada com a taxa de Marketplace


> Na integração braspag, é possivel que dentro de um carrinho, o MarketPlace possa cobrar taxas diferentes dependendo o Seller.



### Tarifas e Custos

Nesta área do manual vamos detalhar como o processo de cobrança e custos afeta cada uma das entidades envolvidas no Split

#### Custo Marketplace

O Split de pagamentos Braspag funciona com base em uma taxa basica tabelada contratada pelo Marketplace e uma tarifa fixa sobre a transação executada. 
A Taxa braspag é cobrada sobre o valor da transação

> *Custo MKP:* TAXA BRASPAG + R$0,30


O valor da tarifa fixa Braspag é retirada do montante destinado ao marketplace.

#### Custo Seller

O Custo total operacional para o Seller é baseado na Taxa Braspag a ser retirada da participação do marketplace. Ela é gerada com base no valor da transação e a margem do MKP

> *Custo Seller:*   Taxa MKP = Margem MKP + (TAXA BRASPAG + R$0,30)


