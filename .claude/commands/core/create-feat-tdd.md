# CREATE FEATURE COM TDD (TEST-DRIVEN DEVELOPMENT)

Desenvolva a feature $ARGUMENTS seguindo rigorosamente a metodologia TDD (Red-Green-Refactor), garantindo 100% de cobertura desde o início.

## FASE 1: DESCOBERTA E PLANEJAMENTO

### 1.1 Análise de Requisitos

```markdown
## Feature: [Nome da Feature]

### User Story
Como [tipo de usuário]
Eu quero [ação/funcionalidade]
Para que [benefício/valor]

### Critérios de Aceitação
- [ ] Dado [contexto inicial], quando [ação], então [resultado esperado]
- [ ] Dado [contexto inicial], quando [ação], então [resultado esperado]
- [ ] Dado [contexto inicial], quando [ação], então [resultado esperado]

### Requisitos Técnicos
- [ ] [Requisito técnico 1]
- [ ] [Requisito técnico 2]
- [ ] [Requisito técnico 3]

### Edge Cases Identificados
1. [Cenário extremo 1]
2. [Cenário extremo 2]
3. [Cenário de erro 1]
4. [Cenário de erro 2]
```

### 1.2 Design da API/Interface

```typescript
// Interface pública da feature
interface [FeatureName] {
  // Métodos principais
  method1(param: Type): ReturnType;
  method2(param: Type): Promise<ReturnType>;
  
  // Eventos (se aplicável)
  on(event: 'eventName', handler: Handler): void;
}

// DTOs/Types
interface [FeatureName]Input {
  field1: Type;
  field2: Type;
}

interface [FeatureName]Output {
  field1: Type;
  field2: Type;
}

// Errors específicos
class [FeatureName]ValidationError extends Error {}
class [FeatureName]NotFoundError extends Error {}
```

### 1.3 Arquitetura e Dependências

```markdown
## Arquitetura

### Camadas
- **Controller/Handler**: Recebe requisições e retorna respostas
- **Service/UseCase**: Lógica de negócio principal
- **Repository/Gateway**: Acesso a dados externos
- **Domain/Model**: Entidades e regras de domínio

### Dependências
- Externa: [biblioteca/serviço]
- Interna: [módulo existente]
- Database: [tabelas/collections]

### Padrões a Seguir
- [ ] [Padrão 1 do projeto]
- [ ] [Padrão 2 do projeto]
```

## FASE 2: TDD CYCLE - RED

### 2.1 Estrutura de Testes

```javascript
// [feature-name].test.js
describe('[FeatureName]', () => {
  // Setup comum
  let sut; // System Under Test
  let mockDependency1;
  let mockDependency2;

  beforeEach(() => {
    // Arrange comum para todos os testes
    mockDependency1 = createMock();
    mockDependency2 = createMock();
    sut = new FeatureName(mockDependency1, mockDependency2);
  });

  afterEach(() => {
    // Cleanup
    jest.clearAllMocks();
  });

  describe('Caso de Uso Principal', () => {
    // Testes do happy path
  });

  describe('Validações', () => {
    // Testes de validação de input
  });

  describe('Edge Cases', () => {
    // Testes de cenários extremos
  });

  describe('Error Handling', () => {
    // Testes de tratamento de erros
  });
});
```

### 2.2 Primeiro Teste (Red)

```javascript
describe('Caso de Uso Principal', () => {
  it('deve [comportamento esperado] quando [condição]', async () => {
    // Arrange
    const input = {
      field1: 'value1',
      field2: 'value2'
    };
    const expectedOutput = {
      result: 'expected'
    };

    // Act
    const result = await sut.execute(input);

    // Assert
    expect(result).toEqual(expectedOutput);
  });
});

// 🔴 EXPECTATIVA: Este teste DEVE falhar
// Error: Cannot read property 'execute' of undefined
```

### 2.3 Lista de Testes Planejados

```markdown
## Test List (TDD)

### ✅ Happy Path
- [ ] deve criar [entidade] com dados válidos
- [ ] deve atualizar [entidade] existente
- [ ] deve retornar [entidade] quando existe
- [ ] deve listar [entidades] com paginação

### 🛡️ Validações
- [ ] deve rejeitar quando [campo] está vazio
- [ ] deve rejeitar quando [campo] tem formato inválido
- [ ] deve rejeitar quando [campo] excede limite

### 🔄 Edge Cases
- [ ] deve lidar com [condição extrema 1]
- [ ] deve processar corretamente [limite máximo]
- [ ] deve funcionar com [entrada mínima]

### ❌ Error Cases
- [ ] deve lançar erro quando [dependência] falha
- [ ] deve fazer rollback quando [operação] falha parcialmente
- [ ] deve logar erro e retornar mensagem apropriada
```

## FASE 3: TDD CYCLE - GREEN

### 3.1 Implementação Mínima

```javascript
// [feature-name].js

class FeatureName {
  constructor(dependency1, dependency2) {
    this.dependency1 = dependency1;
    this.dependency2 = dependency2;
  }

  async execute(input) {
    // Implementação MÍNIMA para fazer o teste passar
    return { result: 'expected' };
  }
}

// 🟢 OBJETIVO: Fazer o teste passar com código MÍNIMO
// Não adicione funcionalidade que não está sendo testada!
```

### 3.2 Evolução Incremental

```javascript
// Adicione um teste por vez e implemente o mínimo necessário

// Teste 2: Validação
it('deve validar input obrigatório', () => {
  // 🔴 Red: Escreva o teste
  expect(() => sut.execute({}))
    .toThrow('field1 is required');
});

// 🟢 Green: Implemente apenas a validação
async execute(input) {
  if (!input.field1) {
    throw new Error('field1 is required');
  }
  return { result: 'expected' };
}
```

### 3.3 Ciclo Completo

```markdown
## TDD Cycle Checklist

Para CADA funcionalidade:

1. **🔴 RED**
   - [ ] Escrever teste que falha
   - [ ] Executar e ver falhar
   - [ ] Verificar que falha pelo motivo certo

2. **🟢 GREEN**
   - [ ] Escrever código MÍNIMO
   - [ ] Fazer teste passar
   - [ ] Não adicionar extras

3. **🔄 REFACTOR**
   - [ ] Melhorar código mantendo testes verdes
   - [ ] Eliminar duplicação
   - [ ] Melhorar nomes
   - [ ] Extrair métodos se necessário
```

## FASE 4: TDD CYCLE - REFACTOR

### 4.1 Identificar Code Smells

```javascript
// Antes da refatoração - código funciona mas tem problemas
async execute(input) {
  // ❌ Validação misturada com lógica
  if (!input.field1) throw new Error('field1 is required');
  if (!input.field2) throw new Error('field2 is required');
  if (input.field1.length > 100) throw new Error('field1 too long');
  
  // ❌ Lógica complexa inline
  const processed = input.field1
    .toLowerCase()
    .trim()
    .replace(/[^a-z0-9]/g, '');
    
  // ❌ Múltiplas responsabilidades
  const saved = await this.dependency1.save(processed);
  await this.dependency2.notify(saved);
  
  return { result: saved };
}
```

### 4.2 Aplicar Refatoração

```javascript
// Depois da refatoração - código limpo e testável
class FeatureName {
  async execute(input) {
    const validatedInput = this.validate(input);
    const processedData = this.processData(validatedInput);
    const result = await this.saveAndNotify(processedData);
    return { result };
  }

  private validate(input) {
    const errors = [];
    
    if (!input.field1) errors.push('field1 is required');
    if (!input.field2) errors.push('field2 is required');
    if (input.field1?.length > 100) errors.push('field1 too long');
    
    if (errors.length > 0) {
      throw new ValidationError(errors);
    }
    
    return input;
  }

  private processData(input) {
    return {
      ...input,
      field1: this.sanitize(input.field1)
    };
  }

  private sanitize(value) {
    return value
      .toLowerCase()
      .trim()
      .replace(/[^a-z0-9]/g, '');
  }

  private async saveAndNotify(data) {
    const saved = await this.dependency1.save(data);
    await this.dependency2.notify(saved);
    return saved;
  }
}
```

### 4.3 Testes Após Refatoração

```javascript
// ✅ Todos os testes continuam passando
// ✅ Adicione testes para métodos extraídos se públicos
describe('sanitize', () => {
  it.each([
    ['Hello World!', 'helloworld'],
    ['  spaces  ', 'spaces'],
    ['123-ABC', '123abc'],
    ['special@#$chars', 'specialchars']
  ])('deve sanitizar "%s" para "%s"', (input, expected) => {
    expect(sut.sanitize(input)).toBe(expected);
  });
});
```

## FASE 5: TESTES DE INTEGRAÇÃO

### 5.1 Setup de Integração

```javascript
// [feature-name].integration.test.js
describe('[FeatureName] Integration Tests', () => {
  let app;
  let database;
  
  beforeAll(async () => {
    // Setup real dependencies
    database = await setupTestDatabase();
    app = await setupApp({ database });
  });

  afterAll(async () => {
    await database.close();
    await app.close();
  });

  beforeEach(async () => {
    await database.clear();
    await database.seed();
  });

  describe('API Endpoints', () => {
    it('deve criar recurso via POST /api/[resource]', async () => {
      // Teste end-to-end real
      const response = await request(app)
        .post('/api/resource')
        .send({ field1: 'value1', field2: 'value2' })
        .expect(201);

      expect(response.body).toMatchObject({
        id: expect.any(String),
        field1: 'value1',
        field2: 'value2',
        createdAt: expect.any(String)
      });

      // Verificar no banco
      const saved = await database.find('resource', response.body.id);
      expect(saved).toBeTruthy();
    });
  });
});
```

### 5.2 Testes de Contrato

```javascript
describe('Contract Tests', () => {
  it('deve manter contrato com consumidores', async () => {
    const result = await sut.execute(validInput);
    
    // Verificar estrutura do response
    expect(result).toMatchSchema({
      type: 'object',
      required: ['id', 'status', 'data'],
      properties: {
        id: { type: 'string', format: 'uuid' },
        status: { type: 'string', enum: ['success', 'pending'] },
        data: {
          type: 'object',
          required: ['field1', 'field2']
        }
      }
    });
  });
});
```

## FASE 6: DOCUMENTAÇÃO E ENTREGA

### 6.1 Documentação da Feature

```markdown
## Feature: [Nome]

### Overview
[Descrição breve da feature e seu propósito]

### API Reference
```typescript
// Exemplo de uso
const feature = new FeatureName(dep1, dep2);
const result = await feature.execute({
  field1: 'value1',
  field2: 'value2'
});
```

### Endpoints (se aplicável)

- `POST /api/[resource]` - Criar novo recurso
- `GET /api/[resource]/:id` - Buscar recurso
- `PUT /api/[resource]/:id` - Atualizar recurso
- `DELETE /api/[resource]/:id` - Remover recurso

### Validações

- `field1`: obrigatório, string, máx 100 caracteres
- `field2`: obrigatório, número, entre 0 e 1000

### Erros Possíveis

- `400 ValidationError`: Input inválido
- `404 NotFoundError`: Recurso não encontrado
- `409 ConflictError`: Recurso já existe
- `500 InternalError`: Erro interno

```

### 6.2 Métricas de Qualidade
```markdown
## Quality Metrics

### Cobertura de Testes
- Statements: 100% ✅
- Branches: 100% ✅
- Functions: 100% ✅
- Lines: 100% ✅

### Complexidade
- Complexidade Ciclomática: 4 (✅ < 10)
- Depth: 2 (✅ < 4)

### Performance
- Tempo médio de resposta: 45ms
- Memória: 12MB
- Throughput: 1000 req/s

### Checklist Final
- [ ] Todos os testes passando
- [ ] 100% de cobertura
- [ ] Documentação completa
- [ ] Code review aprovado
- [ ] CI/CD configurado
```

## COMANDOS E WORKFLOW

### Comandos TDD

```bash
# Rodar testes em watch mode durante desenvolvimento
npm test -- --watch [feature-name]

# Verificar cobertura
npm test -- --coverage [feature-name]

# Rodar apenas testes unitários
npm test:unit

# Rodar testes de integração
npm test:integration

# Rodar todos os testes
npm test:all
```

### Workflow Resumido

```markdown
## TDD Workflow

1. **Entender** → Requisitos e critérios de aceitação
2. **Planejar** → Lista de testes necessários
3. **Red** → Escrever teste que falha
4. **Green** → Código mínimo para passar
5. **Refactor** → Melhorar mantendo verde
6. **Repetir** → Próximo teste da lista
7. **Integrar** → Testes de integração
8. **Documentar** → API e exemplos
9. **Entregar** → PR com 100% cobertura

Tempo estimado: 
- Feature pequena: 2-4 horas
- Feature média: 1-2 dias
- Feature grande: 3-5 dias
```

## EXEMPLO COMPLETO

```javascript
// Exemplo: Feature de cálculo de desconto

// [ADAPT] Para outras linguagens:
// PHP: use PHPUnit, classes com namespaces
// Java: use JUnit/TestNG, annotations @Test
// Python: use pytest/unittest, métodos test_*
// Go: use testing package, functions TestXxx

// 1. RED - Teste falha
describe('DiscountCalculator', () => {
  it('deve aplicar 10% de desconto para clientes VIP', () => {
    const calculator = new DiscountCalculator();
    const result = calculator.calculate({
      price: 100,
      customerType: 'VIP'
    });
    expect(result).toBe(90);
  });
});

// 2. GREEN - Implementação mínima
class DiscountCalculator {
  calculate({ price, customerType }) {
    if (customerType === 'VIP') {
      return price * 0.9;
    }
    return price;
  }
}

// 3. REFACTOR - Melhorar código
class DiscountCalculator {
  private readonly discountRules = {
    VIP: 0.10,
    REGULAR: 0,
    NEW: 0.05
  };

  calculate({ price, customerType }) {
    const discountRate = this.discountRules[customerType] || 0;
    return price * (1 - discountRate);
  }
}

// 4. Mais testes...
it('deve aplicar 5% para novos clientes', () => {
  // ... continuar o ciclo
});
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
