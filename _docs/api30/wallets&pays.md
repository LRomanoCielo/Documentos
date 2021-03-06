---
title: Wallets
category: API CIELO ECOMMERCE
order: 8
---


# Wallet/Carteiras

## O que são Wallets / Carteiras

São repositorios de cartões e dados de pagamentos para consumidores online. As Carteiras digitais permitem que um consumidor realizar o cadastro de seus dados de pagamento, assim agilizando o processo de compra em lojas habilitadas em suas compras por possuir apenas um cadastro.

> *Para utilizar carteiras na API Cielo eCommerce, o lojista deverá possuir as carteiras integradas em seu checkout*. 

Para maiores informações, sugerimos que entre em contato com o setor tecnico da carteira a qual deseja implementar.

## Wallets / Carteiras Disponiveis

A API Cielo Ecommerce possui integração com:

| Carteira                                                           | 
|--------------------------------------------------------------------|
| [*Apple Pay*](https://www.apple.com/br/apple-pay/)                 |
| [*Samsung Pay*](https://www.samsung.com.br/samsungpay/)            |
| [*Android Pay*](https://www.android.com/intl/pt-BR_br/pay/)        | 
| [*VisaCheckout*](https://vaidevisa.visa.com.br/site/visa-checkout) | 
| [*MasterPass*](https://masterpass.com/pt-br/)                      | 

<aside class="notice"><strong>Atenção:</strong> Quando o nó “Wallet” for enviado na requisição, o nó “CreditCard” passa a ser opcional.</aside>

<aside class="notice"><strong>Atenção:</strong> Para o cartão de débito, quando for enviado na requisição o nó “Wallet”, será necessário o nó “DebitCard” contendo a “ReturnUrl”.</aside>

## Integração

### Requisição Padrão

```json
{
  "MerchantOrderId": "2014111708",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 100,
    "Installments": 1,
    "Currency": "BRL",
    "Wallet": {
      "Type": "TIPO DE WALLET",
      "WalletKey": "IDENTIFICADOR DA LOJA NA WALLET",
      "AdditionalData": {
        "EphemeralPublicKey": "TOKEN INFORMADO PELA WALLET"
      }
    }
  }
}

```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`           |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    |                                                      

#### Walletkey

Formato de `WalletKeys` utilizados pelas Wallets na API Cielo

| Carteira       | Exemplo        |
|----------------|----------------|
| *Apple Pay*    | `9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc`   |
| *Samsung Pay*  | `eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ`   |
| *Android Pay*  | `En6NrAzy/V9l7U9FukJTlOkXLx8lsHJ9Lp3aO16WbZuXX+dHI6fy3G8PES5Leu63x+ZSAKQTQxJC/+hFCU3N3Vzl2Eo0bhQDv9pZwU9oghS1rx0QlqNPslspQ8ufTqfDDg2IlsVh7ANh8BXC1pi4YzhUXhAB5J2xSu7ivJwWL4/9FVJ2u0mZIK6QHtuGJXdvol54HtNn9Cik9IA7xeh/gutJ/z9K8rVk1KrCBSSTE1cgxbUbpBp535IXsP//okqXIf7qlTi/sUkfIjXVWLPVP/JPcm+MHE0s/37aVyprz0xfkJjHkUExsJ2h1v7LU5nnK7uYmyUdEI42wUuzfGkwvjvfS3xllxGwqy3Of4ts5mkPLNXdLnPNGvP8RE4uYyrQM4hXdie3tTPgW6FK5ExDsKyw8Qm0ikvbb4Clo9JvALTjqJkr1VkT20UITdetra/8JUWOX/PbcxDYv2oOPLG9hDumRExuifi04cNtwzFvyFVBnV1SO3GKp+k/gGNDs7EziHNjToEq7JOWoaAlhNEUu2nxQxNiDEaRckgYd5A=`   |
| *VisaCheckout* | `1140814777695873901`   |

> A Wallet MasterPass não possui WalletKey

#### EphemeralPublicKey

Formato de `EphemeralPublicKey` utilizados pelas Wallets na API Cielo

| Carteira       | Exemplo                                                                                                                          |
|----------------|----------------------------------------------------------------------------------------------------------------------------------|
| *Apple Pay*    | `MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ==`   |
| *Android Pay*  | `BG9mGFe2/kSo6PJDEoO5bRXRS4RKQ4b3jikXio0FUhZPqQe5f6SMlZQI3sfyiKteM0PRSeNDnQQ10XYeobN9avM=`                                       |

### Respostas

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "TIPO DE WALLET",
            "WalletKey": "IDENTIFICADOR DA LOJA NA WALLET",
            "Eci": 0
            "AdditionalData": {
                "EphemeralPublicKey": "TOKEN INFORMADO PELA WALLET"
                              },                
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`                         | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

## Exemplos

### Apple Pay

#### Requisição

Exemplo de Requisição padrão *Apple Pay*

> É necessário que a loja ja possua cadastro e uma integração Apple Pay, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId": "6242-642-723",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 1100,
    "Provider": "Cielo",
    "Installments": 1,
    "Currency": "BRL",
    "Wallet": {
      "Type": "ApplePay",
      "WalletKey": "9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc",
      "AdditionalData": {
        "EphemeralPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ=="
      }
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`           |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "ApplePay",
            "WalletKey": "9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc",
            "Eci": 0
            "AdditionalData": {
                "EphemeralPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ=="
                              },                
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`                         | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

### Samsung Pay

#### Requisição

Exemplo de Requisição padrão *Samsung Pay*

> É necessário que a loja ja possua cadastro e uma integração Samsung Pay, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId":"6242-642-723",
  "Customer":{
     "Name":"Exemplo Wallet Padrão",
     "Identity":"11225468954",
      "IdentityType":"CPF"
  },
  "Payment":{
     "Type":"CreditCard",
     "Amount":1,
     "Provider":"Cielo",
     "Installments":1,
     "Currency":"BRL",
     "Wallet":{
       "Type":"SamsungPay",
       "WalletKey":"eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ"
    }
  }
}

```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`           |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "SamsungPay",
            "WalletKey": "eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ",
            "Eci": 0
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`                         | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

### Android Pay

#### Requisição

Exemplo de Requisição padrão *Android Pay*

> É necessário que a loja ja possua cadastro e uma integração Android Pay, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId":"6242-642-723",
  "Customer":{
     "Name":"Exemplo Wallet Padrão",
     "Identity":"11225468954",
      "IdentityType":"CPF"
  },
  "Payment":{
     "Type":"CreditCard",
     "Amount":1,
     "Provider":"Cielo",
     "Installments":1,
     "Currency":"BRL",
     "Wallet":{
       "Type":"AndroidPay",
       "WalletKey":"En6NrAzy/V9l7U9FukJTlOkXLx8lsHJ9Lp3aO16WbZuXX+dHI6fy3G8PES5Leu63x+ZSAKQTQxJC/+hFCU3N3Vzl2Eo0bhQDv9pZwU9oghS1rx0QlqNPslspQ8ufTqfDDg2IlsVh7ANh8BXC1pi4YzhUXhAB5J2xSu7ivJwWL4/9FVJ2u0mZIK6QHtuGJXdvol54HtNn9Cik9IA7xeh/gutJ/z9K8rVk1KrCBSSTE1cgxbUbpBp535IXsP//okqXIf7qlTi/sUkfIjXVWLPVP/JPcm+MHE0s/37aVyprz0xfkJjHkUExsJ2h1v7LU5nnK7uYmyUdEI42wUuzfGkwvjvfS3xllxGwqy3Of4ts5mkPLNXdLnPNGvP8RE4uYyrQM4hXdie3tTPgW6FK5ExDsKyw8Qm0ikvbb4Clo9JvALTjqJkr1VkT20UITdetra/8JUWOX/PbcxDYv2oOPLG9hDumRExuifi04cNtwzFvyFVBnV1SO3GKp+k/gGNDs7EziHNjToEq7JOWoaAlhNEUu2nxQxNiDEaRckgYd5A=",
       "AdditionalData":{
           "EphemeralPublicKey":"BG9mGFe2/kSo6PJDEoO5bRXRS4RKQ4b3jikXio0FUhZPqQe5f6SMlZQI3sfyiKteM0PRSeNDnQQ10XYeobN9avM="
       }
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`           |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "AndroidPay",
            "WalletKey": "En6NrAzy/V9l7U9FukJTlOkXLx8lsHJ9Lp3aO16WbZuXX+dHI6fy3G8PES5Leu63x+ZSAKQTQxJC/+hFCU3N3Vzl2Eo0bhQDv9pZwU9oghS1rx0QlqNPslspQ8ufTqfDDg2IlsVh7ANh8BXC1pi4YzhUXhAB5J2xSu7ivJwWL4/9FVJ2u0mZIK6QHtuGJXdvol54HtNn9Cik9IA7xeh/gutJ/z9K8rVk1KrCBSSTE1cgxbUbpBp535IXsP//okqXIf7qlTi/sUkfIjXVWLPVP/JPcm+MHE0s/37aVyprz0xfkJjHkUExsJ2h1v7LU5nnK7uYmyUdEI42wUuzfGkwvjvfS3xllxGwqy3Of4ts5mkPLNXdLnPNGvP8RE4uYyrQM4hXdie3tTPgW6FK5ExDsKyw8Qm0ikvbb4Clo9JvALTjqJkr1VkT20UITdetra/8JUWOX/PbcxDYv2oOPLG9hDumRExuifi04cNtwzFvyFVBnV1SO3GKp+k/gGNDs7EziHNjToEq7JOWoaAlhNEUu2nxQxNiDEaRckgYd5A",
            "Eci": 0
            "AdditionalData":{
                "EphemeralPublicKey":"BG9mGFe2/kSo6PJDEoO5bRXRS4RKQ4b3jikXio0FUhZPqQe5f6SMlZQI3sfyiKteM0PRSeNDnQQ10XYeobN9avM="
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `AndroidPay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`/ `AndroidPay`                         | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

### MasterPass

Para utilizar o Masterpass é necessário entrar em contato diretamente com a Mastercard pelo site: https://masterpass.com/pt-br/ e solicitar as credenciais.
Mais informações e integração completa, você encontra no link: https://developer.mastercard.com/product/masterpass “ 

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111708",
   "Customer":{  
      "Name":"Comprador MasterPass"     
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":15700,
     "Installments":1,
     "CreditCard":{
               "CardNumber": "4532117080573703",
               "Brand": "Visa",
         "SecurityCode":"023"
     },
     "Wallet":{
         "Type":"MasterPass",
         "AdditionalData":{
               "CaptureCode": "103"
         }
     }
   }
}

```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`CreditCard.CardNumber.`|Texto|19|Sim|Número do Cartão do Comprador|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`Wallet.Type`|Texto|255|Sim|indica qual o tipo de carteira: "VisaCheckout" ou "Masterpass"|
|`Wallet.AdditionalData`|---|---|---|Instancia para dados extras informados pela MasterPass. Obrigatório apenas se TYPE = "MasterPass"|
|`Wallet.capturecode`|Texto|255|Sim|Código informado pela MasterPass ao lojista|

#### Resposta

```json
{
  "MerchantOrderId": "2014111708",
  "Customer": {
    "Name": "comprador Masterpass"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "CardNumber": "453211******3703",
      "Holder": "Teste Holder",
      "ExpirationDate": "12/2016",
      "SaveCard": false,
      "Brand": "Visa"
    },
    "Tid": "0915052536103",
    "Provider": "Simulado",
    "Wallet": {
      "Type": "Masterpass",
      "Eci": 0,
      "AdditionalData": {
        "CaptureCode": "103"
      }
    },
    "PaymentId": "689da793-fc99-4900-89f1-9e7fdaa06ef8",
    "Type": "CreditCard",
    "Amount": 15700,
    "ReceivedDate": "2016-09-15 17:25:35",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "57",
    "ReturnMessage": "Card Expired",
    "Status": 3,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/689da793-fc99-4900-89f1-9e7fdaa06ef8"
      }
    ]
  }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|
|`Type`|indica qual o tipo de carteira: "VisaCheckout" ou "Masterpass"|Texto|255|Sim|
|`Capturecode`|Código informado pela MasterPass ao lojista|Texto|255|Sim|

### VisaCheckout

É possivel realizar uma transação com VisaCheckout de duas maneiras:

1. Sem enviar dados do cartão
2. Enviando dados do cartão

<aside class="notice"><strong>Atenção:</strong>  Para VisaCheckout, o nó Wallet pode ser enviado apenas com o "Type", assim marcado a transação com sendo da carteira. Nesse contexto, o cartão de crédito deve ser enviado. </aside>

#### Sem dados do Cartão

**Requisição** 

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111703",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "CreditCard":{  
         "SecurityCode":"123"
      },
      "Wallet":{  
         "Type":"VisaCheckout",
         "WalletKey":"1140814777695873901"
      }
   }
}

```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.ReturnUrl`|Texto|1024|---|Obrigatório para cartão de débito|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`Wallet.Type`|Texto|255|Sim|indica qual o tipo de carteira: "VisaCheckout" ou "Masterpass"|
|`Wallet.Walletkey`|Texto|255|---|Chave criptografica enviada pelo VisaCheckout. Obrigatoria se TYPE =  "Visa Checkout"|

**Resposta**

```json
{
  "MerchantOrderId": "2014111708",
  "Customer": {
    "Name": "comprador VisaCheckout"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "SaveCard": false,
      "Brand": "Undefined"
    },
    "Tid": "0915052340115",
    "Provider": "Simulado",
    "Wallet": {
      "Type": "VisaCheckout",
      "WalletKey": "1140814777695873901",
      "Eci": 0
    },
    "PaymentId": "efdb3338-9c8f-445a-8836-2cc93d8beacf",
    "Type": "CreditCard",
    "Amount": 15700,
    "ReceivedDate": "2016-09-15 17:23:39",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "77",
    "ReturnMessage": "Card Canceled",
    "Status": 3,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/efdb3338-9c8f-445a-8836-2cc93d8beacf"
      }
    ]
  }
}
```

#### Com dados do Cartão

**Requisição**

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111703",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "Wallet":{  
         "Type":"VisaCheckout"
    },
      "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "Brand":"Visa"
    },
  },
}
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.ReturnUrl`|Texto|1024|---|Obrigatório para cartão de débito|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Não|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard).|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`Wallet.Type`|Texto|255|Sim|indica qual o tipo de carteira: "VisaCheckout" ou "Masterpass"|

**Resposta**

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Visa Checkout"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "Tid": "0305023644309",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|
|`Type`|indica qual o tipo de carteira: "VisaCheckout" ou "Masterpass"|Texto|255|Sim|
