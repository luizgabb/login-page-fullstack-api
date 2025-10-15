# Login Page API

## Visão Geral do Projeto
Este projeto é o **backend** de uma aplicação de login simples, utilizando **Spring Boot**, **Spring Security** e **JWT** para autenticação.  
O objetivo é fornecer uma **API segura** que permita que um frontend (Angular) realize login e autenticação de usuários com apenas uma role (`USER`).  
O banco de dados utilizado é o **H2**, em memória, para facilitar testes e desenvolvimento rápido.

---

## Tecnologias Utilizadas
- **Java 21**
- **Spring Boot 3.x**
- **Spring Security**
- **JWT (JSON Web Token)**
- **H2 Database**
- **Maven**
- **Lombok**
- **Spring Web / Spring Data JPA**

---

## Arquitetura do Projeto
O projeto segue a arquitetura padrão **Spring Boot com camadas**:

Controller -> Service -> Repository -> Database

yaml
Copiar código

- **Controller:** Recebe as requisições HTTP e retorna respostas JSON.  
- **Service:** Contém a lógica de autenticação e regras de negócio.  
- **Repository:** Interface para persistência de dados no banco H2.  
- **Model/Entity:** Representa o usuário com atributos `id`, `email`, `password` e `role`.

---

## Fluxo de Autenticação (Spring Security + JWT)
1. O frontend envia um POST para `/auth/login` com `email` e `password`.  
2. Spring Security valida as credenciais.  
3. Se autenticado, o backend gera um **JWT token**.  
4. O token é retornado ao frontend.  
5. O frontend deve enviar este token no header `Authorization: Bearer <token>` para acessar rotas protegidas.

---

## Endpoints Principais

### Autenticação
| Método | Endpoint        | Descrição                    | Body |
|--------|----------------|-----------------------------|------|
| POST   | `/auth/login`   | Realiza login do usuário     | `{ "email": "...", "password": "..." }` |

### Usuários
| Método | Endpoint        | Descrição                    | Body |
|--------|----------------|-----------------------------|------|
| GET    | `/users/me`     | Retorna dados do usuário logado | Header: `Authorization: Bearer <token>` |

> Observação: Outras rotas podem ser adicionadas conforme necessidade, mas neste projeto básico apenas `/auth/login` e `/users/me` são essenciais.

---

## Modelo de Dados (H2 Database)
Tabela principal: **users**

| Campo    | Tipo    | Observações |
|----------|--------|-------------|
| id       | Long   | Auto-increment |
| email    | String | Único, usado para login |
| password | String | Hash da senha |
| role     | String | Default: `USER` |

O banco H2 é configurado para rodar em memória, facilitando testes sem configuração extra.

---

## Como Executar o Projeto Localmente

### Pré-requisitos
- Java 17 ou superior
- Maven 3.x
- IDE (IntelliJ, Eclipse) ou terminal

### Passos para execução
1. Clone o repositório:
```bash
git clone https://github.com/luizgabb/login-page-fullstack-api.git
Entre na pasta do projeto:

bash
Copiar código
cd login-page-fullstack-api
Rode o projeto com Maven:

bash
Copiar código
mvn spring-boot:run
O backend estará disponível em http://localhost:8080

Console do H2
Acesse: http://localhost:8080/h2-console

JDBC URL: jdbc:h2:mem:testdb

Username: sa

Password: (vazio)

Variáveis de Ambiente / Configurações
As principais variáveis de configuração estão no application.properties:

properties
Copiar código
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
jwt.secret=YOUR_SECRET_KEY
jwt.expiration=3600000
Nota: Altere jwt.secret para um valor seguro em produção.

Integração com o Frontend (Angular)
O frontend deve enviar a requisição POST para /auth/login com email e password.

Receberá o JWT token que deve ser armazenado (ex: localStorage).

Para acessar rotas protegidas, enviar o token no header:

makefile
Copiar código
Authorization: Bearer <token>
Exemplo em Angular (HttpClient):

typescript
Copiar código
http.post(`${API_URL}/auth/login`, { email, password })
    .subscribe((res: any) => localStorage.setItem('token', res.token));
Boas Práticas Implementadas
Senhas armazenadas com hash seguro (BCrypt)

JWT com tempo de expiração definido

Spring Security configurado com roles

H2 para testes rápidos sem necessidade de banco externo

Possíveis Melhorias Futuras
Implementar refresh token para maior segurança

Suporte a múltiplas roles (ADMIN, USER, etc.)

Persistência em banco externo (PostgreSQL, MySQL)

Logging centralizado e tratamento de exceções global

Validação de entrada de dados com Bean Validation

Licença
Este projeto é open source, livre para uso e estudo.
