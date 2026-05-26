<h1 align="center">🕐 Ponto Inteligente API</h1>

<p align="center">
  <img alt="Java" src="https://img.shields.io/badge/Java-8-007396?style=for-the-badge&logo=openjdk&logoColor=white">
  <img alt="Spring Boot" src="https://img.shields.io/badge/Spring_Boot-1.5.6-6DB33F?style=for-the-badge&logo=spring&logoColor=white">
  <img alt="MySQL" src="https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white">
  <img alt="Build Status" src="https://img.shields.io/travis/michelspirlandeli/ponto-inteligente-api/master?style=for-the-badge">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-brightgreen?style=for-the-badge">
</p>

<p align="center">
  <a href="#-sobre">Sobre</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-funcionalidades">Funcionalidades</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-stack">Stack</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-arquitetura">Arquitetura</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-como-executar">Como executar</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-endpoints">Endpoints</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-testes">Testes</a>&nbsp;&nbsp;•&nbsp;
  <a href="#-licença">Licença</a>
</p>

---

## 💡 Sobre

**Ponto Inteligente** é uma API RESTful para controle de ponto eletrônico de funcionários. O sistema permite o registro de lançamentos de entrada e saída com suporte a dois perfis de acesso — **administrador** e **pessoa física (PF)** — com autenticação via token JWT.

O projeto foi construído com foco em boas práticas de desenvolvimento: separação de responsabilidades em camadas, autenticação stateless com Spring Security, migração de banco de dados versionada com Flyway e cache de consultas com EhCache.

---

## ✅ Funcionalidades

- 🔐 **Autenticação JWT** — login seguro com token stateless via Spring Security
- 👤 **Dois perfis de acesso** — ADMIN (empresa) e USUARIO (funcionário PF)
- 📋 **Cadastro de empresas** — criação com CNPJ validado
- 👷 **Cadastro de funcionários** — vínculo com empresa, CPF e dados pessoais
- ⏱️ **Lançamentos de ponto** — registro de entrada, saída, início e fim de almoço
- 📄 **Paginação** — listagem de lançamentos com suporte a página e tamanho configuráveis
- ⚡ **Cache** — respostas de funcionários cacheadas com EhCache para melhor performance
- 📖 **Documentação** — Swagger UI disponível para explorar e testar todos os endpoints
- 🔄 **CI** — integração contínua configurada com TravisCI

---

## 🛠 Stack

| Tecnologia | Versão | Papel |
|---|---|---|
| Java | 8 | Linguagem base |
| Spring Boot | 1.5.6 | Framework principal |
| Spring Security | — | Autenticação e autorização |
| Spring Data JPA | — | Persistência e repositórios |
| MySQL | — | Banco de dados em produção |
| H2 | — | Banco em memória para testes |
| Flyway | — | Versionamento e migração do banco |
| JJWT | 0.7.0 | Geração e validação de tokens JWT |
| EhCache | — | Cache de dados em memória |
| Springfox Swagger | 2.6.1 | Documentação automática da API |
| JUnit + Mockito | — | Testes unitários e de integração |
| TravisCI | — | Integração contínua |
| Maven | — | Gerenciador de dependências |

---

## 🏗 Arquitetura

O projeto segue uma estrutura em camadas clássica para APIs Spring Boot:

```
src/main/java/com/michel/pontointeligente/
├── api/
│   ├── controllers/       # Endpoints REST (PF, Empresa, Lançamento, Auth)
│   ├── dtos/              # Objetos de transferência de dados (request/response)
│   ├── response/          # Wrapper genérico de resposta com erros
│   └── security/          # Configuração JWT e filtros de autenticação
├── core/
│   ├── entities/          # Entidades JPA (Funcionario, Empresa, Lancamento)
│   ├── enums/             # Tipos de lançamento e perfil de acesso
│   ├── repositories/      # Interfaces Spring Data JPA
│   └── services/          # Regras de negócio
└── PontoInteligenteApplication.java
```

**Fluxo de autenticação:**
```
Cliente → POST /auth → JWT Token → Header Authorization: Bearer <token> → Endpoints protegidos
```

---

## 🚀 Como executar

### Pré-requisitos

- [Git](https://git-scm.com/)
- [Java 8+](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
- [Maven](https://maven.apache.org/)
- [MySQL](https://www.mysql.com/) (para ambiente de produção)

### Configurando o banco de dados

```sql
CREATE DATABASE ponto_inteligente;
CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON ponto_inteligente.* TO 'root'@'localhost';
```

> O Flyway criará as tabelas automaticamente na primeira execução.

### Rodando localmente

```bash
# Clone o repositório
git clone https://github.com/michelspirlandeli/ponto-inteligente-api.git
cd ponto-inteligente-api

# Execute com Maven
mvn spring-boot:run
```

A API estará disponível em `http://localhost:8080`.

### Rodando os testes

```bash
mvn test
```

> Os testes utilizam o banco H2 em memória — nenhuma configuração adicional necessária.

### Build de produção

```bash
mvn clean package
java -jar target/PontoInteligente-0.0.1-SNAPSHOT.jar
```

### Importando no Eclipse / Spring Tool Suite

```bash
mvn eclipse:eclipse
```

Depois importe como **Existing Maven Project** no Eclipse/STS.

---

## 📡 Endpoints

Com a aplicação rodando, acesse a documentação interativa:

- **Swagger UI:** `http://localhost:8080/swagger-ui.html`

### Principais rotas

| Método | Endpoint | Descrição | Acesso |
|---|---|---|---|
| `POST` | `/auth` | Gera token JWT | Público |
| `POST` | `/api/cadastrar-pf` | Cadastra funcionário PF | Público |
| `POST` | `/api/cadastrar-empresa` | Cadastra empresa | Público |
| `GET` | `/api/funcionarios/{id}` | Busca funcionário por ID | ADMIN |
| `POST` | `/api/lancamentos` | Registra lançamento de ponto | USUARIO |
| `GET` | `/api/lancamentos/funcionario/{id}` | Lista lançamentos por funcionário | Autenticado |
| `PUT` | `/api/lancamentos/{id}` | Atualiza um lançamento | USUARIO |
| `DELETE` | `/api/lancamentos/{id}` | Remove um lançamento | ADMIN |

### Exemplo de autenticação

```bash
# Login
curl -X POST http://localhost:8080/auth \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@empresa.com", "senha": "123456"}'

# Resposta
{
  "data": {
    "token": "eyJhbGciOiJIUzUxMiJ9..."
  },
  "errors": []
}

# Usando o token
curl http://localhost:8080/api/lancamentos/funcionario/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzUxMiJ9..."
```

### Exemplo de lançamento de ponto

```bash
curl -X POST http://localhost:8080/api/lancamentos \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <token>" \
  -d '{
    "data": "2024-01-15 08:00:00",
    "tipo": "INICIO_TRABALHO",
    "funcionarioId": 1
  }'
```

**Tipos de lançamento:** `INICIO_TRABALHO` · `TERMINO_TRABALHO` · `INICIO_ALMOCO` · `TERMINO_ALMOCO`

---

## 🧪 Testes

O projeto possui cobertura com testes unitários e de integração usando JUnit e Mockito:

```
src/test/java/com/michel/pontointeligente/
├── api/controllers/       # Testes de integração dos endpoints (MockMvc)
└── core/services/         # Testes unitários das regras de negócio
```

```bash
# Executar todos os testes
mvn test

# Executar com relatório de cobertura
mvn test jacoco:report
```

---

## ⚙️ Variáveis de ambiente

| Variável | Descrição | Padrão |
|---|---|---|
| `spring.datasource.url` | URL do banco MySQL | `jdbc:mysql://localhost/ponto_inteligente` |
| `spring.datasource.username` | Usuário do banco | `root` |
| `spring.datasource.password` | Senha do banco | `root` |
| `jwt.secret` | Chave secreta para assinar o JWT | — |
| `jwt.expiration` | Expiração do token em segundos | `604800` (7 dias) |

---

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](./LICENSE) para mais detalhes.

---

<p align="center">
  Desenvolvido por <a href="https://www.linkedin.com/in/michel-spirlandeli/">Michel Spirlandeli</a>
</p>
