# 🔐 OAuth2 Login com Google + Spring Boot

Projeto de exemplo demonstrando autenticação com Google utilizando:

- Frontend HTML + Google Identity Services
- Backend Spring Boot
- OAuth2
- JWT Token do Google
- Spring Security

---

# 📌 Objetivo

Permitir que usuários façam login utilizando conta Google sem necessidade de criar senha na aplicação.

Fluxo:

1. Usuário clica em “Login com Google”
2. Google autentica usuário
3. Google retorna um JWT Token
4. Frontend envia token para backend Spring
5. Spring valida token
6. Usuário é autenticado

---

# 🧠 Conceitos importantes

## OAuth2

OAuth2 é um protocolo de autorização.

Neste caso, a autenticação é delegada ao Google.  
A aplicação não armazena senha do usuário.

---

## JWT

O Google retorna um JWT (JSON Web Token).

Esse token contém informações como:

- nome do usuário
- email
- foto
- tempo de expiração
- assinatura digital do Google

---

## Google Identity Services

Biblioteca oficial do Google responsável por:

- renderizar botão login
- autenticar usuário
- gerar token JWT

Script utilizado:

```html
<script src="https://accounts.google.com/gsi/client" async defer></script>
```

---

## Spring Security

Responsável por:

- validar autenticação
- proteger endpoints
- gerenciar sessão ou JWT interno

---

# ⚙️ Tecnologias

- Java 17+
- Spring Boot
- Spring Security
- OAuth2
- HTML
- JavaScript
- Google Identity Services

---

# 📁 Estrutura esperada

```txt
src/
 ├── main/
 │    ├── java/
 │    ├── resources/
 │    │      └── application.yml
 │    └── static/
 │           └── index.html
```

---

# 🚀 Configuração Google Cloud

## 1. Criar projeto

Acesse:

https://console.cloud.google.com/

Crie um novo projeto.

---

## 2. Configurar OAuth2

Vá em:

```txt
APIs & Services → Credentials
```

Crie:

```txt
OAuth Client ID
```

Tipo:

```txt
Web Application
```

---

## 3. Configurar Authorized Origins

Exemplo:

```txt
http://localhost:8080
```

---

## 4. Configurar Redirect URI

Exemplo:

```txt
http://localhost:8080/login/oauth2/code/google
```

---

## 5. Copiar credenciais

Você receberá:

- Client ID
- Client Secret

---

# ⚙️ Configuração Spring Boot

## Dependência Maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

---

## application.yml

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: SEU_CLIENT_ID
            client-secret: SEU_CLIENT_SECRET
            scope:
              - email
              - profile
```

---

# 🌐 Frontend HTML

## index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OAuth2 Google Login</title>
</head>
<body>

    <h1>Login OAuth2.0 Google</h1>

    <script src="https://accounts.google.com/gsi/client" async defer></script>

    <div id="g_id_onload"
        data-client_id="SEU_CLIENT_ID"
        data-callback="handleCredentialResponse"
        data-auto_prompt="false">
    </div>

    <div class="g_id_signin"
        data-type="standard"
        data-size="large"
        data-theme="outline"
        data-text="signin_with"
        data-shape="rectangular"
        data-logo_alignment="left">
    </div>

    <script>
        function handleCredentialResponse(response) {
            console.log("JWT recebido:", response.credential);

            fetch('/login/google', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    token: response.credential
                })
            });
        }
    </script>

</body>
</html>
```

---

# 🔐 O que acontece após login

Quando usuário autentica:

1. Google gera JWT
2. Frontend recebe token
3. Token é enviado para backend
4. Backend valida assinatura Google
5. Backend autentica usuário

---

# ▶️ Como rodar projeto

## 1. Clonar projeto

```bash
git clone SEU_REPOSITORIO
```

---

## 2. Configurar Client ID e Secret

Editar:

```txt
application.yml
```

E também:

```html
index.html
```

---

## 3. Rodar aplicação

Maven:

```bash
mvn spring-boot:run
```

Ou:

```bash
./mvnw spring-boot:run
```

---

## 4. Acessar navegador

```txt
http://localhost:8080
```

---

# ⚠️ Problemas comuns

## Invalid Client ID

Causa:

- client_id incorreto
- domínio não autorizado no Google Cloud

---

## redirect_uri_mismatch

Causa:

- Redirect URI diferente da configurada no Google Cloud

---

## JWT inválido

Causa:

- token expirado
- assinatura inválida
- backend não validando corretamente

---

# ✅ Vantagens do OAuth2 Google

- Não armazena senha
- Login rápido
- Segurança maior
- Menor responsabilidade da aplicação
- Melhor experiência usuário

---

# 📚 Referências

Google Identity:

https://developers.google.com/identity/gsi/web

Spring Security OAuth2:

https://docs.spring.io/spring-security/reference/servlet/oauth2/index.html
