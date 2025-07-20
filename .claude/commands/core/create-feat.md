# CREATE FEATURE (DESENVOLVIMENTO RÁPIDO)

Desenvolva a feature $ARGUMENTS de forma rápida e eficiente, com foco em entregar valor funcional rapidamente, adicionando testes após a implementação.

## FASE 1: ANÁLISE RÁPIDA E DESIGN

### 1.1 Definição da Feature

```markdown
## Feature: [Nome da Feature]

### Objetivo
[Uma frase descrevendo o que a feature faz e por quê]

### Escopo
✅ **Incluído**:
- [Funcionalidade principal 1]
- [Funcionalidade principal 2]
- [Funcionalidade principal 3]

❌ **Fora do Escopo** (próxima iteração):
- [Funcionalidade futura 1]
- [Funcionalidade futura 2]

### MVP (Minimum Viable Product)
1. [Funcionalidade essencial 1]
2. [Funcionalidade essencial 2]
3. [Funcionalidade essencial 3]
```

### 1.2 Design Rápido

```javascript
// Estrutura proposta (alto nível)

// [ADAPT] Para outras linguagens:
// PHP: use Laravel/Symfony patterns (routes, controllers, models)
// Java: use Spring Boot patterns (@Controller, @Service, @Entity)
// Python: use Django/FastAPI patterns (views, serializers, models)
// Go: use Gin/Echo patterns (handlers, services, repositories)

// API/Interface principal
POST   /api/[feature]     // Criar
GET    /api/[feature]/:id // Buscar
PUT    /api/[feature]/:id // Atualizar  
DELETE /api/[feature]/:id // Deletar
GET    /api/[feature]     // Listar

// Estrutura de dados
{
  "id": "uuid",
  "field1": "string",
  "field2": "number",
  "status": "active|inactive",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}

// Fluxo principal
1. Cliente → Request
2. Validação básica
3. Processamento
4. Persistência
5. Response
```

### 1.3 Decisões Técnicas Rápidas

```markdown
## Stack e Ferramentas

### Escolhas
- **Framework**: [Express/Fastify/etc]
- **Database**: [PostgreSQL/MongoDB/etc]
- **Validação**: [Joi/Yup/Zod]
- **ORM/ODM**: [Prisma/TypeORM/Mongoose]

// [ADAPT] Para outras linguagens:
// PHP: Framework (Laravel/Symfony), ORM (Eloquent/Doctrine), Validation (FormRequest/Symfony Validator)
// Java: Framework (Spring Boot), ORM (JPA/Hibernate), Validation (Bean Validation)
// Python: Framework (Django/FastAPI), ORM (Django ORM/SQLAlchemy), Validation (Pydantic/Marshmallow)
// Go: Framework (Gin/Echo), ORM (GORM), Validation (validator package)

### Padrões a Seguir
- [ ] Estrutura MVC/Clean Architecture existente
- [ ] Convenções de nomenclatura do projeto
- [ ] Error handling padrão
- [ ] Logging padrão

### Reutilizar
- Middleware de autenticação: `authMiddleware`
- Validação base: `BaseValidator`
- Error handler: `errorHandler`
- Logger: `logger`
```

## FASE 2: IMPLEMENTAÇÃO RÁPIDA

### 2.1 Estrutura de Arquivos

```bash
# Criar estrutura rapidamente
src/
├── features/
│   └── [feature-name]/
│       ├── [feature-name].controller.js
│       ├── [feature-name].service.js
│       ├── [feature-name].repository.js
│       ├── [feature-name].validator.js
│       ├── [feature-name].model.js
│       └── index.js
```

### 2.2 Model/Schema

```javascript
// [feature-name].model.js

// Mongoose example
const FeatureSchema = new Schema({
  field1: {
    type: String,
    required: true,
    trim: true,
    maxLength: 100
  },
  field2: {
    type: Number,
    required: true,
    min: 0,
    max: 1000
  },
  status: {
    type: String,
    enum: ['active', 'inactive'],
    default: 'active'
  },
  userId: {
    type: Schema.Types.ObjectId,
    ref: 'User',
    required: true
  }
}, {
  timestamps: true
});

// Indexes para performance
FeatureSchema.index({ userId: 1, status: 1 });
FeatureSchema.index({ createdAt: -1 });

// Prisma example
model Feature {
  id        String   @id @default(uuid())
  field1    String   @db.VarChar(100)
  field2    Int
  status    Status   @default(ACTIVE)
  userId    String
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([userId, status])
  @@index([createdAt])
}
```

### 2.3 Controller (Entry Point)

```javascript
// [feature-name].controller.js

class FeatureController {
  constructor(featureService) {
    this.featureService = featureService;
  }

  // CREATE
  create = async (req, res, next) => {
    try {
      const { userId } = req.user;
      const data = { ...req.body, userId };
      
      const result = await this.featureService.create(data);
      
      res.status(201).json({
        success: true,
        data: result
      });
    } catch (error) {
      next(error);
    }
  };

  // READ
  findById = async (req, res, next) => {
    try {
      const { id } = req.params;
      const { userId } = req.user;
      
      const result = await this.featureService.findById(id, userId);
      
      res.json({
        success: true,
        data: result
      });
    } catch (error) {
      next(error);
    }
  };

  // UPDATE
  update = async (req, res, next) => {
    try {
      const { id } = req.params;
      const { userId } = req.user;
      
      const result = await this.featureService.update(id, req.body, userId);
      
      res.json({
        success: true,
        data: result
      });
    } catch (error) {
      next(error);
    }
  };

  // DELETE
  delete = async (req, res, next) => {
    try {
      const { id } = req.params;
      const { userId } = req.user;
      
      await this.featureService.delete(id, userId);
      
      res.status(204).send();
    } catch (error) {
      next(error);
    }
  };

  // LIST
  list = async (req, res, next) => {
    try {
      const { userId } = req.user;
      const { page = 1, limit = 20, ...filters } = req.query;
      
      const result = await this.featureService.list({
        userId,
        page: parseInt(page),
        limit: parseInt(limit),
        filters
      });
      
      res.json({
        success: true,
        data: result.data,
        pagination: result.pagination
      });
    } catch (error) {
      next(error);
    }
  };
}

module.exports = FeatureController;
```

### 2.4 Service (Business Logic)

```javascript
// [feature-name].service.js

class FeatureService {
  constructor(featureRepository, validator) {
    this.repository = featureRepository;
    this.validator = validator;
  }

  async create(data) {
    // Validação
    const validated = await this.validator.validateCreate(data);
    
    // Regras de negócio
    if (await this.isDuplicate(validated)) {
      throw new ConflictError('Feature already exists');
    }
    
    // Processamento adicional
    const processed = this.processData(validated);
    
    // Persistir
    const created = await this.repository.create(processed);
    
    // Pós-processamento (se necessário)
    await this.postCreate(created);
    
    return this.serialize(created);
  }

  async findById(id, userId) {
    const feature = await this.repository.findById(id);
    
    if (!feature) {
      throw new NotFoundError('Feature not found');
    }
    
    // Verificar permissão
    if (feature.userId !== userId) {
      throw new ForbiddenError('Access denied');
    }
    
    return this.serialize(feature);
  }

  async update(id, data, userId) {
    // Buscar existente
    const existing = await this.findById(id, userId);
    
    // Validar mudanças
    const validated = await this.validator.validateUpdate(data);
    
    // Aplicar regras de negócio
    const processed = this.processUpdate(existing, validated);
    
    // Atualizar
    const updated = await this.repository.update(id, processed);
    
    return this.serialize(updated);
  }

  async delete(id, userId) {
    // Verificar existência e permissão
    await this.findById(id, userId);
    
    // Soft delete ou hard delete
    await this.repository.softDelete(id);
    
    // Limpar cache, notificar, etc
    await this.postDelete(id);
  }

  async list({ userId, page, limit, filters }) {
    // Construir query
    const query = {
      userId,
      ...this.buildFilters(filters)
    };
    
    // Buscar com paginação
    const { data, total } = await this.repository.findMany({
      query,
      page,
      limit,
      sort: { createdAt: -1 }
    });
    
    return {
      data: data.map(this.serialize),
      pagination: {
        page,
        limit,
        total,
        pages: Math.ceil(total / limit)
      }
    };
  }

  // Métodos auxiliares
  private async isDuplicate(data) {
    const existing = await this.repository.findOne({
      field1: data.field1,
      userId: data.userId
    });
    return !!existing;
  }

  private processData(data) {
    return {
      ...data,
      field1: data.field1.trim().toLowerCase()
    };
  }

  private serialize(feature) {
    return {
      id: feature.id,
      field1: feature.field1,
      field2: feature.field2,
      status: feature.status,
      createdAt: feature.createdAt,
      updatedAt: feature.updatedAt
    };
  }
}

module.exports = FeatureService;
```

### 2.5 Repository (Data Access)

```javascript
// [feature-name].repository.js

class FeatureRepository {
  constructor(model) {
    this.model = model;
  }

  async create(data) {
    return await this.model.create(data);
  }

  async findById(id) {
    return await this.model.findById(id);
  }

  async findOne(query) {
    return await this.model.findOne(query);
  }

  async findMany({ query, page, limit, sort }) {
    const skip = (page - 1) * limit;
    
    const [data, total] = await Promise.all([
      this.model
        .find(query)
        .sort(sort)
        .skip(skip)
        .limit(limit)
        .lean(),
      this.model.countDocuments(query)
    ]);
    
    return { data, total };
  }

  async update(id, data) {
    return await this.model.findByIdAndUpdate(
      id,
      { $set: data },
      { new: true, runValidators: true }
    );
  }

  async softDelete(id) {
    return await this.model.findByIdAndUpdate(
      id,
      { 
        $set: { 
          status: 'inactive',
          deletedAt: new Date()
        }
      }
    );
  }

  async hardDelete(id) {
    return await this.model.findByIdAndDelete(id);
  }
}

module.exports = FeatureRepository;
```

### 2.6 Validação

```javascript
// [feature-name].validator.js

const Joi = require('joi');

class FeatureValidator {
  constructor() {
    this.createSchema = Joi.object({
      field1: Joi.string()
        .required()
        .trim()
        .max(100)
        .pattern(/^[a-zA-Z0-9\s]+$/)
        .messages({
          'string.pattern.base': 'field1 must contain only letters, numbers and spaces'
        }),
      field2: Joi.number()
        .required()
        .min(0)
        .max(1000),
      userId: Joi.string()
        .required()
    });

    this.updateSchema = Joi.object({
      field1: Joi.string()
        .trim()
        .max(100)
        .pattern(/^[a-zA-Z0-9\s]+$/),
      field2: Joi.number()
        .min(0)
        .max(1000),
      status: Joi.string()
        .valid('active', 'inactive')
    }).min(1); // Pelo menos um campo
  }

  async validateCreate(data) {
    const { error, value } = this.createSchema.validate(data, {
      abortEarly: false,
      stripUnknown: true
    });

    if (error) {
      throw new ValidationError(
        'Validation failed',
        error.details.map(d => ({
          field: d.path.join('.'),
          message: d.message
        }))
      );
    }

    return value;
  }

  async validateUpdate(data) {
    const { error, value } = this.updateSchema.validate(data, {
      abortEarly: false,
      stripUnknown: true
    });

    if (error) {
      throw new ValidationError(
        'Validation failed',
        error.details.map(d => ({
          field: d.path.join('.'),
          message: d.message
        }))
      );
    }

    return value;
  }
}

module.exports = FeatureValidator;
```

## FASE 3: INTEGRAÇÃO RÁPIDA

### 3.1 Rotas

```javascript
// [feature-name].routes.js

const express = require('express');
const router = express.Router();

// Importar dependências
const FeatureController = require('./[feature-name].controller');
const FeatureService = require('./[feature-name].service');
const FeatureRepository = require('./[feature-name].repository');
const FeatureValidator = require('./[feature-name].validator');
const FeatureModel = require('./[feature-name].model');

// Middleware
const { authenticate } = require('../../middleware/auth');
const { rateLimiter } = require('../../middleware/rate-limit');

// Instanciar
const repository = new FeatureRepository(FeatureModel);
const validator = new FeatureValidator();
const service = new FeatureService(repository, validator);
const controller = new FeatureController(service);

// Aplicar middleware global
router.use(authenticate);
router.use(rateLimiter);

// Definir rotas
router.post('/', controller.create);
router.get('/', controller.list);
router.get('/:id', controller.findById);
router.put('/:id', controller.update);
router.delete('/:id', controller.delete);

module.exports = router;
```

### 3.2 Registro no App

```javascript
// app.js ou server.js

// Importar rotas da feature
const featureRoutes = require('./features/[feature-name]/[feature-name].routes');

// Registrar no app
app.use('/api/[feature-name]', featureRoutes);
```

## FASE 4: TESTES RÁPIDOS

### 4.1 Testes de Smoke (Básicos)

```javascript
// [feature-name].test.js

describe('[FeatureName] - Smoke Tests', () => {
  let request;
  let authToken;
  
  beforeAll(async () => {
    request = supertest(app);
    authToken = await getTestAuthToken();
  });

  describe('POST /api/[feature]', () => {
    it('deve criar recurso com dados válidos', async () => {
      const response = await request
        .post('/api/[feature]')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          field1: 'Test Value',
          field2: 123
        })
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data).toHaveProperty('id');
      expect(response.body.data.field1).toBe('test value'); // processado
    });

    it('deve retornar erro 400 com dados inválidos', async () => {
      const response = await request
        .post('/api/[feature]')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          field1: '' // vazio
        })
        .expect(400);

      expect(response.body.success).toBe(false);
      expect(response.body.errors).toBeInstanceOf(Array);
    });
  });

  describe('GET /api/[feature]/:id', () => {
    it('deve retornar recurso existente', async () => {
      // Criar primeiro
      const created = await createTestFeature();
      
      const response = await request
        .get(`/api/[feature]/${created.id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);

      expect(response.body.data.id).toBe(created.id);
    });

    it('deve retornar 404 para ID inexistente', async () => {
      await request
        .get('/api/[feature]/invalid-id')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(404);
    });
  });
});
```

### 4.2 Testes Manuais Rápidos

```bash
# Criar arquivo de testes manuais
# manual-tests.http ou use Postman/Insomnia

### Login
POST http://localhost:3000/api/auth/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "password123"
}

### Criar Feature
POST http://localhost:3000/api/[feature]
Authorization: Bearer {{authToken}}
Content-Type: application/json

{
  "field1": "Test Value",
  "field2": 123
}

### Listar Features
GET http://localhost:3000/api/[feature]?page=1&limit=10
Authorization: Bearer {{authToken}}

### Buscar Feature
GET http://localhost:3000/api/[feature]/{{featureId}}
Authorization: Bearer {{authToken}}

### Atualizar Feature
PUT http://localhost:3000/api/[feature]/{{featureId}}
Authorization: Bearer {{authToken}}
Content-Type: application/json

{
  "field1": "Updated Value"
}

### Deletar Feature
DELETE http://localhost:3000/api/[feature]/{{featureId}}
Authorization: Bearer {{authToken}}
```

## FASE 5: DEPLOYMENT RÁPIDO

### 5.1 Checklist Pré-Deploy

```markdown
## Deploy Checklist

### Code
- [ ] Feature funcionando localmente
- [ ] Sem erros de linting
- [ ] Logs apropriados adicionados
- [ ] Variáveis de ambiente configuradas

### Database
- [ ] Migrations criadas e testadas
- [ ] Índices necessários criados
- [ ] Backup antes de deploy

### Tests
- [ ] Smoke tests passando
- [ ] Testes manuais realizados
- [ ] Performance aceitável

### Documentation
- [ ] README atualizado
- [ ] API docs básica criada
- [ ] Changelog atualizado
```

### 5.2 Deploy Script

```bash
#!/bin/bash
# deploy-feature.sh

echo "🚀 Deploying [feature-name]..."

# 1. Run tests
npm test -- [feature-name] || exit 1

# 2. Build
npm run build || exit 1

# 3. Database migrations
npm run migrate:up || exit 1

# 4. Deploy to staging
git push staging main

# 5. Smoke test staging
curl -f https://staging.app.com/api/[feature]/health || exit 1

# 6. Deploy to production
git push production main

echo "✅ Deploy completed!"
```

## FASE 6: MONITORAMENTO E ITERAÇÃO

### 6.1 Logs e Métricas

```javascript
// Adicionar logs estratégicos
logger.info('Feature created', {
  featureId: created.id,
  userId: data.userId,
  duration: Date.now() - startTime
});

logger.error('Feature creation failed', {
  error: error.message,
  stack: error.stack,
  data: sanitizedData
});

// Métricas customizadas
metrics.increment('feature.created');
metrics.histogram('feature.create.duration', duration);
```

### 6.2 Próximas Iterações

```markdown
## Roadmap da Feature

### v1.0 (Current) ✅
- CRUD básico
- Validações essenciais
- Autenticação

### v1.1 (Next Sprint)
- [ ] Busca avançada
- [ ] Filtros complexos
- [ ] Export CSV/PDF
- [ ] Webhook notifications

### v1.2 (Future)
- [ ] Batch operations
- [ ] Real-time updates
- [ ] Analytics dashboard
- [ ] API rate limiting por tier

### Tech Debt para resolver
- [ ] Adicionar testes unitários completos
- [ ] Otimizar queries N+1
- [ ] Implementar cache
- [ ] Melhorar error handling
```

## EXEMPLO DE USO

```bash
# Usar o comando
claude-code /create-feat "User Preferences API"

# Feature será criada com:
# - CRUD completo funcional
# - Validações básicas
# - Autenticação
# - Estrutura escalável
# - Pronta para deploy

# Tempo estimado: 1-3 horas
```

## DICAS DE PRODUTIVIDADE

### Do's ✅

- Comece com o mínimo viável
- Reutilize código existente
- Copie e adapte de features similares
- Use geradores/snippets
- Foque no happy path primeiro
- Deploy cedo e itere

### Don'ts ❌

- Não over-engineer
- Não optimize prematuramente
- Não adicione features não pedidas
- Não perca tempo em nomes perfeitos
- Não espere 100% de cobertura inicial
- Não atrase o deploy por perfeição

### Atalhos Úteis

```bash
# Gerar estrutura rapidamente
npx create-feature [name]

# Copiar de feature similar
cp -r src/features/existing-feature src/features/new-feature
find src/features/new-feature -type f -exec sed -i 's/existing/new/g' {} \;

# Testar rapidamente
npm run dev & 
sleep 5
curl -X POST localhost:3000/api/[feature] -d '{...}'
```

## Adaptações por Linguagem

### JavaScript/TypeScript

- **Test Runner**: Jest, Vitest, Mocha
- **Linter**: ESLint, Biome
- **Build**: Webpack, Vite, esbuild
- **Package Manager**: npm, yarn, pnpm, bun

### PHP

- **Test Runner**: PHPUnit, Pest
- **Linter**: PHPStan, Psalm, PHPCS
- **Build**: Composer scripts
- **Package Manager**: Composer

### Java

- **Test Runner**: JUnit, TestNG
- **Linter**: SpotBugs, PMD, Checkstyle
- **Build**: Maven, Gradle
- **Package Manager**: Maven, Gradle

### Python

- **Test Runner**: pytest, unittest
- **Linter**: pylint, flake8, mypy, ruff
- **Build**: setuptools, poetry build
- **Package Manager**: pip, poetry, pipenv

### Go

- **Test Runner**: go test, testify
- **Linter**: golangci-lint, go vet
- **Build**: go build
- **Package Manager**: go mod

### Ruby

- **Test Runner**: RSpec, Minitest
- **Linter**: RuboCop, Reek
- **Build**: rake build
- **Package Manager**: Bundler

### C#/.NET

- **Test Runner**: xUnit, NUnit, MSTest
- **Linter**: Roslyn Analyzers
- **Build**: dotnet build
- **Package Manager**: NuGet

### Rust

- **Test Runner**: cargo test
- **Linter**: clippy, rustfmt
- **Build**: cargo build
- **Package Manager**: Cargo
