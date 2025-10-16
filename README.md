# 🍽️ Totem Auto-Atendimento

Um sistema completo de auto-atendimento para restaurantes, desenvolvido com arquitetura moderna e tecnologias robustas. O projeto oferece uma experiência fluida para clientes realizarem pedidos de forma autônoma, com gestão administrativa completa.

## 🏗️ Arquitetura do Sistema

Este é um projeto **fullstack** que implementa:

- **Frontend**: Interface moderna e responsiva para clientes e administradores
- **Backend**: API REST robusta com autenticação JWT
- **Banco de Dados**: Sistema de persistência relacional 
- **Containerização**: Deploy simplificado com Docker

## 🚀 Stack Tecnológica

| Camada | Tecnologia | Versão |
|--------|------------|--------|
| **Frontend** | React + TypeScript | 18+ |
| | Vite | 5+ |
| | TailwindCSS | 3+ |
| | Node.js | 20+ |
| **Backend** | Spring Boot | 3+ |
| | Java | 17 |
| | Maven | 3.8+ |
| **Database** | PostgreSQL | 16+ |
| **Container** | Docker | Latest |
| | Docker Compose | Latest |
| **Auth** | JWT | - |

## 📁 Estrutura do Projeto

```
totem-auto-atendimento/
├── 📁 totem/                    # Backend (Spring Boot)
│   ├── 📁 src/main/java/        # Código fonte Java
│   ├── 📁 src/main/resources/   # Configurações e recursos
│   ├── 📄 pom.xml              # Dependências Maven
│   ├── 📄 Dockerfile           # Container do backend
│   └── 📄 .env.example         # Variáveis de ambiente
├── 📁 totem-front/             # Frontend (React + Vite)
│   ├── 📁 src/                 # Código fonte React/TS
│   ├── 📁 public/              # Assets estáticos
│   ├── 📄 package.json         # Dependências NPM
│   ├── 📄 Dockerfile           # Container do frontend
│   └── 📄 vite.config.ts       # Configuração Vite
├── 📄 docker-compose.yml       # Orquestração dos serviços
├── 📄 init_db.sql             # Schema e dados iniciais
├── 📄 .env.example            # Variáveis do Docker Compose
└── 📄 README.md               # Esta documentação
```

## ⚡ Execução Rápida (Docker)

### Pré-requisitos

- [Docker](https://www.docker.com/get-started/) instalado
- [Docker Compose](https://docs.docker.com/compose/install/) instalado
- Git para clonar o repositório

### 🚀 Inicializando o Sistema

```bash
# Clone o repositório
git clone https://github.com/ArthurJsph/totem-auto-atendimento.git
cd totem-auto-atendimento

# Configure as variáveis de ambiente
cp .env.example .env
cp totem/.env.example totem/.env

# Inicie todos os serviços
docker compose up --build -d
```

### 📱 Acessando a Aplicação

Após a inicialização completa:

- **Frontend (Cliente)**: http://localhost:3000
- **Backend (API)**: http://localhost:8080
- **Banco PostgreSQL**: localhost:5432

### 🛑 Parando os Serviços

```bash
# Parar serviços (mantém dados)
docker compose stop

# Remover containers (mantém volumes)
docker compose down

# Reset completo (⚠️ apaga dados)
docker compose down --volumes --rmi all
```

## 🔧 Desenvolvimento Local

### Backend (Spring Boot)

```bash
cd totem

# Configure variáveis de ambiente
cp .env.example .env
# Edite o arquivo .env com suas configurações

# Execute com Maven
./mvnw spring-boot:run

# Ou compile e execute o JAR
./mvnw clean package -DskipTests
java -jar target/totem-0.0.1-SNAPSHOT.jar
```

### Frontend (React + Vite)

```bash
cd totem-front

# Instale dependências
npm install

# Desenvolvimento com hot reload
npm run dev

# Build para produção
npm run build
```

## 🌐 Configuração de Ambiente

### Variáveis do Docker Compose (`.env`)

```bash
# Banco de Dados
POSTGRES_DB=totem_db
POSTGRES_USER=totem_user
POSTGRES_PASSWORD=totem_pass
POSTGRES_PORT=5432

# Backend
BACKEND_PORT=8080
BACKEND_EXPOSED_PORT=8080

# Frontend  
FRONTEND_PORT=3000
FRONTEND_EXPOSED_PORT=3000
```

### Variáveis do Backend (`totem/.env`)

```bash
# Database
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/totem_db
SPRING_DATASOURCE_USERNAME=totem_user
SPRING_DATASOURCE_PASSWORD=totem_pass
SPRING_DATASOURCE_DRIVER_CLASS_NAME=org.postgresql.Driver

# Server
SERVER_PORT=8080

# JWT
JWT_SECRET=your_jwt_secret_here_minimum_32_characters
JWT_EXPIRATION_TIME=86400000

# Email (para recuperação de senha)
SPRING_MAIL_HOST=smtp.gmail.com
SPRING_MAIL_PORT=587
SPRING_MAIL_USERNAME=your_email@gmail.com
SPRING_MAIL_PASSWORD=your_app_password
```

## � Autenticação e API

### Criando Primeiro Usuário Admin

```bash
# Requisição POST para criar usuário administrador
curl -X POST http://localhost:8080/api/users/save \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Admin User",
    "email": "admin@totem.com",
    "password": "admin123",
    "phone": "11999999999",
    "cpf": "12345678900",
    "role": "ADMIN"
  }'
```

### Realizando Login

```bash
# Obter token JWT
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@totem.com",
    "password": "admin123"
  }'
```

### Usando Token em Requisições

```bash
# Exemplo de requisição autenticada
curl -X GET http://localhost:8080/api/protected-endpoint \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE"
```

## 🗃️ Banco de Dados

O sistema utiliza PostgreSQL com schema pré-configurado que inclui:

- **Usuários**: Sistema de autenticação e autorização
- **Restaurantes**: Cadastro de estabelecimentos
- **Produtos**: Catálogo de itens do cardápio
- **Pedidos**: Gestão completa de orders
- **Pagamentos**: Processamento de transações
- **Tokens**: Sistema de recuperação de senha

### Dados Iniciais

O arquivo `init_db.sql` popula automaticamente:
- 1 restaurante exemplo ("Pizzaria Bella")
- 4 categorias de menu
- 20 produtos variados
- Schema completo com relacionamentos

## 📧 Sistema de Email

Configurado para recuperação de senhas com:
- Tokens temporários com expiração
- Templates de email responsivos
- Suporte SMTP (Gmail, etc.)

## 🔍 Troubleshooting

### Problemas Comuns

**Erro de conexão com banco:**
```bash
# Verifique se o PostgreSQL está rodando
docker compose ps db

# Veja logs do banco
docker compose logs db
```

**Frontend não carrega:**
```bash
# Verifique se o backend está respondendo
curl http://localhost:8080/health

# Verifique logs do frontend
docker compose logs frontend
```

**Problemas de build:**
```bash
# Limpe containers e rebuild
docker compose down
docker system prune -f
docker compose up --build
```

### Logs Úteis

```bash
# Todos os serviços
docker compose logs -f

# Serviço específico
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
```

## 🤝 Contribuindo

### Como Contribuir

1. **Fork** este repositório
2. **Clone** seu fork localmente
3. **Crie** uma branch para sua feature: `git checkout -b feature/nova-funcionalidade`
4. **Commit** suas mudanças: `git commit -m 'feat: adiciona nova funcionalidade'`
5. **Push** para a branch: `git push origin feature/nova-funcionalidade`
6. **Abra** um Pull Request

### Padrões de Commit

Utilizamos [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` nova funcionalidade
- `fix:` correção de bug
- `docs:` documentação
- `style:` formatação
- `refactor:` refatoração de código
- `test:` testes
- `chore:` tarefas de build/config

### Estrutura de Branches

- `master`: código estável em produção
- `develop`: branch de desenvolvimento
- `feature/*`: novas funcionalidades
- `bugfix/*`: correções de bugs
- `hotfix/*`: correções urgentes

## 📋 Roadmap

- [ ] Sistema de notificações push
- [ ] Integração com gateway de pagamento
- [ ] App mobile (React Native)
- [ ] Dashboard de analytics
- [ ] Sistema de avaliações
- [ ] Integração com delivery

## 🐛 Reportando Issues

Encontrou um bug? [Abra uma issue](https://github.com/ArthurJsph/totem-auto-atendimento/issues) incluindo:

- Descrição do problema
- Passos para reproduzir
- Comportamento esperado vs atual
- Screenshots (se aplicável)
- Ambiente (OS, Docker version, etc.)

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👨‍💻 Autor

**Arthur Joseph** - [@ArthurJsph](https://github.com/ArthurJsph)

---

## 📞 Suporte

- 📧 Email: [contato](mailto:arthur@example.com)
- 🐛 Issues: [GitHub Issues](https://github.com/ArthurJsph/totem-auto-atendimento/issues)
- 📖 Wiki: [Documentação Completa](https://github.com/ArthurJsph/totem-auto-atendimento/wiki)

---

<div align="center">

**⭐ Se este projeto te ajudou, considere dar uma estrela!**

[![GitHub stars](https://img.shields.io/github/stars/ArthurJsph/totem-auto-atendimento.svg?style=social&label=Star)](https://github.com/ArthurJsph/totem-auto-atendimento)

</div>
