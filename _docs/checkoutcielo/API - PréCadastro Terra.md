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

### API da Autoriza&ccedil;&atilde;o

Para iniciar o pr&eacute;-cadastro de uma loja &eacute; necess&aacute;rio obter uma autoriza&ccedil;&atilde;o (AccessToken) de acesso a API cadastral. Para se obter o AccessToken &eacute; obrigat&oacute;rio realizar um POST a API utilizando credenciais como as abaixo:

* **ClientId**: CB8713EXX55X4889
* **ClientSecret**: BA89FB6F1AXXXD4D874C93532D7C44A9639D092A004E4FB78CCF331385F

**OBS:** As Credenciais de produ&ccedil;&atilde;o ser&atilde;o fornecidas a plataformas registradas junto a equipe de PRODUTOS CIELO.

Um `POST`{: .highlighter-rouge} deve ser realizado a URL abaixo:

> `POST`{: .highlighter-rouge}: https://cieloecommerce.cielo.com.br/api/public/v1/`accesstoken`{: .highlighter-rouge}

**Requisi&ccedil;&atilde;o - AccessToken** Conteuido do Header

<div class="highlighter-rouge"><pre class="highlight"><code><span class="w">  </span><span class="p">{</span><span class="w">
  </span><span class="nt">"ClientId"</span><span class="p">:</span><span class="s2">"CB8713EXX55X4889"</span><span class="p">,</span><span class="w">
  </span><span class="nt">"ClientSecret"</span><span class="p">:</span><span class="s2">"BA89FB6F1AXXXD4D874C93532D7C44A9639D092A004E4FB78CCF331385F"</span><span class="w">
  </span><span class="p">}</span><span class="w">

</span></code></pre></div>

**Resposta - - AccessToken**

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
    </span><span class="nt">"accessToken"</span><span class="p">:</span><span class="w"> </span><span class="s2">"ZDViNWI5ZDMtNTExZS00MDAyLTkxMTQtYTYwMWEzZDg4ZGJjMDY2MDgzMzUzNw=="</span><span class="p">,</span><span class="w">
    </span><span class="nt">"tokenType"</span><span class="p">:</span><span class="w"> </span><span class="s2">"oauth"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"clientId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"CB8713EXX55X4889"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"issued"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2017-06-27T16:50:46"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"expires"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2017-06-27T17:10:46"</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div>

### Criando uma Loja

Com o `accesstoken`{: .highlighter-rouge}, basta realizar um `POST`{: .highlighter-rouge} para criar uma loja de pr&eacute;-cadastro.

> `POST`{: .highlighter-rouge} https://cieloecommerce.cielo.com.br/api/public/v1/PreRegistration

Conteudo do HEADER

<div class="highlighter-rouge"><pre class="highlight"><code>Header:
 Content-Type  :  application/json
 Authorization  : OAuth (AccessToken)
</code></pre></div>

Requisi&ccedil;&atilde;o:

<div class="highlighter-rouge"><pre class="highlight"><code><span class="w"> </span><span class="p">{</span><span class="w">
   </span><span class="nt">"GeneralData"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"CieloClient"</span><span class="p">:</span><span class="kc">true</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Name"</span><span class="p">:</span><span class="s2">"Name Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Email"</span><span class="p">:</span><span class="s2">"test@test.com"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"SecondEmail"</span><span class="p">:</span><span class="s2">"test2@test.com.br"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"AcceptReceiveCieloInformation"</span><span class="p">:</span><span class="kc">true</span><span class="p">,</span><span class="w">
      </span><span class="nt">"CellPhone"</span><span class="p">:</span><span class="s2">"(99) 99999-9999"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Landline"</span><span class="p">:</span><span class="s2">"(99) 9999-9999"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"WebSite"</span><span class="p">:</span><span class="s2">"http://www.test.com"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"PersonType"</span><span class="p">:</span><span class="s2">"TIPO DE LOJA &ndash; Vide a Tabela 01"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"PersonalData"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"Cpf"</span><span class="p">:</span><span class="s2">"999.999.999-99"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"BirthDate"</span><span class="p">:</span><span class="s2">"31/01/2001"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"BusinessActivity"</span><span class="p">:</span><span class="s2">"Tipo de atividade"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"CompanyData"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"Cnpj"</span><span class="p">:</span><span class="s2">"99.999.999/9999-99"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"CompanyName"</span><span class="p">:</span><span class="s2">"Corporate Name Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"TradingName"</span><span class="p">:</span><span class="s2">"Trading Name Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"OwnerName"</span><span class="p">:</span><span class="s2">"Owner Name Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"OwnerCpf"</span><span class="p">:</span><span class="s2">"999.999.999-99"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"OwnerBirthDate"</span><span class="p">:</span><span class="s2">"31/01/2001"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"AdicionalData"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"PromoCode"</span><span class="p">:</span><span class="s2">"0123456789"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"ContactAddress"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"ZipCode"</span><span class="p">:</span><span class="s2">"99999-999"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Street"</span><span class="p">:</span><span class="s2">"Street Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Number"</span><span class="p">:</span><span class="s2">"99"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Complement"</span><span class="p">:</span><span class="s2">"Complement Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"City"</span><span class="p">:</span><span class="s2">"City Test"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"State"</span><span class="p">:</span><span class="s2">"TO"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"MailAddress"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"ZipCode"</span><span class="p">:</span><span class="s2">"11111-111"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Street"</span><span class="p">:</span><span class="s2">"Test Street"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Number"</span><span class="p">:</span><span class="s2">"11"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Complement"</span><span class="p">:</span><span class="s2">"Test Complement"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"City"</span><span class="p">:</span><span class="s2">"Test City"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"State"</span><span class="p">:</span><span class="s2">"AC"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"BankingData"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"Bank"</span><span class="p">:</span><span class="s2">"Abc"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Agency"</span><span class="p">:</span><span class="s2">"1111"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"DigitAgency"</span><span class="p">:</span><span class="s2">"1"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"Account"</span><span class="p">:</span><span class="s2">"22222"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"DigitAccount"</span><span class="p">:</span><span class="s2">"2"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"AccountType"</span><span class="p">:</span><span class="s2">"CurrentAccount"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"TechnicalContact"</span><span class="p">:{</span><span class="w">
      </span><span class="nt">"ContactName"</span><span class="p">:</span><span class="s2">"ContactNameTest"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"ContactPhone"</span><span class="p">:</span><span class="s2">"99 99999-9999"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"ContactEmail"</span><span class="p">:</span><span class="s2">"ContactEmailTest"</span><span class="p">,</span><span class="w">
      </span><span class="nt">"CompanyDeveloper"</span><span class="p">:</span><span class="s2">"CompanyDeveloperTest"</span><span class="w">
   </span><span class="p">},</span><span class="w">
   </span><span class="nt">"PackageCielo"</span><span class="p">:</span><span class="s2">"Avulso"</span><span class="w">
   </span><span class="p">}</span><span class="w">
</span></code></pre></div>

Resposta:

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
    </span><span class="nt">"id"</span><span class="p">:</span><span class="w"> </span><span class="mi">9</span><span class="p">,</span><span class="w">
    </span><span class="nt">"name"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Name Test"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"email"</span><span class="p">:</span><span class="w"> </span><span class="s2">"test@test.com"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"identity"</span><span class="p">:</span><span class="w"> </span><span class="s2">"99.999.999/9999-99"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"tradingName"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Trading Name Test"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"companyName"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Corporate Name Test"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"model"</span><span class="p">:</span><span class="w"> </span><span class="s2">"{\"GeneralData\":{\"CieloClient\":true,\"Name\":\"Name Test\",\"Email\":\"test@test.com\",\"Landline\":\"(99) 9999-9999\",\"CellPhone\":\"(99) 99999-9999\",\"AcceptReceiveCieloInformation\":true,\"PersonType\":2},\"PersonalData\":{\"Cpf\":\"999.999.999-99\",\"BusinessActivity\":8999,\"BirthDate\":\"31/01/2001\"},\"CompanyData\":{\"Cnpj\":\"99.999.999/9999-99\",\"TradingName\":\"Trading Name Test\",\"CompanyName\":\"Corporate Name Test\",\"OwnerName\":\"Owner Name Test\",\"OwnerCpf\":\"999.999.999-99\",\"OwnerBirthDate\":\"31/01/2001\"},\"AdicionalData\":{\"PromoCode\":\"0123456789\"},\"ContactAddress\":{\"City\":\"City Test\",\"Complement\":\"Complement Test\",\"Street\":\"Street Test\",\"Number\":\"99\",\"State\":26,\"ZipCode\":\"99999-999\"},\"MailAddress\":{\"City\":\"Test City\",\"Complement\":\"Test Complement\",\"Street\":\"Test Street\",\"Number\":\"11\",\"State\":0,\"ZipCode\":\"11111-111\"},\"BankingData\":{\"Bank\":246,\"Agency\":\"1111\",\"DigitAgency\":\"1\",\"AccountType\":1,\"Account\":\"22222\",\"DigitAccount\":\"2\"},\"PackageCielo\":0,\"PackageTerra\":0}"</span><span class="w">
 </span><span class="p">}</span><span class="w">
</span></code></pre></div>

| Parametro | Descri&ccedil;&atilde;o | Tipo | Tamanho | Obrigat&oacute;rio |
| --- | --- | --- | --- | --- |
| CieloClient | Sim / N&atilde;o | Booleano | &nbsp; | Sim |
| Name | Nome de contato com a loja (Deve ser sempre enviado o nome completo do lojista) | Alfa Num. | 32 | Sim |
| Email | E-mail que ser&aacute; usado para contato e como login | Alfa Num. | 60 | Sim |
| SecondEmail | E-mail secund&aacute;rio caso o lojista deseje ter um segundo inbox de informa&ccedil;&otilde;es | Alfa Num. | 64 | N&atilde;o |
| CellPhone | Telefone celular - (99) 99999-9999 | Alfa Num. | 15 | Sim |
| Landline | Telefone fixo (99) 99999-9999 | Alfa Num. | 15 | Sim |
| AcceptReceiveCieloInformation | Sim / N&atilde;o | Booleano | &nbsp; | Sim |
| Website | URL da loja. | Alfa Num. | 256 | N&atilde;o |
| PersonType | Enum de cadastro: (1- Personal (Pessoa F&iacute;sica)/2- Company (Pessoa Jur&iacute;dica)) | Enum | &nbsp; | Sim (Influencia outros campos) |
| CPF | 999.999.999-99 | Alfa Num. | 11 | Sim (caso PF) |
| BusinessActivity | Veja tabela 02 | Enum | &nbsp; | Sim (caso PF) |
| BirthDate | DD/MM/AAAA | Alfa Num. | 10 | Sim (caso PF) |
| CNPJ | 99.999.999/9999-99 | Alfa Num. | 14 | Sim (caso PJ) |
| TradingName | Nome Fantasia da loja | Alfa Num. | 32 | Sim (caso PJ) |
| CompanyName | Raz&atilde;o social da loja | Alfa Num. | 32 | Sim (caso PJ) |
| OwnerName | Nome do propriet&aacute;rio da loa | Alfa Num. | 50 | Sim (caso PJ) |
| OwnerCpf | CPF do propriet&aacute;rio da loja - 999.999.999-99 | Alfa Num. | 11 | Sim (caso PJ) |
| OwnerBirthDate | Data de nascimento do propriet&aacute;rio: (DD/MM/AAAA) | Alfa Num. | 10 | Sim (caso PJ) |
| ZipCode | CEP de contato - 99999-999 | Alfa Num. | 9 | Sim |
| Street | Endere&ccedil;o do contato | Alfa Num. | 32 | Sim |
| Number | Numero | Alfa Num. | 6 | Sim |
| Complement (opcional) | Complemento do endere&ccedil;o | Alfa Num. | 26 | N&atilde;o |
| City | Cidade | Alfa Num. | 28 | Sim |
| State | Unidade Federativa (EX: RJ) | Enum | &nbsp; | Sim |
| ZipCode | 99999-999 | Alfa Num. | 9 | Sim |
| Street | Endere&ccedil;o da Loja | Alfa Num. | 32 | Sim |
| Number | Numero | Alfa Num. | 6 | Sim |
| Complement (opcional) | Complemento do endere&ccedil;o | Alfa Num. | 26 | N&atilde;o |
| City | Cidade | Alfa Num. | 28 | Sim |
| State | Unidade Federativa (EX: RJ) | Enum | &nbsp; | Sim |
| Bank | Veja tabela 03 | Enum | &nbsp; | Sim |
| Agency | Ag&ecirc;ncia bancaria | Alfa Num. | 4 | Sim |
| DigitAgency | Digito da ag&ecirc;ncia | Alfa Num. | 1 | Para Banco do Brasil |
| AccountType | Enum de Conta bancaria: (001 – CurrentAccount (Conta Corrente)/002 – SimpleAccount (Conta Simples)) | Enum | &nbsp; | Para Caixa Economica Federal |
| Account | Conta Bancaria | Alfa Num. | 13 | Sim |
| DigitAccount | Digito da conta | Alfa Num. | 1 | Sim |
| ContactName | Nome do desenvolvedor da loja | Alfa Num. | 32 | N&atilde;o |
| ContactPhone | Numero de telefone do desenvolvedor | Alfa Num. | 15 | 15 |
| ContactEmail | E-mail do desenvolvedor da loja | Alfa Num. | 64 | N&atilde;o |
| CompanyDeveloper | Nome da empresa respons&aacute;vel pelo desenvolvimento da loja | Alfa Num. | 32 | N&atilde;o |
| PackageCielo | Enum de pacote Cielo:(Avulso/PlanoA/PlanoB/PlanoC/PlanoD) | Enum | &nbsp; | Sim |

> 1. Os campos “**PersonalData**” e “**CompanyData**” passam a ser obrigat&oacute;rios ou n&atilde;o de acordo com o tipo de loja definido no campo “**PersonType**”.
> 2. Os campos “**BusinessActivity**” deve conter apenas o numero do c&oacute;digo da atividade da loja ou a denomina&ccedil;&atilde;o descrita na tabela 02 (denomina&ccedil;&atilde;o em ingl&ecirc;s)
> 3. Os nomes dos planos devem ser enviados juntos, sem espa&ccedil;o ( “**plano A**” = **Errado** / “**planoStart**” = **Correto** )
> 4. A API apenas cria as lojas. **N&atilde;o &eacute; poss&iacute;vel editar ou sobrescrever lojas enviadas via pr&eacute;-cadastro via API, somente acionando a equipe de suporte Cielo**.

**Tabela 02 - BusinessActivity**

| C&oacute;digo | Atividade | Descri&ccedil;&atilde;o |
| --- | --- | --- |
| 5137 | TailorsSeamstresses | Alfaiates/Costureiras |
| 7211 | Nanny | Bab&aacute; |
| 8299 | SingerBandsOrchestras | Cantor /Bandas /Orquestras |
| 8911 | BuildingDecoration | Constru&ccedil;&atilde;o / Decora&ccedil;&atilde;o |
| 6211 | InsuranceBroker | Corretor de Seguros |
| 6513 | Realtors | Corretores Imobili&aacute;rios |
| 4215 | CourierFastDelivery | Courier /Entregas R&aacute;pidas |
| 5814 | SweetmeatFrozenFood | Docerias / Comidas Congeladas |
| 7333 | PhotographyAndFilming | Fotografia e Filmagem) |
| 8999 | ActivityNotFound | N&atilde;o encontrei minha atividade |
| 780 | Landscaping | Paisagismo |
| 7997 | PersonalTrainner | Personal Trainner |
| 8049 | PodiatryManicure | Podologia / Manicure |
| 8244 | Teacher | Professor |
| 7230 | BeautySalonBarberShop | Sal&atilde;o de Beleza / Barbearia |
| 8011 | MedicalServices | Servi&ccedil;os M&eacute;dicos |
| 8021 | DentalServices | Servi&ccedil;os Odontol&oacute;gicos |
| 742 | VeterinaryServices | Servi&ccedil;os Veterin&aacute;rios |
| 7277 | AttorneysServices | Servi&ccedil;os Advocat&iacute;cios |
| 7298 | AestheticTreatments | Tratamentos Est&eacute;ticos |
| 4121 | Taxi | T&aacute;xis |
| 5964 | CatalogsSales | Vendas por Cat&aacute;logos |

**Tabela 03 – Bank**

| C&oacute;digo | Atividade | Descri&ccedil;&atilde;o |
| --- | --- | --- |
| 1 | BancoDoBrasil | BANCO DO BRASIL S.A. |
| 237 | Bradesco | BANCO BRADESCO S.A. |
| 104 | CaixaEconomicaFederal | CAIXA ECON&Ocirc;MICA FEDERAL |
| 399 | HsbcmuLtiplo | HSBC BANK BRASIL S.A.- BANCO M&Uacute;LTIPLO |
| 341 | ItauUnibanco | ITA&Uacute; UNIBANCO S.A. |
| 33 | Santander | BANCO SANTANDER (BRASIL) S.A. |
| 21 | Banestes | BANESTES S.A. BANCO DO ESTADO DO ESP&Iacute;RITO SANTO |
| 70 | BrbBancodebras&iacute;lia | BRB- BANCO DE BRAS&Iacute;LIA S.A. |
| 246 | Abc | BANCO ABC BRASIL S.A. |
| 25 | Alfa | BANCO ALFA S.A. |
| 44 | Bva | BANCO BVA S.A. |
| 745 | Citibank | BANCO CITIBANK S.A. |
| 748 | Sicredi | BANCO COOPERATIVO SICREDI S.A) |
| 756 | Bancoob | BANCO COOPERATIVO DO BRASIL S.A - BANCOOB |
| 707 | Daycoval | BANCO DAYCOVAL S.A. |
| 604 | Bancoindustrialdobrasil | BANCO INDUSTRIAL DO BRASIL S.A. |
| 389 | Mercantil | BANCO MERCANTIL DO BRASIL S.A. |
| 623 | Panamericano | BANCO PANAMERICANO S.A. |
| 453 | Rural | BANCO RURAL S.A. |
| 422 | Safra | BANCO SAFRA S.A. |
| 634 | BancoTriangulo | BANCO TRI&Acirc;NGULO S.A. |
| 655 | Votorantim | BANCO VOTORANTIM S.A. |
| 41 | BancoDoEstadoDoRioGrandeDoSul | BANCO DO ESTADO DO RIO GRANDE DO SUL S.A. |
