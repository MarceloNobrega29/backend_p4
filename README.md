# 🏥 Voll.med API - Sistema de Gestão Médica

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Security](https://img.shields.io/badge/Spring%20Security-6.x-green.svg)](https://spring.io/projects/spring-security)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-blue.svg)](https://www.mysql.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> API RESTful para gestão de clínicas médicas desenvolvida com Spring Boot, implementando autenticação JWT e Spring Security. Projeto acadêmico da disciplina de **Tecnologia para Back-End Avançado**.

---

## 📋 Sobre o Projeto

O **Voll.med API** é uma API REST completa para gerenciamento de clínicas médicas, permitindo o cadastro e controle de médicos, pacientes, consultas e usuários do sistema.

### 🎓 Contexto Acadêmico

Este projeto foi desenvolvido como trabalho da disciplina **Tecnologia para Back-End Avançado**, ministrada pelo professor **Antonio Junio Figueiredo da Mata**, com foco em:

- 🔐 **Spring Security** - Implementação completa de autenticação e autorização
- 🎯 **JWT (JSON Web Tokens)** - Autenticação stateless
- 🏗️ **Arquitetura REST** - Boas práticas e padrões RESTful
- 🗄️ **JPA/Hibernate** - Persistência de dados com ORM
- ✅ **Bean Validation** - Validação robusta de dados
- 🔄 **Migrations** - Controle de versão do banco com Flyway

### ✨ Funcionalidades Principais

- 🔐 **Autenticação e Autorização** com Spring Security e JWT
- 👨‍⚕️ **Gestão de Médicos** - CRUD completo com especialidades
- 👥 **Gestão de Pacientes** - Cadastro e controle de pacientes
- 📅 **Agendamento de Consultas** - Sistema de marcação e controle
- 🔒 **Segurança** - Senhas criptografadas com BCrypt
- ✅ **Validações** - Bean Validation para dados consistentes
- 📄 **Paginação** - Listagens otimizadas com Spring Data
- 🗄️ **Migrations** - Controle de versão do banco com Flyway

---

## 🚀 Tecnologias Utilizadas

### Backend
- **Java 21** - Linguagem principal com recursos modernos
- **Spring Boot 3.x** - Framework base
- **Spring Security 6.x** - Autenticação e autorização robusta
- **Spring Data JPA** - Persistência de dados
- **JWT (Auth0)** - Tokens de autenticação stateless
- **Bean Validation** - Validação de dados
- **Lombok** - Redução de boilerplate

### Banco de Dados
- **MySQL 8.0** - Banco de produção
- **Flyway** - Migrations e versionamento

### Ferramentas
- **Maven** - Gerenciamento de dependências
- **Docker** - Containerização da aplicação
- **Git** - Controle de versão

---

## 📦 Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- [Java JDK 21+](https://www.oracle.com/java/technologies/downloads/)
- [Maven 3.8+](https://maven.apache.org/download.cgi)
- [MySQL 8.0+](https://dev.mysql.com/downloads/mysql/)
- [Docker](https://www.docker.com/get-started) (Opcional)
- [Git](https://git-scm.com/downloads)

---

## 🔧 Instalação e Configuração

### 1. Clone o Repositório

```bash
git clone https://github.com/MarceloNobrega29/backend_p4.git
cd backend_p4
```

### 2. Configure o Banco de Dados

Crie o banco de dados no MySQL:

```sql
CREATE DATABASE p4_backend;
```

### 3. Configure as Variáveis de Ambiente

Edite o arquivo `src/main/resources/application.properties`:

```properties
# Banco de Dados
spring.datasource.url=jdbc:mysql://localhost:3306/p4_backend
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha

# JWT Secret
api.security.token.secret=sua_chave_secreta_jwt
```

### 4. Execute as Migrations

As migrations do Flyway serão executadas automaticamente ao iniciar a aplicação.

### 5. Compile e Execute

```bash
# Compilar o projeto
mvn clean install

# Executar a aplicação
mvn spring-boot:run
```

A aplicação estará disponível em: `http://localhost:8080`

---

## 🐳 Executando com Docker

### Usando Docker Compose (Recomendado)

Crie um arquivo `docker-compose.yml` na raiz do projeto:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: vollmed-mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: p4_backend
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - vollmed-network

  app:
    build: .
    container_name: vollmed-api
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: prod
      DATASOURCE_URL: jdbc:mysql://mysql:3306/p4_backend
      DATASOURCE_USERNAME: root
      DATASOURCE_PASSWORD: root
    depends_on:
      - mysql
    networks:
      - vollmed-network

volumes:
  mysql-data:

networks:
  vollmed-network:
    driver: bridge
```

Execute com:

```bash
# Iniciar todos os serviços
docker-compose up -d

# Ver logs
docker-compose logs -f app

# Parar serviços
docker-compose down
```

### Usando Docker Manual

```bash
# Build da imagem
docker build -t vollmed-api .

# Executar container
docker run -p 8080:8080 \
  -e DATASOURCE_URL=jdbc:mysql://host.docker.internal:3306/p4_backend \
  -e DATASOURCE_USERNAME=root \
  -e DATASOURCE_PASSWORD=senha \
  vollmed-api
```

### Criar Dockerfile

Crie um arquivo `Dockerfile` na raiz do projeto:

```dockerfile
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN apk add --no-cache maven
RUN mvn clean package -DskipTests

FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 📚 Documentação da API

### Autenticação

#### Cadastrar Usuário
```http
POST /cadastros
Content-Type: application/json

{
  "login": "usuario@email.com",
  "senha": "senha123"
}
```

#### Login
```http
POST /login
Content-Type: application/json

{
  "login": "usuario@email.com",
  "senha": "senha123"
}
```

**Resposta:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Endpoints Protegidos

> **⚠️ Nota:** Todos os endpoints abaixo requerem autenticação via Bearer Token

**Header necessário:**
```http
Authorization: Bearer {seu_token_jwt}
```

### Médicos

#### Listar Médicos
```http
GET /medicos?page=0&size=10
```

#### Cadastrar Médico
```http
POST /medicos
Content-Type: application/json

{
  "nome": "Dr. João Silva",
  "email": "joao.silva@email.com",
  "crm": "123456",
  "especialidade": "CARDIOLOGIA",
  "endereco": {
    "logradouro": "Rua das Flores",
    "numero": "100",
    "bairro": "Centro",
    "cidade": "São Paulo",
    "uf": "SP",
    "cep": "01234-567"
  }
}
```

#### Atualizar Médico
```http
PUT /medicos
Content-Type: application/json

{
  "id": 1,
  "nome": "Dr. João Silva Atualizado",
  "telefone": "(11) 98765-4321"
}
```

#### Excluir Médico (Exclusão Lógica)
```http
DELETE /medicos/{id}
```

### Pacientes

#### Listar Pacientes
```http
GET /pacientes?page=0&size=10
```

#### Cadastrar Paciente
```http
POST /pacientes
Content-Type: application/json

{
  "nome": "Maria Santos",
  "email": "maria@email.com",
  "cpf": "123.456.789-00",
  "telefone": "(11) 91234-5678",
  "endereco": {
    "logradouro": "Av. Principal",
    "numero": "500",
    "bairro": "Jardim",
    "cidade": "São Paulo",
    "uf": "SP",
    "cep": "12345-678"
  }
}
```

---

## 🗂️ Estrutura do Projeto

```
backend_p4/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── med/voll/api/
│   │   │       ├── controller/          # Controllers REST
│   │   │       ├── domain/              # Entidades e regras de negócio
│   │   │       │   ├── medico/
│   │   │       │   ├── paciente/
│   │   │       │   ├── consulta/
│   │   │       │   └── usuario/
│   │   │       ├── infra/               # Infraestrutura
│   │   │       │   ├── exception/       # Tratamento de exceções
│   │   │       │   └── security/        # Configurações de segurança
│   │   │       └── ApiApplication.java
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── application-prod.properties
│   │       └── db/migration/            # Migrations Flyway
│   └── test/                            # Testes unitários e integração
├── target/                              # Build da aplicação
├── docker-compose.yml                   # Configuração Docker Compose
├── Dockerfile                           # Imagem Docker da aplicação
├── pom.xml                              # Dependências Maven
└── README.md
```

---

## 🔐 Segurança

### Configuração do Spring Security

O projeto implementa segurança robusta baseada em JWT utilizando **Spring Security 6.x**:

- **Endpoints Públicos:** `/login`, `/cadastros`
- **Endpoints Protegidos:** Todos os demais requerem autenticação
- **Filtro de Segurança:** `SecurityFilter` valida tokens JWT em cada requisição
- **Criptografia:** BCrypt para hash de senhas
- **Stateless:** Sessões não são mantidas no servidor

### Arquitetura de Segurança

```
Cliente → SecurityFilter → Validação JWT → Controller → Service → Repository
           ↓ (se inválido)
         HTTP 403 Forbidden
```

### Componentes de Segurança

1. **SecurityConfiguration** - Configuração principal do Spring Security
2. **SecurityFilter** - Filtro customizado para validação de tokens
3. **TokenService** - Geração e validação de tokens JWT
4. **AuthenticationService** - Serviço de autenticação de usuários

### Geração de Token JWT

```java
// Token válido por 2 horas
// Algoritmo: HMAC256
// Claim: login do usuário
```

---

## 🧪 Testes

Execute os testes com:

```bash
# Todos os testes
mvn test

# Testes específicos
mvn test -Dtest=MedicoControllerTest
```

---

## 📊 Deploy em Produção

### Gerar JAR

```bash
mvn clean package -DskipTests
```

O JAR será gerado em: `target/api-0.0.1-SNAPSHOT.jar`

### Executar em Produção

```bash
java -Dspring.profiles.active=prod \
     -DDATASOURCE_URL=jdbc:mysql://localhost:3306/p4_backend \
     -DDATASOURCE_USERNAME=usuario \
     -DDATASOURCE_PASSWORD=senha \
     -jar target/api-0.0.1-SNAPSHOT.jar
```

---

## 🐛 Troubleshooting

### Erro 403 Forbidden

Verifique se o endpoint está liberado no `SecurityConfiguration`:

```java
.requestMatchers(HttpMethod.POST, "/cadastros").permitAll()
```

### Erro de Conexão com Banco

Confirme se o MySQL está rodando e as credenciais estão corretas:

```bash
mysql -u root -p
SHOW DATABASES;
```

### Token JWT Inválido

Verifique:
- Token expirado (válido por 2 horas)
- Secret key configurada corretamente
- Header `Authorization: Bearer {token}`

---

## 📝 Boas Práticas Implementadas

✅ **Clean Code** - Código limpo e legível  
✅ **SOLID** - Princípios de design orientado a objetos  
✅ **DTOs** - Separação entre entidades e dados de transferência  
✅ **Repository Pattern** - Abstração da camada de dados  
✅ **Exception Handling** - Tratamento centralizado de erros  
✅ **Validations** - Validações em múltiplas camadas  
✅ **Security Best Practices** - Autenticação e autorização robustas

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Para contribuir:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/NovaFuncionalidade`)
3. Commit suas mudanças (`git commit -m 'Add: Nova funcionalidade'`)
4. Push para a branch (`git push origin feature/NovaFuncionalidade`)
5. Abra um Pull Request

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## 👨‍💻 Autor

**Marcelo Nobrega**

- GitHub: [@MarceloNobrega29](https://github.com/MarceloNobrega29)
- LinkedIn: [Marcelo Nobrega](https://www.linkedin.com/in/marcelo-n%C3%B3brega-8046752ba/)

---

## 🎓 Informações Acadêmicas

**Disciplina:** Tecnologia para Back-End Avançado  
**Professor:** Antonio Junio Figueiredo da Mata  
**Instituição:** UNIESP - Cabedelo  
**Período:** 4° Período 

### 📚 Conceitos Abordados

- Spring Security e autenticação JWT
- Arquitetura RESTful
- Persistência com JPA/Hibernate
- Migrations com Flyway
- Validações e tratamento de exceções
- Boas práticas de desenvolvimento back-end

---

## 📧 Contato

Para dúvidas ou sugestões, entre em contato:

- Email: nobregamf29@hotmail.com
- Issues: [GitHub Issues](https://github.com/MarceloNobrega29/backend_p4/issues)

---

<div align="center">

**Desenvolvido com ❤️ e ☕ por Marcelo Nobrega**

⭐ Se este projeto te ajudou, considere dar uma estrela!

</div>