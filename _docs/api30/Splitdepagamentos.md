---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 4
---
### O que é o Split de pagamentos ?

Lorem ipsum dolor sit amet, id interdum eget, amet lectus commodo malesuada etiam turpis orci, est malesuada placerat neque nascetur, cras adipiscing vel nulla. Quam sapien donec quod quisque auctor venenatis, a scelerisque amet maecenas pulvinar, vitae massa eu vitae eu sed. Nec proin vestibulum et metus aliquet quis. Est magna ac. Et posuere quam erat ipsum, est quae, sollicitudin vivamus, ligula sit. Euismod per wisi, platea elit suspendisse lobortis interdum ante, faucibus sed. Et in tincidunt, risus maecenas neque. Porta dolor lacinia imperdiet amet nullam, inceptos luctus eu taciti consequat vestibulum, mollis nihil a. Vehicula nunc conubia metus egestas pellentesque at, in purus, nunc id tristique, dolor sem rhoncus a.

Suscipit tincidunt id feugiat ut lorem ea, urna molestie ipsum placerat conubia mauris convallis, hymenaeos nullam. Auctor sodales sed faucibus ultricies egestas, integer lorem vel ut in vitae. Bibendum leo quam. Quo lacus scelerisque amet dis, elit enim dictum mattis nonummy. Id nec ac phasellus pede sem, risus in pede placerat, ante wisi. Et vel magna bibendum ultricies, augue hac, sapien nulla morbi ante suscipit ipsum vestibulum. Libero diam, a aliquid nunc pede duis suspendisse pede, ultrices elit suscipit nullam, vel ligula. Faucibus lacus vestibulum. Sollicitudin ac in.

Ullamcorper augue urna sem arcu vestibulum. Mauris integer. Magna ut. Pretium volutpat condimentum tempor. Purus nulla sed at sit, ac dis. Phasellus congue lacinia a faucibus orci. Auctor urna, tortor blandit vivamus nibh quisque, sed magna netus mollis et, mauris dapibus nulla.
Senectus adipiscing donec natoque, tortor tellus sagittis, erat quis id posuere sed curae, est lorem quam ornare magna montes, imperdiet quis tellus eu arcu odio a. Varius pellentesque quis, fringilla neque vehicula velit semper, lacus sit, at leo fusce. Felis luctus minima pulvinar porta a, suscipit sollicitudin sed ut adipiscing nunc in. Id vel cursus potenti facilisis tempus eget, tincidunt est, sodales lacus erat quam eget aliquam penatibus, varius pharetra ipsum pede nulla. Orci vel tempor risus nostra elit rhoncus, elit mauris hendrerit eget aliquam, lorem est tincidunt massa maxime ante massa. Tellus ea aliquam sed orci scelerisque, eget suscipit vel a tristique elit, praesent tempus quam, interdum erat pellentesque sit pellentesque id. Quis odio sit ut lacinia eu, aliquam amet felis, suspendisse volutpat diam elit, habitasse orci metus et quis nulla erat. Convallis risus vestibulum venenatis ab neque odio, dignissim minus nulla vel nec, venenatis metus, mauris et. Nunc ad faucibus, taciti sodales tincidunt mauris, dolor sed leo, magna egestas ac vehicula cursus.


### Caso de uso


Lorem ipsum dolor sit amet, id interdum eget, amet lectus commodo malesuada etiam turpis orci, est malesuada placerat neque nascetur, cras adipiscing vel nulla. Quam sapien donec quod quisque auctor venenatis, a scelerisque amet maecenas pulvinar, vitae massa eu vitae eu sed. Nec proin vestibulum et metus aliquet quis. Est magna ac. Et posuere quam erat ipsum, est quae, sollicitudin vivamus, ligula sit. Euismod per wisi, platea elit suspendisse lobortis interdum ante, faucibus sed. Et in tincidunt, risus maecenas neque. Porta dolor lacinia imperdiet amet nullam, inceptos luctus eu taciti consequat vestibulum, mollis nihil a. Vehicula nunc conubia metus egestas pellentesque at, in purus, nunc id tristique, dolor sem rhoncus a.


### Integração

Lorem ipsum dolor sit amet, id interdum eget, amet lectus commodo malesuada etiam turpis orci, est malesuada placerat neque nascetur, cras adipiscing vel nulla. Quam sapien donec quod quisque auctor venenatis, a scelerisque amet maecenas pulvinar, vitae massa eu vitae eu sed. Nec proin vestibulum et metus aliquet quis. Est magna ac. Et posuere quam erat ipsum, est quae, sollicitudin vivamus, ligula sit. Euismod per wisi, platea elit suspendisse lobortis interdum ante, faucibus sed. Et in tincidunt, risus maecenas neque. Porta dolor lacinia imperdiet amet nullam, inceptos luctus eu taciti consequat vestibulum, mollis nihil a. Vehicula nunc conubia metus egestas pellentesque at, in purus, nunc id tristique, dolor sem rhoncus a.


> POST: www.lerom.com.br


Abaixo, a listagem de campos da Requisição:

| Paramêtro      | Descrição                                                                                                             | Tipo    | Tamanho | Obrigatório |
|----------------|-----------------------------------------------------------------------------------------------------------------------|---------|---------|:-----------:|
| CardType       | Define o tipo de cartão utilizados:<br><br>*CreditCard*<br>*DebitCard*<br><br>Se não enviado, CreditCard como default | Texto   | 255     | Não         |
| CardNumber     | Número do Cartão do Comprador                                                                                         | Texto   | 16      | sim         |
| Holder         | Nome do Comprador impresso no cartão.                                                                                 | Texto   | 25      | não         |
| ExpirationDate | Data de e validade impresso no cartão.                                                                                | Texto   | 7       | sim         |
| SecurityCode   | Código de segurança impresso no verso do cartão.                                                                      | Texto   | 4       | não         |
| SaveCard       | Booleano que identifica se o cartão será salvo para gerar o CardToken.                                                | Boolean | ---     | Não         |
| Brand          | Bandeira do cartão: <br><br>Visa<br>Master<br>| Texto   | 10      | não         |
| CardToken      | Token do cartão na 3.0                                                                                                | GUID    | 36      | Condicional |


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


| Paramêtro     | Descrição                                                                       | Tipo    | Tamanho |
|---------------|---------------------------------------------------------------------------------|---------|:-------:|
| Valid         | Situação do cartão:<br> **True** – Cartão válido<BR>**False** – Cartão Inválido | Boolean | ---     |
| ReturnCode    | Código de retorno                                                               | texto   | 2       |
| ReturnMessage | Mensagem de retorno                                                             | texto   | 255     |



## Histórico de Atualizações:

| Versão | Data       | Descrição                                                                                                      |
|:------:|------------|----------------------------------------------------------------------------------------------------------------|
| 1.0    | 14/08/2017 | ✓ Versão inicial                                                                                               |






