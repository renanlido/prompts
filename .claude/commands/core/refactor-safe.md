# REFATORAÇÃO SEGURA

Refatore o código em $ARGUMENTS seguindo um processo sistemático e seguro que garante zero regressões.

## FASE 1: ANÁLISE DE IMPACTO (think hard)

### 1.1 Mapeamento de Dependências

- Liste TODOS os arquivos que importam o código a ser refatorado
- Identifique APIs públicas que não podem mudar
- Mapeie fluxos de dados através do código
- Documente side effects existentes

### 1.2 Avaliação de Riscos

Classifique o risco da refatoração (1-5):

- **1 - Trivial**: Mudanças locais, sem dependências externas
- **2 - Baixo**: Poucas dependências, bem testado
- **3 - Médio**: Múltiplas dependências, testes parciais
- **4 - Alto**: Core do sistema, muitas dependências
- **5 - Crítico**: Código de segurança/pagamento, sem testes

### 1.3 Análise de Testes

- Cobertura atual do código: ____%
- Qualidade dos testes existentes
- Testes de integração disponíveis
- Gaps de cobertura identificados

### 1.4 Breaking Changes

Liste possíveis breaking changes:

- [ ] Mudanças em assinaturas de funções
- [ ] Alterações em estruturas de dados
- [ ] Modificações em comportamento
- [ ] Impactos em performance

## FASE 2: PREPARAÇÃO

### 2.1 Garantir Cobertura de Testes quando aplicável

```bash
# Verificar cobertura atual
npm test -- --coverage [arquivo]

# Se cobertura < 100% no código afetado:
# 1. Escrever testes para comportamento atual
# 2. Incluir edge cases
# 3. Adicionar testes de integração se necessário
```

### 2.2 Criar Safety Net

- Snapshot dos testes atuais
- Documentar comportamento esperado
- Criar testes de caracterização
- Preparar rollback plan

### 2.3 Documentação Pré-Refatoração

```markdown
## Comportamento Atual
- Input: [descrição]
- Processing: [steps]
- Output: [descrição]
- Side Effects: [lista]
```

## FASE 3: REFATORAÇÃO INCREMENTAL

### 3.1 Estratégia de Decomposição

Para funções > 40 linhas:

1. Extrair funções auxiliares primeiro
2. Manter interface pública inalterada
3. Um refactor por vez
4. Executar testes após CADA mudança

### 3.2 Padrões de Refatoração

Aplicar na ordem:

1. **Extract Method**: Isolar lógica complexa
2. **Rename**: Melhorar clareza
3. **Move**: Reorganizar responsabilidades
4. **Simplify**: Reduzir complexidade
5. **Remove Duplication**: Consolidar código

### 3.3 Commits Atômicos

```bash
# Cada commit deve:
git add [arquivo específico]
git commit -m "refactor: [ação específica] em [componente]

- Extraído método [nome] para melhorar clareza
- Todos os testes passando
- Sem mudanças funcionais"
```

### 3.4 Checklist por Iteração

- [ ] Mudança única e isolada
- [ ] Todos os testes passam
- [ ] Sem warnings de linting
- [ ] Performance mantida ou melhorada
- [ ] Código mais claro que antes

## FASE 4: VALIDAÇÃO

### 4.1 Suite de Testes

```bash
# Executar em ordem:
1. npm test -- [arquivo].test   # Testes unitários
2. npm test                     # Todos os testes
3. npm run test:integration     # Integração
4. npm run test:e2e            # Se aplicável
```

### 4.2 Análise de Performance

- Comparar métricas antes/depois
- Verificar uso de memória
- Medir tempo de execução
- Validar complexidade algorítmica

### 4.3 Code Review Checklist

- [ ] Código mais legível
- [ ] Menos complexidade ciclomática
- [ ] Melhor separação de responsabilidades
- [ ] Redução de duplicação
- [ ] Manutenibilidade aumentada

### 4.4 Validação de Comportamento

- [ ] Outputs idênticos para mesmos inputs
- [ ] Side effects preservados
- [ ] Tratamento de erros mantido
- [ ] Edge cases funcionando
- [ ] Backwards compatibility

## FASE 5: DOCUMENTAÇÃO

### 5.1 Relatório de Mudanças

```markdown
## Refatoração Realizada

### Antes
- Complexidade: X
- Linhas: Y
- Cobertura: Z%

### Depois
- Complexidade: X-n
- Linhas: Y-m
- Cobertura: 100%

### Melhorias
1. [Descrição específica]
2. [Métrica objetiva]
```

### 5.2 Atualizar Documentação

- [ ] JSDoc/docstrings atualizados
- [ ] README se interface mudou
- [ ] Changelog atualizado
- [ ] Documentação de arquitetura

## ROLLBACK PLAN

Se QUALQUER teste falhar:

1. `git stash` mudanças atuais
2. Analisar falha específica
3. Se não for óbvio: reverter ao último commit verde
4. Replanejar abordagem
5. Tentar novamente com passos menores

## CRITÉRIOS DE SUCESSO

A refatoração está completa quando:

- [ ] 100% dos testes passam
- [ ] Nenhuma funcionalidade foi alterada
- [ ] Código está objetivamente melhor (métricas)
- [ ] Review manual não encontra problemas
- [ ] Performance igual ou superior
- [ ] Documentação reflete mudanças

## FLAGS DE ALERTA

PARE imediatamente se:

- 🚨 Testes começam a falhar inexplicavelmente
- 🚨 Performance degrada significativamente
- 🚨 Comportamento muda de forma não intencional
- 🚨 Descoberta de dependências não mapeadas
- 🚨 Complexidade aumenta ao invés de diminuir

Nestes casos, reverta e replaneje com mais análise.

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
