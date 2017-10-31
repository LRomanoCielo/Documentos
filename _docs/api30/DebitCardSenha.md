---
title: Tokens de Acesso
category: Autenticação
order: 1
---

As APIs BlackBox utilizam o protocolo padrão de mercado OAuth 2.0 para autorização de acesso a seus recursos.

Este documento descreve o fluxo necessário para que aplicações cliente obtenham tokens de acesso válidos para uso na plataforma. 

> Para saber mais sobre OAuth 2, consulte [https://oauth.net/2/](https://oauth.net/2/)  

<a name="topo"></a>

### Obtenção do token de acesso

O token de acesso é obtido através do fluxo de autorização **Resource Owner Password Credentials**. O diagrama a seguir ilustra, em ordem cronológica, a comunicação que se dá entre a **Aplicação Cliente**, o **Servidor de Autorização** e as APIs.

![Obtenção de Tokens de Acesso]({{ site.url }}/images/docs-blackboxapi-02.png){: .centerimg }{:title="Fluxo para obtenção do Token de Acesso "}

1. O usuário, através da **Aplicação Cliente**, informa ao **Authorization Server** suas credenciais de serviço.  

2. O **Authorization Server** valida a credencial recebida. Se for válida, retorna o token de acesso para a **Aplicação Cliente**.  

3. A **Aplicação Cliente** informa o token de acesso obtido no cabeçalho das requisições HTTP feitas às APIs.   

4. Se o token de acesso for válido, a requisição é processada e os dados são retornados para a **Aplicação Cliente**.

<a name="http_operations"></a>

### Operações HTTP  

`POST`{:.http-post} [/oauth/token](#post_oauth){:.custom-attrib}  
Criação de token de acesso para as APIs BlackBox.  

<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>


<a name="post_oauth"></a>

### **POST** /oauth/token  

**DESCRIÇÃO:**  
Criação de token de acesso para as APIs BlackBox.

**PARÂMETROS NO CORPO:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| username | Nome único do SKU | `String` `64` |
| password | Descrição do SKU | `String` `1024`  |
| grant_type | Habilita ou não o controle de estoque para o SKU | `Boolean`  |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
POST /oauth/token HTTP/1.1
Host: {blackbox endpoint}
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

username={usuario}&password={senha}&grant_type=password
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "token_type": "bearer",
    "expires_in": 86399
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

----------------
