---
title: Pré-cadastro
category: CHECKOUT CIELO
order: 2
---


### O QUE &Eacute; O PR&Eacute;-CADASTRO

A API de pr&eacute;-cadastro &eacute; projetada para simplificar o processo de cadastro de lojas originarias de plataformas ecommerce. Ela visa operacionalizar o processo de recebimento de informa&ccedil;&otilde;es e cria&ccedil;&atilde;o de novos cadastros no Checkout Cielo.

Este processo deve acontecer seguindo duas etapas:

1. Obten&ccedil;&atilde;o do “**accessToken**”
2. Envio de dados Cadastrais

O fluxo pode ser resumido conforme o diagrama:

![](/uploads/versions/Fluxo-Pre-cadastro---x----817-675x---.png)


### API da Autorização

Para iniciar o pré-cadastro de uma loja é necessário obter uma autorização (AccessToken) de acesso a API cadastral. Para se obter o AccessToken é obrigatório realizar um POST a API utilizando credenciais como as abaixo:


* **ClientId**: CB8713EXX55X4889
* **ClientSecret**: BA89FB6F1AXXXD4D874C93532D7C44A9639D092A004E4FB78CCF331385F

**OBS:** As Credenciais de produção serão fornecidas a plataformas registradas junto a equipe de PRODUTOS CIELO.

Um `POST` deve ser realizado a URL abaixo:

> `POST`: https://cieloecommerce.cielo.com.br/api/public/v1/`accesstoken`


**Requisição - AccessToken**
Conteuido do Header
```
  {
  "ClientId":"CB8713EXX55X4889",
  "ClientSecret":"BA89FB6F1AXXXD4D874C93532D7C44A9639D092A004E4FB78CCF331385F"
  }

```


**Resposta - - AccessToken**
```
{
    "accessToken": "ZDViNWI5ZDMtNTExZS00MDAyLTkxMTQtYTYwMWEzZDg4ZGJjMDY2MDgzMzUzNw==",
    "tokenType": "oauth",
    "clientId": "CB8713EXX55X4889",
    "issued": "2017-06-27T16:50:46",
    "expires": "2017-06-27T17:10:46"
}
```


### Criando uma Loja

Com o `accesstoken`, basta realizar um `POST` para criar uma loja de pré-cadastro.

> `POST` https://cieloecommerce.cielo.com.br/api/public/v1/PreRegistration


Conteudo do HEADER
```
Header: 
 Content-Type  :  application/json
 Authorization  : OAuth (AccessToken)
```

Requisição:
```
 {  
   "GeneralData":{  
      "CieloClient":true,
      "Name":"Name Test",
      "Email":"test@test.com",
      "SecondEmail":"test2@test.com.br",
      "AcceptReceiveCieloInformation":true,
      "CellPhone":"(99) 99999-9999",
      "Landline":"(99) 9999-9999",
      "WebSite":"http://www.test.com",
      "PersonType":"TIPO DE LOJA – Vide a Tabela 01"
   },
   "PersonalData":{  
      "Cpf":"999.999.999-99",
      "BirthDate":"31/01/2001",
      "BusinessActivity":"Tipo de atividade"
   },
   "CompanyData":{  
      "Cnpj":"99.999.999/9999-99",
      "CompanyName":"Corporate Name Test",
      "TradingName":"Trading Name Test",
      "OwnerName":"Owner Name Test",
      "OwnerCpf":"999.999.999-99",
      "OwnerBirthDate":"31/01/2001"
   },
   "AdicionalData":{  
      "PromoCode":"0123456789"
   },
   "ContactAddress":{  
      "ZipCode":"99999-999",
      "Street":"Street Test",
      "Number":"99",
      "Complement":"Complement Test",
      "City":"City Test",
      "State":"TO"
   },
   "MailAddress":{  
      "ZipCode":"11111-111",
      "Street":"Test Street",
      "Number":"11",
      "Complement":"Test Complement",
      "City":"Test City",
      "State":"AC"
   },
   "BankingData":{  
      "Bank":"Abc",
      "Agency":"1111",
      "DigitAgency":"1",
      "Account":"22222",
      "DigitAccount":"2",
      "AccountType":"CurrentAccount"
   },
   "TechnicalContact":{  
      "ContactName":"ContactNameTest",
      "ContactPhone":"99 99999-9999",
      "ContactEmail":"ContactEmailTest",
      "CompanyDeveloper":"CompanyDeveloperTest"
   },
   "PackageCielo":"Avulso"
   }
```

Resposta:
```
{
    "id": 9,
    "name": "Name Test",
    "email": "test@test.com",
    "identity": "99.999.999/9999-99",
    "tradingName": "Trading Name Test",
    "companyName": "Corporate Name Test",
    "model": "{\"GeneralData\":{\"CieloClient\":true,\"Name\":\"Name Test\",\"Email\":\"test@test.com\",\"Landline\":\"(99) 9999-9999\",\"CellPhone\":\"(99) 99999-9999\",\"AcceptReceiveCieloInformation\":true,\"PersonType\":2},\"PersonalData\":{\"Cpf\":\"999.999.999-99\",\"BusinessActivity\":8999,\"BirthDate\":\"31/01/2001\"},\"CompanyData\":{\"Cnpj\":\"99.999.999/9999-99\",\"TradingName\":\"Trading Name Test\",\"CompanyName\":\"Corporate Name Test\",\"OwnerName\":\"Owner Name Test\",\"OwnerCpf\":\"999.999.999-99\",\"OwnerBirthDate\":\"31/01/2001\"},\"AdicionalData\":{\"PromoCode\":\"0123456789\"},\"ContactAddress\":{\"City\":\"City Test\",\"Complement\":\"Complement Test\",\"Street\":\"Street Test\",\"Number\":\"99\",\"State\":26,\"ZipCode\":\"99999-999\"},\"MailAddress\":{\"City\":\"Test City\",\"Complement\":\"Test Complement\",\"Street\":\"Test Street\",\"Number\":\"11\",\"State\":0,\"ZipCode\":\"11111-111\"},\"BankingData\":{\"Bank\":246,\"Agency\":\"1111\",\"DigitAgency\":\"1\",\"AccountType\":1,\"Account\":\"22222\",\"DigitAccount\":\"2\"},\"PackageCielo\":0,\"PackageTerra\":0}"
 }
```








|Parametro                      | Descrição                                                                                           |Tipo      |Tamanho  |Obrigatório                              |
|-------------------------------|-----------------------------------------------------------------------------------------------------|----------|---------|-----------------------------------------|
| CieloClient                   | Sim / Não                                                                                           | Booleano |         | Sim                                     |
| Name                          | Nome de contato com a loja (Deve ser sempre enviado o nome completo do lojista)                     | Alfa Num.| 32      | Sim                                     |
| Email                         | E-mail que será usado para contato e como login                                                     | Alfa Num.| 60      | Sim                                     |
| SecondEmail                   | E-mail secundário caso o lojista deseje ter um segundo inbox de informações                         | Alfa Num.| 64      | Não                                     |
| CellPhone                     | Telefone celular - (99) 99999-9999                                                                  | Alfa Num.| 15      | Sim                                     |
| Landline                      | Telefone fixo (99) 99999-9999                                                                       | Alfa Num.| 15      | Sim                                     |
| AcceptReceiveCieloInformation | Sim / Não                                                                                           | Booleano |         | Sim                                     |
| Website                       | URL da loja.                                                                                        | Alfa Num.| 256     | Não                                     |
| PersonType                    | Enum de cadastro: (1- Personal (Pessoa Física)/2- Company (Pessoa Jurídica))                        | Enum     |         | Sim (Influencia outros campos) |
| CPF                           | 999.999.999-99                                                                                      | Alfa Num.| 11      | Sim (caso PF)                           |
| BusinessActivity              | Veja tabela 02                                                                                      | Enum     |         | Sim (caso PF)                           |
| BirthDate                     | DD/MM/AAAA                                                                                          | Alfa Num.| 10      | Sim (caso PF)                           |
| CNPJ                          | 99.999.999/9999-99                                                                                  | Alfa Num.| 14      | Sim (caso PJ)                           |
| TradingName                   | Nome Fantasia da loja                                                                               | Alfa Num.| 32      | Sim (caso PJ)                           |
| CompanyName                   | Razão social da loja                                                                                | Alfa Num.| 32      | Sim (caso PJ)                           |
| OwnerName                     | Nome do proprietário da loa                                                                         | Alfa Num.| 50      | Sim (caso PJ)                           |
| OwnerCpf                      | CPF do proprietário da loja  - 999.999.999-99                                                       | Alfa Num.| 11      | Sim (caso PJ)                           |
| OwnerBirthDate                | Data de nascimento do proprietário: (DD/MM/AAAA)                                                    | Alfa Num.| 10      | Sim (caso PJ)                           |
| ZipCode                       | CEP de contato - 99999-999                                                                          | Alfa Num.| 9       | Sim                                     |
| Street                        | Endereço do contato                                                                                 | Alfa Num.| 32      | Sim                                     |
| Number                        | Numero                                                                                              | Alfa Num.| 6       | Sim                                     |
| Complement (opcional)         | Complemento do endereço                                                                             | Alfa Num.| 26      | Não                                     |
| City                          | Cidade                                                                                              | Alfa Num.| 28      | Sim                                     |
| State                         | Unidade Federativa (EX: RJ)                                                                         | Enum     |         | Sim                                     |
| ZipCode                       | 99999-999                                                                                           | Alfa Num.| 9       | Sim                                     |
| Street                        | Endereço da Loja                                                                                    | Alfa Num.| 32      | Sim                                     |
| Number                        | Numero                                                                                              | Alfa Num.| 6       | Sim                                     |
| Complement (opcional)         | Complemento do endereço                                                                             | Alfa Num.| 26      | Não                                     |
| City                          | Cidade                                                                                              | Alfa Num.| 28      | Sim                                     |
| State                         | Unidade Federativa (EX: RJ)                                                                         | Enum     |         | Sim                                     |
| Bank                          | Veja tabela 03                                                                                      | Enum     |         | Sim                                     |
| Agency                        | Agência bancaria                                                                                    | Alfa Num.| 4       | Sim                                     |
| DigitAgency                   | Digito da agência                                                                                   | Alfa Num.| 1       | Para Banco do Brasil|
| AccountType                   | Enum de Conta bancaria: (001 – CurrentAccount (Conta Corrente)/002 – SimpleAccount (Conta Simples)) | Enum     |         | Para Caixa Economica Federal|
| Account                       | Conta Bancaria                                                                                      | Alfa Num.| 13      | Sim                                     |
| DigitAccount                  | Digito da conta                                                                                     | Alfa Num.| 1       | Sim                                     |
| ContactName                   | Nome do desenvolvedor da loja                                                                       | Alfa Num.| 32      | Não                                     |
| ContactPhone                  | Numero de telefone do desenvolvedor                                                                 | Alfa Num.| 15      | 15                                      |
| ContactEmail                  | E-mail do desenvolvedor da loja                                                                     | Alfa Num.| 64      | Não                                     |
| CompanyDeveloper              | Nome da empresa responsável pelo desenvolvimento da loja                                            | Alfa Num.| 32      | Não                                     |
| PackageCielo                  | Enum de pacote Cielo:(Avulso/PlanoA/PlanoB/PlanoC/PlanoD)                                           | Enum     |         | Sim                                     |			

> 
1. Os campos “**PersonalData**” e “**CompanyData**” passam a ser obrigatórios ou não de acordo com o tipo de loja definido no campo “**PersonType**”. 
2. Os campos “**BusinessActivity**” deve conter apenas o numero do código da atividade da loja ou a denominação descrita na tabela 02 (denominação em inglês)
3. Os nomes dos planos devem ser enviados juntos, sem espaço ( “**plano A**” =  **Errado** / “**planoStart**” = **Correto** )
4. A API apenas cria as lojas. **Não é possível editar ou sobre escrever lojas enviadas via pré-cadastro via API, somente acionando a equipe de suporte Cielo**.


**Tabela 02 - BusinessActivity**

| Código | Atividade                     | Descrição                                       |
|--------|-------------------------------|-------------------------------------------------|
| 5137   | TailorsSeamstresses           | Alfaiates/Costureiras                           |
| 7211   | Nanny                         | Babá                                            |
| 8299   | SingerBandsOrchestras         | Cantor /Bandas /Orquestras                      |
| 8911   | BuildingDecoration            | Construção / Decoração                          |
| 6211   | InsuranceBroker               | Corretor de Seguros                             |
| 6513   | Realtors                      | Corretores Imobiliários                         |
| 4215   | CourierFastDelivery           | Courier /Entregas Rápidas                       |
| 5814   | SweetmeatFrozenFood           | Docerias / Comidas Congeladas                   |
| 7333   | PhotographyAndFilming         | Fotografia e Filmagem)                           |
| 8999   | ActivityNotFound              | Não encontrei minha atividade                   |
| 780    | Landscaping                   | Paisagismo                                      |
| 7997   | PersonalTrainner              | Personal Trainner                               |
| 8049   | PodiatryManicure              | Podologia / Manicure                            |
| 8244   | Teacher                       | Professor                                       |
| 7230   | BeautySalonBarberShop         | Salão de Beleza / Barbearia                     |
| 8011   | MedicalServices               | Serviços Médicos                                |
| 8021   | DentalServices                | Serviços Odontológicos                          |
| 742    | VeterinaryServices            | Serviços Veterinários                           |
| 7277   | AttorneysServices             | Serviços Advocatícios                           |
| 7298   | AestheticTreatments           | Tratamentos Estéticos                           |
| 4121   | Taxi                          | Táxis                                           |
| 5964   | CatalogsSales                 | Vendas por Catálogos                            |



**Tabela 03 – Bank**

| Código | Atividade                     | Descrição                                       |
|--------|-------------------------------|-------------------------------------------------|
| 1      | BancoDoBrasil                 | BANCO DO BRASIL S.A.                            |
| 237    | Bradesco                      | BANCO BRADESCO S.A.                             |
| 104    | CaixaEconomicaFederal         | CAIXA ECONÔMICA FEDERAL                         |
| 399    | HsbcmuLtiplo                  | HSBC BANK BRASIL S.A.- BANCO MÚLTIPLO           |
| 341    | ItauUnibanco                  | ITAÚ UNIBANCO S.A.                              |
| 33     | Santander                     | BANCO SANTANDER (BRASIL) S.A.                   |
| 21     | Banestes                      | BANESTES S.A. BANCO DO ESTADO DO ESPÍRITO SANTO |
| 70     | BrbBancodebrasília            | BRB- BANCO DE BRASÍLIA S.A.                     |
| 246    | Abc                           | BANCO ABC BRASIL S.A.                           |
| 25     | Alfa                          | BANCO ALFA S.A.                                 |
| 44     | Bva                           | BANCO BVA S.A.                                  |
| 745    | Citibank                      | BANCO CITIBANK S.A.                             |
| 748    | Sicredi                       | BANCO COOPERATIVO SICREDI S.A)                  |
| 756    | Bancoob                       | BANCO COOPERATIVO DO BRASIL S.A - BANCOOB       |
| 707    | Daycoval                      | BANCO DAYCOVAL S.A.                             |
| 604    | Bancoindustrialdobrasil       | BANCO INDUSTRIAL DO BRASIL S.A.                 |
| 389    | Mercantil                     | BANCO MERCANTIL DO BRASIL S.A.                  |
| 623    | Panamericano                  | BANCO PANAMERICANO S.A.                         |
| 453    | Rural                         | BANCO RURAL S.A.                                |
| 422    | Safra                         | BANCO SAFRA S.A.                                |
| 634    | BancoTriangulo                | BANCO TRIÂNGULO S.A.                            |
| 655    | Votorantim                    | BANCO VOTORANTIM S.A.                           |
| 41     | BancoDoEstadoDoRioGrandeDoSul | BANCO DO ESTADO DO RIO GRANDE DO SUL S.A.       |






