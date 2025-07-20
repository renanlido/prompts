# CLAUDE.md

## 🎯 Objetivo do Projeto

[Descreva em 1-2 parágrafos o propósito principal do projeto, o problema que resolve e o valor que entrega. Seja claro e direto.]

**Exemplo:**
Este projeto é uma API REST para gerenciamento de e-commerce, permitindo que lojistas criem e gerenciem suas lojas online. A plataforma oferece funcionalidades de catálogo de produtos, gestão de pedidos, pagamentos e logística integrada.

## 🏗️ Arquitetura

### Visão Geral

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Frontend  │────▶│   API       │────▶│  Database   │
│   (React)   │     │   (Node.js) │     │ (PostgreSQL)│
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │   Services  │
                    │ (Redis, S3) │
                    └─────────────┘
```

### Stack Tecnológica

- **Backend**: Node.js v20+, Express/Fastify
- **Database**: PostgreSQL 15+ / MongoDB 6+
- **Cache**: Redis 7+
- **Queue**: Bull/BullMQ
- **Storage**: AWS S3 / MinIO
- **Monitoring**: Prometheus + Grafana
- **CI/CD**: GitHub Actions
- **Testing**: Vitest/Jest, Supertest
- **Linting**: Biome/ESLint

### Estrutura de Pastas

```
src/
├── domain/
│   ├── user/
│   │   ├── infra/
│   │   │   ├── repositories/       # Implementações de repositórios
│   │   │   ├── http/              # Controllers/Routes HTTP
│   │   │   └── persistence/       # Schemas/Models de banco
│   │   ├── application/
│   │   │   ├── use-cases/         # Casos de uso/Services
│   │   │   ├── dtos/              # Data Transfer Objects
│   │   │   └── mappers/           # Mapeadores de dados
│   │   └── entities/
│   │       ├── user.entity.ts     # Entidade de domínio
│   │       ├── value-objects/     # Objetos de valor
│   │       └── interfaces/        # Contratos do domínio
│   │
│   ├── product/
│   │   ├── infra/
│   │   ├── application/
│   │   └── entities/
│   │
│   └── order/
│       ├── infra/
│       ├── application/
│       └── entities/
│
├── shared/
│   ├── errors/                    # Erros customizados globais
│   │   ├── app-error.ts
│   │   └── validation-error.ts
│   ├── utils/                     # Utilitários compartilhados
│   │   ├── date.utils.ts
│   │   └── crypto.utils.ts
│   ├── interfaces/                # Interfaces compartilhadas
│   │   └── repository.interface.ts
│   ├── middleware/                # Middlewares globais
│   │   ├── auth.middleware.ts
│   │   └── error.middleware.ts
│   └── config/                    # Configurações globais
│       ├── database.config.ts
│       └── app.config.ts
│
└── main.ts                        # Entry point da aplicação
```

### Organização por Domínio (DDD)

Cada domínio segue a estrutura:

- **entities/**: Regras de negócio puras, sem dependências externas
- **application/**: Orquestra casos de uso, depende de entities
- **infra/**: Implementações técnicas, adaptadores externos

Exemplo de organização de arquivos:

```
domain/user/
├── entities/
│   ├── user.entity.ts
│   ├── value-objects/
│   │   ├── email.vo.ts
│   │   └── password.vo.ts
│   └── interfaces/
│       └── user-repository.interface.ts
├── application/
│   ├── use-cases/
│   │   ├── create-user.use-case.ts
│   │   ├── authenticate-user.use-case.ts
│   │   └── update-profile.use-case.ts
│   ├── dtos/
│   │   ├── create-user.dto.ts
│   │   └── update-profile.dto.ts
│   └── mappers/
│       └── user.mapper.ts
└── infra/
    ├── repositories/
    │   └── user.repository.ts
    ├── http/
    │   ├── user.controller.ts
    │   └── user.routes.ts
    └── persistence/
        └── user.schema.ts
```

## 📋 Convenções de Código

### Nomenclatura

- **Arquivos de Domínio**:
  - Entities: `[nome].entity.ts` (ex: `user.entity.ts`)
  - Use Cases: `[ação]-[recurso].use-case.ts` (ex: `create-user.use-case.ts`)
  - Repositories: `[nome].repository.ts` (ex: `user.repository.ts`)
  - Value Objects: `[nome].vo.ts` (ex: `email.vo.ts`)
  - DTOs: `[ação]-[recurso].dto.ts` (ex: `create-user.dto.ts`)
- **Arquivos Shared**: `[funcionalidade].[tipo].ts` (ex: `date.utils.ts`, `app.error.ts`)
- **Classes**: `PascalCase` (ex: `UserEntity`, `CreateUserUseCase`)
- **Funções/Variáveis**: `camelCase` (ex: `getUserById`)
- **Constantes**: `UPPER_SNAKE_CASE` (ex: `MAX_RETRY_ATTEMPTS`)
- **Interfaces**: `I[Nome]` ou `[Nome]Interface` (ex: `IUserRepository`, `RepositoryInterface`)
- **Types**: `T[Nome]` ou `[Nome]Type` (ex: `TUserResponse`, `UserResponseType`)
- **Enums**: `E[Nome]` ou `[Nome]Enum` (ex: `EOrderStatus`, `OrderStatusEnum`)
- **Exceções**: `[Nome]Error` (ex: `NotFoundError`, `ValidationError`)

### Padrões de Código

```typescript
// ✅ BOM: Separação clara de responsabilidades (DDD)

// entities/user.entity.ts
export class UserEntity {
  constructor(
    private readonly id: string,
    private readonly email: Email,
    private readonly password: Password
  ) {}

  changePassword(newPassword: Password): void {
    // Regra de negócio aqui
    this.password = newPassword;
  }
}

// application/use-cases/create-user.use-case.ts
export class CreateUserUseCase {
  constructor(
    private readonly userRepository: IUserRepository,
    private readonly hashService: IHashService
  ) {}

  async execute(dto: CreateUserDTO): Promise<UserEntity> {
    const hashedPassword = await this.hashService.hash(dto.password);
    const user = new UserEntity(
      generateId(),
      new Email(dto.email),
      new Password(hashedPassword)
    );
    
    return await this.userRepository.save(user);
  }
}

// ❌ EVITAR: Misturar camadas e responsabilidades
class UserService {
  async createUser(data) {
    // Validação, regra de negócio, persistência, tudo junto
    // 100+ linhas de código...
  }
}
```

### Imports e Organização

```typescript
// ✅ BOM: Imports organizados por camada
// 1. Bibliotecas externas
import { Injectable } from '@nestjs/common';
import { v4 as uuid } from 'uuid';

// 2. Shared/Core
import { AppError } from '@/shared/errors/app-error';
import { Either, left, right } from '@/shared/utils/either';

// 3. Outros domínios (se necessário)
import { ProductEntity } from '@/domain/product/entities/product.entity';

// 4. Mesmo domínio (de fora para dentro)
import { IUserRepository } from '../entities/interfaces/user-repository.interface';
import { UserEntity } from '../entities/user.entity';
import { CreateUserDTO } from '../dtos/create-user.dto';
```

### Git Conventions

- **Branches**: `feature/nome-da-feature`, `fix/nome-do-bug`, `hotfix/critical-fix`
- **Commits**: Conventional Commits
  - `feat:` nova funcionalidade
  - `fix:` correção de bug
  - `docs:` documentação
  - `style:` formatação
  - `refactor:` refatoração
  - `test:` testes
  - `chore:` tarefas

## 🔧 Comandos Úteis

### Gerenciador de Pacotes

> **IMPORTANTE**: Verificar qual gerenciador de pacotes está sendo usado. Procure pelos arquivos de lock na raiz do projeto:
>
> - `yarn.lock` → Use `yarn`
> - `package-lock.json` → Use `npm run`
> - `pnpm-lock.yaml` → Use `pnpm`
> - `bun.lockb` → Use `bun`

### Desenvolvimento

```bash
# Instalar dependências
[PKG_MANAGER] install

# Desenvolvimento local
[PKG_MANAGER] dev

# Build para produção
[PKG_MANAGER] build

# Executar testes
[PKG_MANAGER] test                 # Todos os testes
[PKG_MANAGER] test:unit           # Apenas unitários
[PKG_MANAGER] test:integration    # Apenas integração
[PKG_MANAGER] test:e2e            # End-to-end

# Linting e formatação
[PKG_MANAGER] lint                # Verificar problemas
[PKG_MANAGER] lint:fix            # Corrigir automaticamente
[PKG_MANAGER] format              # Formatar código

# Database
[PKG_MANAGER] db:migrate          # Rodar migrations
[PKG_MANAGER] db:seed             # Popular dados de teste
[PKG_MANAGER] db:reset            # Reset completo
```

**Nota**: Substitua `[PKG_MANAGER]` pelo gerenciador detectado no projeto.

### Docker

```bash
# Build e run
docker-compose up -d

# Logs
docker-compose logs -f api

# Reset completo
docker-compose down -v && docker-compose up -d
```

## ⚠️ Áreas Críticas

### Segurança

- **Autenticação**: JWT com refresh tokens em `src/middleware/auth.ts`
- **Rate Limiting**: Configurado em `src/middleware/rate-limit.ts`
- **Validação**: Sempre use Joi/Zod antes de processar dados
- **SQL Injection**: Use sempre prepared statements

### Performance

- **Database Queries**: Índices críticos em `userId`, `createdAt`, `status`
- **Caching**: Redis para sessões e dados frequentes
- **Paginação**: Limite máximo de 100 items por página
- **File Upload**: Limite de 10MB, processamento assíncrono

### Áreas Sensíveis

```
src/
├── domain/
│   ├── payment/          # ⚠️ Processamento de pagamentos
│   │   ├── entities/     # ⚠️ Regras críticas de cobrança
│   │   └── infra/        # ⚠️ Integrações com gateways
│   ├── auth/             # ⚠️ Autenticação e autorização  
│   │   ├── entities/     # ⚠️ Tokens e sessões
│   │   └── application/  # ⚠️ Fluxos de autenticação
│   └── billing/          # ⚠️ Faturamento e cobrança
│       └── application/  # ⚠️ Cálculos financeiros
└── shared/
    └── utils/
        └── crypto/       # ⚠️ Criptografia de dados
```

## 📚 Recursos e Documentação

### Documentação Interna

- **API Docs**: <http://localhost:3000/api-docs> (Swagger)
- **Architecture Decision Records**: `/docs/adr/`
- **Database Schema**: `/docs/database/schema.md`
- **Deployment Guide**: `/docs/deployment.md`

### Ambientes

- **Local**: <http://localhost:3000>
- **Staging**: <https://staging.example.com>
- **Production**: <https://api.example.com>

### Serviços Externos

- **Sentry**: [Dashboard](https://sentry.io/organizations/example)
- **Datadog**: [Monitoring](https://app.datadoghq.com)
- **AWS Console**: [Resources](https://console.aws.amazon.com)

### Links Úteis

- **Jira Board**: [PROJ-123](https://example.atlassian.net)
- **Confluence**: [Wiki](https://example.confluence.com)
- **Figma**: [Design System](https://figma.com/file/xxx)

## 🚀 Setup Inicial

### Pré-requisitos

- Node.js 20+
- PostgreSQL 15+ / MongoDB 6+
- Redis 7+
- Docker & Docker Compose (opcional)

### Detecção Automática de Configuração

```bash
# O projeto pode usar diferentes ferramentas. Verifique:

# Gerenciador de pacotes (ordem de prioridade)
ls -la | grep -E "(bun.lockb|pnpm-lock.yaml|yarn.lock|package-lock.json)"

# Linter/Formatter
ls -la | grep -E "(biome.json|.eslintrc|.prettierrc)"

# Test runner
cat package.json | grep -E "(vitest|jest)"
```

### Instalação Rápida

```bash
# 1. Clone o repositório
git clone https://github.com/empresa/projeto.git
cd projeto

# 2. Detecte o gerenciador de pacotes
if [ -f "bun.lockb" ]; then
    echo "Using Bun"
    bun install
elif [ -f "pnpm-lock.yaml" ]; then
    echo "Using PNPM"
    pnpm install
elif [ -f "yarn.lock" ]; then
    echo "Using Yarn"
    yarn install
elif [ -f "package-lock.json" ]; then
    echo "Using NPM"
    npm install
else
    echo "No lock file found, using NPM by default"
    npm install
fi

# 3. Configure ambiente
cp .env.example .env
# Edite .env com suas configurações

# 4. Setup database (substitua [PKG_MANAGER] pelo detectado)
[PKG_MANAGER] db:create
[PKG_MANAGER] db:migrate
[PKG_MANAGER] db:seed

# 5. Inicie o servidor
[PKG_MANAGER] dev
```

### Verificação

```bash
# Health check
curl http://localhost:3000/health

# API Version
curl http://localhost:3000/api/v1/version
```

## 🧪 Estratégia de Testes

### Pirâmide de Testes

```
        /\
       /e2e\      (5%)  - Fluxos críticos
      /------\
     /integr. \   (25%) - APIs e integrações
    /----------\
   /   unit     \ (70%) - Lógica de negócio
  /--------------\
```

### Cobertura Mínima

- **Global**: 80%
- **Services**: 90%
- **Controllers**: 70%
- **Utils**: 95%

### Executar Testes

```bash
# Com coverage
[PKG_MANAGER] test:coverage

# Watch mode
[PKG_MANAGER] test:watch

# Específico
[PKG_MANAGER] test -- user.service.test.js
```

## 🐛 Debugging

### Logs

- **Desenvolvimento**: Console colorido com timestamp
- **Produção**: JSON estruturado para parsing

```javascript
// Níveis de log
logger.error('Payment failed', { orderId, error });
logger.warn('Rate limit approaching', { userId });
logger.info('Order created', { orderId, total });
logger.debug('Cache miss', { key });
```

### Debug Mode

```bash
# Node.js debug
[PKG_MANAGER] dev:debug

# VS Code: Use configuração em .vscode/launch.json

# Chrome DevTools
node --inspect src/server.js
```

## 📈 Métricas e Monitoramento

### KPIs Principais

- **Uptime**: > 99.9%
- **Response Time**: p95 < 200ms
- **Error Rate**: < 0.1%
- **Throughput**: 1000 req/s

### Alertas Configurados

- CPU > 80% por 5 minutos
- Memory > 85%
- Error rate > 1%
- Response time p95 > 500ms

### Dashboards

- [System Overview](http://grafana.local/d/system)
- [API Performance](http://grafana.local/d/api)
- [Business Metrics](http://grafana.local/d/business)

## 👥 Time e Contatos

### Responsáveis

- **Tech Lead**: @fulano (Slack: #tech-lead)
- **Backend**: @ciclano (Slack: #backend)
- **DevOps**: @beltrano (Slack: #devops)

### Canais de Comunicação

- **Slack**: #projeto-dev (desenvolvimento)
- **Slack**: #projeto-alerts (alertas de produção)
- **Email**: <projeto-team@empresa.com>

### On-call

- **PagerDuty**: [Escalation Policy](https://empresa.pagerduty.com)
- **Runbook**: `/docs/runbook/`

## 🔄 Processo de Desenvolvimento

### Workflow

1. **Planning**: Segunda-feira, 10h
2. **Daily**: 9h30 (15 min)
3. **Review**: Sexta-feira, 14h
4. **Retrospective**: Última sexta do mês

### Definition of Done

- [ ] Código revisado (mínimo 1 aprovação)
- [ ] Testes escritos e passando
- [ ] Documentação atualizada
- [ ] Sem débitos de segurança
- [ ] Performance validada
- [ ] Deploy em staging testado

### Deploy

```bash
# Staging (automático em merge para develop)
git push origin develop

# Production (manual após aprovação)
[PKG_MANAGER] deploy:production
```

## 🚨 Troubleshooting

### Problemas Comuns

#### Database Connection Failed

```bash
# Verificar se PostgreSQL está rodando
sudo systemctl status postgresql

# Verificar conexão
psql -h localhost -U postgres -d projeto_dev
```

#### Redis Connection Failed

```bash
# Verificar se Redis está rodando
redis-cli ping

# Limpar cache se necessário
redis-cli FLUSHDB
```

#### Port Already in Use

```bash
# Encontrar processo
lsof -i :3000

# Matar processo
kill -9 [PID]
```

#### Package Manager Issues

```bash
# Lock file desatualizado
rm -rf node_modules
rm [package-lock.json|yarn.lock|pnpm-lock.yaml|bun.lockb]
[PKG_MANAGER] install

# Cache corrompido
npm cache clean --force       # NPM
yarn cache clean              # Yarn
pnpm store prune             # PNPM
bun pm cache rm              # Bun
```

## 📝 Notas Importantes

### ⚡ Performance Tips

- Use `.lean()` em queries Mongoose quando não precisar de hydration
- Implemente paginação cursor-based para grandes datasets
- Use projeção em queries para buscar apenas campos necessários

### 🔒 Security Reminders

- Nunca commite `.env` ou secrets
- Sempre valide e sanitize inputs
- Use rate limiting em todas as rotas públicas
- Mantenha dependências atualizadas

### 💡 Best Practices

- Prefira composition over inheritance
- Use early returns para reduzir nesting
- Mantenha funções pequenas (< 20 linhas ideal)
- Um arquivo = uma responsabilidade

### 🏛️ Arquitetura DDD (Domain-Driven Design)

#### Princípios por Camada

**Entities (Domínio Puro)**

- Sem dependências de frameworks
- Regras de negócio isoladas
- Imutabilidade quando possível
- Value Objects para conceitos importantes

**Application (Casos de Uso)**

- Orquestra entidades e infraestrutura
- Não contém regras de negócio
- Retorna DTOs, não entidades
- Um caso de uso = uma transação

**Infrastructure (Adaptadores)**

- Implementações concretas
- Dependências de frameworks aqui
- Mappers para converter dados
- Isolamento de detalhes técnicos

#### Fluxo de Dependências

```
HTTP Request
    ↓
Controller (infra/http)
    ↓
Use Case (application)
    ↓
Entity (entities)
    ↓
Repository Interface (entities/interfaces)
    ↓
Repository Implementation (infra/repositories)
    ↓
Database
```

#### Exemplo de Isolamento

```typescript
// ❌ ERRADO: Entity dependendo de infraestrutura
// entities/user.entity.ts
import { Column } from 'typeorm'; // NÃO!

// ✅ CORRETO: Entity pura
// entities/user.entity.ts
export class UserEntity {
  // Sem decorators de ORM
  // Sem dependências externas
}

// infra/persistence/user.schema.ts
import { Entity, Column } from 'typeorm';
// Decorators e ORM ficam aqui
```

### 🛠️ Configurações Específicas por Gerenciador

#### NPM

```json
{
  "engines": {
    "node": ">=20.0.0",
    "npm": ">=10.0.0"
  }
}
```

#### Yarn

```yaml
# .yarnrc.yml
nodeLinker: node-modules
enableGlobalCache: true
```

#### PNPM

```yaml
# .npmrc
shamefully-hoist=true
strict-peer-dependencies=false
```

#### Bun

```toml
# bunfig.toml
[install]
production = false
frozen-lockfile = false
```

---

**Última atualização**: [DATA]
**Versão do documento**: 1.1.0
