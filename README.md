# Finance Control API

Uma API REST completa para controle financeiro pessoal, desenvolvida com Spring Boot. Permite que usuários autenticados cadastrem receitas e despesas, visualizem saldos e gerem relatórios mensais, com foco em boas práticas de desenvolvimento e arquitetura limpa.

## 🚀 Funcionalidades

- **Autenticação JWT**: Login e registro seguro com tokens JWT
- **CRUD de Transações**: Criar, ler, atualizar e deletar transações financeiras
- **Controle de Saldo**: Cálculo automático de saldo baseado em receitas e despesas
- **Relatórios Mensais**: Geração de relatórios por período
- **Filtragem Avançada**: Busca por categoria, data e tipo de transação
- **Paginação**: Suporte a paginação para listagem de transações
- **Segurança**: Cada usuário acessa apenas seus próprios dados
- **Documentação**: API documentada com Swagger/OpenAPI

## 🛠️ Tecnologias Utilizadas

- **Java 17**
- **Spring Boot 3.x**
- **Spring Security** (JWT)
- **Spring Data JPA**
- **H2 Database** (desenvolvimento)
- **PostgreSQL** (produção)
- **Maven**
- **Lombok**
- **Bean Validation**
- **Swagger/OpenAPI**
- **JUnit 5** (testes)
- **Mockito** (testes)

## 📁 Estrutura do Projeto

```
src/
├── main/
│   ├── java/com/brenodev/finance/
│   │   ├── FinanceControlApiApplication.java
│   │   ├── controller/
│   │   │   ├── AuthController.java
│   │   │   └── TransactionController.java
│   │   ├── dto/
│   │   │   ├── request/
│   │   │   │   ├── TransactionRequest.java
│   │   │   │   ├── UserLoginRequest.java
│   │   │   │   └── UserRegisterRequest.java
│   │   │   └── response/
│   │   │       ├── BalanceResponse.java
│   │   │       ├── TransactionResponse.java
│   │   │       └── UserResponse.java
│   │   ├── entity/
│   │   │   ├── Transaction.java
│   │   │   └── User.java
│   │   ├── enums/
│   │   │   ├── Category.java
│   │   │   ├── Role.java
│   │   │   └── TransactionType.java
│   │   ├── exception/
│   │   │   └── GlobalExceptionHandler.java
│   │   ├── repository/
│   │   │   ├── TransactionRepository.java
│   │   │   └── UserRepository.java
│   │   ├── security/
│   │   │   ├── JwtAuthenticationFilter.java
│   │   │   ├── JwtUtil.java
│   │   │   └── SecurityConfig.java
│   │   ├── service/
│   │   │   ├── TransactionService.java
│   │   │   └── UserService.java
│   │   └── config/
│   │       └── SwaggerConfig.java
│   └── resources/
│       └── application.yml
└── test/
    └── java/com/brenodev/finance/service/
        ├── TransactionServiceTest.java
        └── UserServiceTest.java
```

## ⚙️ Como Executar

### Pré-requisitos

- Java 17 ou superior
- Maven 3.6+
- PostgreSQL (para produção) ou H2 (para desenvolvimento)

### Configuração do Banco de Dados

#### Desenvolvimento (H2)
O projeto está configurado para usar H2 por padrão. O banco será criado automaticamente.

#### Produção (PostgreSQL)
1. Crie um banco de dados PostgreSQL
2. Configure as variáveis de ambiente:
   ```bash
   export DB_USERNAME=seu_usuario
   export DB_PASSWORD=sua_senha
   export JWT_SECRET=seu_secret_jwt
   ```
3. Execute com perfil de produção:
   ```bash
   mvn spring-boot:run -Dspring-boot.run.profiles=prod
   ```

### Executando a Aplicação

1. Clone o repositório
2. Navegue até o diretório do projeto
3. Execute:
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

A aplicação estará disponível em: http://localhost:8080

## 📚 Documentação da API

### Swagger UI
Acesse a documentação interativa em: http://localhost:8080/swagger-ui.html

### H2 Console (Desenvolvimento)
Acesse o console do banco H2 em: http://localhost:8080/h2-console

## 🔐 Autenticação

### Registro de Usuário
```http
POST /api/v1/auth/register
Content-Type: application/json

{
  "nome": "Breno Barros",
  "email": "breno@example.com",
  "senha": "senha123"
}
```

### Login
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "breno@example.com",
  "senha": "senha123"
}
```

**Resposta:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "user": {
    "id": 1,
    "nome": "Breno Barros",
    "email": "breno@example.com",
    "role": "USER"
  }
}
```

## 📋 Endpoints Principais

### Transações

Todos os endpoints de transação requerem autenticação (Bearer Token no header Authorization).

#### Criar Transação
```http
POST /api/v1/transactions
Authorization: Bearer {token}

{
  "description": "Salário",
  "value": 3000.00,
  "type": "RECEITA",
  "category": "OUTROS",
  "date": "2024-01-15"
}
```

#### Listar Transações
```http
GET /api/v1/transactions?page=0&size=10
Authorization: Bearer {token}
```

#### Buscar por Categoria
```http
GET /api/v1/transactions/category/ALIMENTACAO
Authorization: Bearer {token}
```

#### Buscar por Período
```http
GET /api/v1/transactions/period?startDate=2024-01-01&endDate=2024-01-31
Authorization: Bearer {token}
```

#### Obter Saldo
```http
GET /api/v1/transactions/balance
Authorization: Bearer {token}
```

**Resposta:**
```json
{
  "totalReceitas": 5000.00,
  "totalDespesas": 1500.00,
  "saldo": 3500.00
}
```

#### Relatório Mensal
```http
GET /api/v1/transactions/report/monthly?year=2024&month=1
Authorization: Bearer {token}
```

## 🧪 Testes

Execute os testes com:
```bash
mvn test
```

## 📦 Build e Deploy

### Build
```bash
mvn clean package
```

### Executar JAR
```bash
java -jar target/finance-control-api-0.0.1-SNAPSHOT.jar
```

## 🔒 Segurança

- **JWT Tokens**: Autenticação stateless com tokens JWT
- **BCrypt**: Senhas criptografadas com BCrypt
- **CORS**: Configurado para permitir requisições do frontend
- **Validação**: Bean Validation em todos os DTOs
- **Tratamento de Exceções**: Global exception handler com respostas padronizadas

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## 📞 Contato

**Breno Dev** - breno.dev@example.com

Link do Projeto: [https://github.com/brenodev/finance-control-api](https://github.com/brenodev/finance-control-api)

---

⭐ **Dê uma estrela se este projeto te ajudou!**
