---
title: Controle Transacional
category: CHECKOUT CIELO
order: 3
---

### O QUE É A API DE CONTROLE TRANSACIONAL 

A **API de Controle Transacional** permite ao lojista realizar operações sobre os pedidos que antes eram possíveis somente através do Backoffice do Checkout Cielo. 


As operações possíveis de serem realizadas são: 
* **Consulta** – consultar uma transação
* **Captura** – capturar uma transação com valor total ou parcial.
* **Cancelamento** – cancelar uma transação com valor total ou parcial

Seu principal objetivo é permitir que lojas e plataformas possam automatizar as operações através de seus próprios sistemas. 

> Endereço: https://cieloecommerce.cielo.com.br/api/public/v2/orders/  


### Autorização de acesso

A API de controle transacional utiliza como forma de segurança o **protocolo OAUTH**.
Para acessá-la será necessário possuir: 

1. Clientd
2. ClientSecret

|PROPRIEDADE|DESCRIÇÃO|TIPO|
|ClientId|Mesmo que o **MerchantId**|guid|
|ClientSecret|Chave secreta que deverá ser obtida junto à Cielo|string|

Para obter acesso aos serviços da API de controle transacional, será necessário obter um token de acesso, conforme os passos abaixo:

1. Concatenar o _ClientId_ e o _ClientSecret_, **ClientId:ClientSecret**
2. Codificar o resultado em *Base64*
3. Enviar uma requisição, utilizando o método HTTP POST, para obter o token de acesso conforme abaixo:

|**POST**|https://cieloecommerce.cielo.com.br/api/public/v2/token|
|**Authorization**|Basic {Base64}|


**EXEMPLO**

* **ClientId** - MerchantId = b521b6b2-b9b4-4a30-881d-3b63dece0006
* **ClientSecret** - 08Qkje79NwWRx5BdgNJsIkBuITt5cIVO
* **ClientId:ClientSecret** - *b521b6b2-b9b4-4a30-881d-3b63dece0006:08Qkje79NwWRx5BdgNJsIkBuITt5cIVO*
* **Base64** - YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw


**REQUISIÇÃO**
```
"POST": https://cieloecommerce.cielo.com.br/v2/public/v2/token
"Headers"
"Authorization": Basic YjUyMWI2YjItYjliNC00YTMwLTg4MWQtM2I2M2RlY2UwMDA2OiAwOFFramU3OU53V1J4NUJkZ05Kc0lrQnVJVHQ1Y0lWTw
```

**RESPOSTA**
```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzc47I6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA",
    "token_type": "bearer",
    "expires_in": 1199
}
```
|PROPRIEDADE|DESCRIÇÃO|TIPO|
|access_token|Utilizado para acesso aos serviços da API|string|
|token_type|Sempre será do tipo “bearer”|texto|
|expires_in|Validade do token em segundos. Aproximadamente 20 minutos|int|

Como se pode observar, o token possui um tempo de expiração aproximado de 20 minutos (1.200 segundos). Portanto, quando o mesmo expirar, será necessário obter um novo token para acesso aos serviços. 



### Utilizando Serviços

Para utilizar os serviços da API de controle transacional, deve ser enviado em todas as requisições o token de acesso obtido na etapa de autorização.
O mesmo deve ser enviado no header da requisição, conforme abaixo:

```
Authorization: Bearer {access_token}
```

**Exemplo**


```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjbGllbnRfbmFtZSI6Ik1ldUNoZWNrb3V0IE1hc3RlciBLZXkiLCJjbGllbnRfaWQiOiJjODlmZGasdasdasdmUyLTRlNzctODA2YS02ZDc1Y2QzOTdkYWMiLCJzY29wZXMiOiJ7XCJTY29wZVwiOlwiQ2hlY2tvdXRBcGlcIixcIkNsYWltc1wiOltdfSIsInJvbGUiOiJasdasdasd291dEFwaSIsImlzcyI6Imh0dHBzOi8vYXV0aGhvbasdasdnJhc3BhZy5jb20uYnIiLCJhdWQiOiJVVlF4Y1VBMmNTSjFma1EzSVVFbk9pSTNkbTl0ZmasdsadQjVKVVV1UVdnPSIsImV4cCI6MTQ5Nzk5NjY3NywibmJmIjoxNDk3OTEwMjc3fQ.ozj4xnH9PA3dji-ARPSbI7Nakn9dw5I8w6myBRkF-uA
```





### Consultar uma transação

Permite consultar uma transação pelo número do pedido

> `GET` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{orderNumber}`

**Header:**

```
Authorization: Bearer {access_token}
```


**Resposta**
```
"HTTP Status": 200 – OK
```
```
{ 
    "merchantId": "c89fdfbb-dbe2-4e77-806a-6d75cd397dac", 
    "orderNumber": "054f5b509f7149d6aec3b4023a6a2957", 
    "softDescriptor": "Pedido1234", 
    "cart": { 
        "items": [ 
            { 
                "name": "Pedido ABC", 
                "description": "50 canetas - R$30,00 | 10 cadernos - R$50,00 | 10 Borrachas - R$10,00", 
                "unitPrice": 9000, 
                "quantity": 1, 
                "type": "1" 
            } 
        ] 
    }, 
    "shipping": { 
        "type": "FixedAmount", 
        "services": [ 
            { 
              "name": "Entrega Rápida", 
                "price": 2000 
            } 
        ], 
        "address": { 
            "street": "Estrada Caetano Monteiro", 
            "number": "391A", 
            "complement": "BL 10 AP 208", 
            "district": "Badu", 
            "city": "Niterói", 
            "state": "RJ" 
        } 
    }, 
    "payment": { 
        "status": "Paid", 
        "antifraud": { 
            "description": "Lojista optou não realizar a análise do antifraude." 
        } 
    }, 
    "customer": { 
        "identity": "12345678911", 
        "fullName": "Fulano da Silva", 
        "email": "exemplo@email.com.br", 
        "phone": "11123456789" 
    }, 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/054f5b509f7149d6aec3b4023a6a2957" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/054f5b509f7149d6aec3b4023a6a2957/void" 
        } 
    ] 
}
```

### Capturar uma transação

Permite capturar uma transação pelo número do pedido.

**Definição**

> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{orderNumber}`/capture 


**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**
```
HTTP Status: 200 – OK
```
```
{ 
    "success": true, 
    "status": 2, 
    "returnCode": "6", 
    "returnMessage": "Operation Successful", 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96/void" 
        } 
    ] 
} 
```



### Cancelar uma transação

Permite cancelar uma transação pelo número do pedido.

**Definição**

> `PUT` https://cieloecommerce.cielo.com.br/api/public/v2/orders/`{orderNumber}`/void  


**Header:**

```
Authorization: Bearer {access_token}
```

**Resposta**
```
HTTP Status: 200 – OK
```

```
{ 
    "success": true, 
    "status": 2, 
    "returnCode": "6", 
    "returnMessage": "Operation Successful", 
    "links": [ 
        { 
            "method": "GET", 
            "rel": "self", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96" 
        }, 
        { 
            "method": "PUT", 
            "rel": "void", 
            "href": "https://cieloecommerce.cielo.com.br/api/public/v2/orders/a9d517d81fb24b98b2d16eae2744be96/void" 
        } 
    ] 
} 

```
