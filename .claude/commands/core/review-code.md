# CODE REVIEW INTELIGENTE

Realize uma revisão abrangente e sistemática do código em $ARGUMENTS, identificando problemas de segurança, qualidade e manutenibilidade.

## FASE 1: ANÁLISE DE SEGURANÇA (Prioridade CRÍTICA)

### 1.1 Vulnerabilidades de Injeção

- [ ] **SQL Injection**

  ```javascript
  // ❌ VULNERÁVEL
  query(`SELECT * FROM users WHERE id = ${userId}`)
  
  // ✅ SEGURO
  query('SELECT * FROM users WHERE id = ?', [userId])
  ```

- [ ] **NoSQL Injection**

  ```javascript
  // ❌ VULNERÁVEL
  db.find({ user: req.body.user })
  
  // ✅ SEGURO
  db.find({ user: sanitize(req.body.user) })
  ```

- [ ] **Command Injection**

  ```javascript
  // ❌ VULNERÁVEL
  exec(`ls ${userInput}`)
  
  // ✅ SEGURO
  execFile('ls', [userInput])
  ```

### 1.2 XSS (Cross-Site Scripting)

- [ ] **Reflected XSS**
- [ ] **Stored XSS**
- [ ] **DOM-based XSS**

  ```javascript
  // ❌ VULNERÁVEL
  element.innerHTML = userContent
  
  // ✅ SEGURO
  element.textContent = userContent
  // ou
  element.innerHTML = DOMPurify.sanitize(userContent)
  ```

### 1.3 Autenticação e Autorização

- [ ] **Authentication Bypass**
- [ ] **Broken Access Control**
- [ ] **Session Management**

  ```javascript
  // Verificar:
  - Tokens seguros e com expiração
  - Verificação de permissões em TODAS as rotas
  - Proteção contra CSRF
  - Rate limiting implementado
  ```

### 1.4 Exposição de Dados Sensíveis

- [ ] **Hardcoded Secrets**

  ```bash
  # Buscar por:
  grep -r "password\|secret\|key\|token" --include="*.js" .
  ```

- [ ] **Logs com dados sensíveis**
- [ ] **Error messages revelando informações**
- [ ] **Dados sensíveis em URLs**

### 1.5 Dependências Vulneráveis

```bash
# Verificar vulnerabilidades conhecidas
npm audit
# ou
yarn audit

# Verificar licenças
license-checker --summary
```

### 1.6 Classificação de Segurança

Para cada vulnerabilidade encontrada:

```markdown
**ID**: SEC-001
**Tipo**: [SQL Injection]
**Severidade**: CRITICAL
**Localização**: [arquivo:linha]
**Impacto**: [descrição do risco]
**Correção**:
```[código corrigido]```
**Referência**: [OWASP link]
```

## FASE 2: ANÁLISE DE QUALIDADE DE CÓDIGO

### 2.1 Complexidade

- [ ] **Complexidade Ciclomática**

  ```javascript
  // Meta: < 10 por função
  // Tool: eslint-plugin-complexity
  
  // ❌ COMPLEXO (CC > 10)
  function processData(data) {
    if (condition1) {
      if (condition2) {
        // múltiplos níveis aninhados
      }
    }
  }
  
  // ✅ SIMPLES
  function processData(data) {
    if (!isValid(data)) return;
    return transform(data);
  }
  ```

- [ ] **Profundidade de Aninhamento**
  - Máximo: 4 níveis
  - Ideal: 2 níveis

### 2.2 Duplicação de Código

- [ ] **DRY Violations**

  ```javascript
  // Tool: jscpd
  // Threshold: < 5% duplicação
  ```

- [ ] **Padrões repetidos**
- [ ] **Lógica similar em múltiplos lugares**

### 2.3 Princípios SOLID

- [ ] **Single Responsibility**

  ```javascript
  // ❌ Múltiplas responsabilidades
  class UserService {
    validateEmail() {}
    saveToDatabase() {}
    sendEmail() {}
    generateReport() {}
  }
  
  // ✅ Responsabilidade única
  class UserValidator {}
  class UserRepository {}
  class EmailService {}
  ```

- [ ] **Open/Closed**
- [ ] **Liskov Substitution**
- [ ] **Interface Segregation**
- [ ] **Dependency Inversion**

### 2.4 Cobertura de Testes

```bash
# Verificar cobertura
npm test -- --coverage

# Metas:
- Statements: > 80%
- Branches: > 80%
- Functions: > 80%
- Lines: > 80%
```

### 2.5 Performance

- [ ] **Big-O Analysis**

  ```javascript
  // ❌ O(n²)
  users.forEach(user => {
    orders.forEach(order => {
      if (order.userId === user.id) {}
    })
  })
  
  // ✅ O(n)
  const ordersByUser = _.groupBy(orders, 'userId');
  users.forEach(user => {
    const userOrders = ordersByUser[user.id] || [];
  })
  ```

- [ ] **Query Optimization**
  - N+1 queries
  - Missing indexes
  - Unnecessary joins

- [ ] **Memory Leaks**
  - Event listeners não removidos
  - Closures retendo referências
  - Timers não limpos

## FASE 3: ANÁLISE DE MANUTENIBILIDADE

### 3.1 Documentação

- [ ] **Funções Públicas Documentadas**

  ```javascript
  /**
   * Processa dados do usuário e retorna formato normalizado
   * @param {Object} userData - Dados brutos do usuário
   * @param {string} userData.email - Email do usuário
   * @param {Object} options - Opções de processamento
   * @returns {Object} Dados normalizados
   * @throws {ValidationError} Se dados inválidos
   */
  function processUser(userData, options = {}) {}
  ```

- [ ] **README Atualizado**
- [ ] **Decisões de Arquitetura Documentadas**
- [ ] **APIs com exemplos**

### 3.2 Nomenclatura

- [ ] **Variáveis Descritivas**

  ```javascript
  // ❌ Ruim
  const d = new Date();
  const u = users.filter(x => x.a > 18);
  
  // ✅ Bom
  const currentDate = new Date();
  const adultUsers = users.filter(user => user.age > 18);
  ```

- [ ] **Funções com Verbos**
- [ ] **Classes como Substantivos**
- [ ] **Booleanos com is/has/can**

### 3.3 Estrutura

- [ ] **Arquivos < 300 linhas**
- [ ] **Funções < 50 linhas**
- [ ] **Parâmetros < 3 (ideal)**
- [ ] **Imports organizados**

### 3.4 Tratamento de Erros

- [ ] **Errors Específicos**

  ```javascript
  // ❌ Genérico
  throw new Error('Invalid');
  
  // ✅ Específico
  throw new ValidationError('Email format invalid', {
    field: 'email',
    value: email,
    pattern: EMAIL_REGEX
  });
  ```

- [ ] **Try/Catch apropriados**
- [ ] **Errors logados com contexto**
- [ ] **Graceful degradation**

## FASE 4: RELATÓRIO DE FINDINGS

### 4.1 Template de Finding

```markdown
### [SEVERITY] - [Título do Problema]

**Localização**: `src/file.js:42-45`
**Categoria**: [Security|Quality|Maintainability]
**Impacto**: [Descrição do impacto no sistema/usuários]

**Problema**:
```javascript
// Código problemático
```

**Solução Recomendada**:

```javascript
// Código corrigido
```

**Justificativa**: [Por que isso é importante]
**Referências**: [Links para documentação/standards]

```

### 4.2 Sumário Executivo
```markdown
## Code Review Summary

**Arquivos Revisados**: X
**Linhas de Código**: Y
**Tempo de Review**: Z minutos

### Findings por Severidade
- 🚨 **CRITICAL**: 0
- ⚠️  **HIGH**: 0
- 🔶 **MEDIUM**: 0
- 📝 **LOW**: 0

### Áreas de Destaque
✅ **Pontos Positivos**:
- [Boa prática encontrada]
- [Código bem estruturado]

⚠️  **Áreas de Melhoria**:
- [Principal preocupação]
- [Sugestão de refatoração]

### Métricas de Qualidade
- Complexidade média: X
- Cobertura de testes: Y%
- Duplicação: Z%
```

## FASE 5: RECOMENDAÇÕES ACIONÁVEIS

### 5.1 Quick Wins (< 1 hora)

1. [ ] [Correção simples com alto impacto]
2. [ ] [Melhoria de nomenclatura]
3. [ ] [Adicionar validação faltante]

### 5.2 Melhorias Médias (1-4 horas)

1. [ ] [Refatoração de função complexa]
2. [ ] [Adicionar testes faltantes]
3. [ ] [Implementar error handling]

### 5.3 Melhorias Maiores (> 4 horas)

1. [ ] [Reestruturação de módulo]
2. [ ] [Implementar nova arquitetura]
3. [ ] [Migração de dependências]

## FASE 6: AUTOMAÇÃO E PREVENÇÃO

### 6.1 Configurações de Linting

```javascript
// .eslintrc.js recomendado
{
  "extends": ["eslint:recommended", "plugin:security/recommended"],
  "plugins": ["security", "complexity"],
  "rules": {
    "complexity": ["error", 10],
    "max-depth": ["error", 4],
    "max-lines": ["error", 300],
    "max-params": ["error", 3]
  }
}
```

### 6.2 Pre-commit Hooks

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged && npm test"
    }
  },
  "lint-staged": {
    "*.js": ["eslint --fix", "prettier --write"]
  }
}
```

### 6.3 CI/CD Checks

```yaml
# .github/workflows/code-quality.yml
- name: Run Security Audit
  run: npm audit --audit-level=high

- name: Check Code Coverage
  run: npm test -- --coverage --coverageThreshold='{"global":{"branches":80}}'

- name: Run SonarQube
  run: sonar-scanner
```

## CHECKLIST FINAL

Antes de aprovar o código:

- [ ] Todos os findings CRITICAL/HIGH foram resolvidos
- [ ] Testes cobrem mudanças principais
- [ ] Documentação está atualizada
- [ ] Não há regressões óbvias
- [ ] Código segue padrões do projeto
- [ ] Performance é aceitável
- [ ] Logs são adequados para debugging

## FERRAMENTAS RECOMENDADAS

- **Segurança**: Snyk, OWASP ZAP, npm audit
- **Qualidade**: ESLint, SonarQube, CodeClimate
- **Performance**: Lighthouse, WebPageTest
- **Cobertura**: Jest, NYC, Codecov
- **Documentação**: JSDoc, Swagger, Storybook

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
