# BUG FIX SISTEMÁTICO

Resolva o bug descrito em $ARGUMENTS usando uma abordagem metódica que garante correção completa e previne regressões.

## FASE 1: INVESTIGAÇÃO (think hard)

### 1.1 Reprodução do Bug

```bash
# Passos para reproduzir:
1. [Setup necessário]
2. [Ação que trigger o bug]
3. [Resultado atual vs esperado]

# Frequência: [100% | Intermitente | Condicional]
# Ambiente: [Dev | Staging | Prod]
# Versão afetada: [x.y.z]
```

### 1.2 Coleta de Evidências

- [ ] Stack trace completo
- [ ] Logs relevantes (antes, durante, depois)
- [ ] Estado do sistema no momento do erro
- [ ] Inputs que causam o problema
- [ ] Configurações relevantes

### 1.3 Análise "Five Whys"

```markdown
1. Por que o erro ocorre?
   → [Causa imediata]

2. Por que [causa imediata] acontece?
   → [Causa subjacente 1]

3. Por que [causa subjacente 1] existe?
   → [Causa subjacente 2]

4. Por que [causa subjacente 2] não foi prevenida?
   → [Falha no design/validação]

5. Por que essa falha passou despercebida?
   → [Gap no processo/testes]

CAUSA RAIZ: [Identificada após análise]
```

### 1.4 Escopo do Impacto

- Componentes afetados: [lista]
- Usuários impactados: [quantidade/tipo]
- Funcionalidades comprometidas: [lista]
- Severidade: [Critical | High | Medium | Low]
- Workarounds disponíveis: [sim/não - quais]

### 1.5 Código Suspeito

```javascript
// Localização: [arquivo:linha]
// Hipótese: [por que este código pode ser o problema]
// Evidência: [o que aponta para este código]
```

## FASE 2: PLANEJAMENTO

### 2.1 Estratégia de Correção

- [ ] Correção mínima vs refatoração
- [ ] Impacto em outras partes do sistema
- [ ] Necessidade de mudanças de arquitetura
- [ ] Compatibilidade com versões anteriores

### 2.2 Casos de Teste

```javascript
describe('Bug #[ID]: [Descrição]', () => {
  it('deve falhar antes da correção', () => {
    // Teste que reproduz o bug
    // DEVE falhar com o código atual
  });

  it('deve funcionar após correção', () => {
    // Mesmo teste, mas esperando sucesso
  });

  it('edge case 1: [descrição]', () => {
    // Variações do problema
  });

  it('não deve afetar: [funcionalidade relacionada]', () => {
    // Testes de regressão
  });
});
```

### 2.3 Riscos e Mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| [Risco 1] | [A/M/B] | [A/M/B] | [Ação] |
| [Risco 2] | [A/M/B] | [A/M/B] | [Ação] |

## FASE 3: IMPLEMENTAÇÃO

### 3.1 Test-Driven Bug Fix

```bash
# 1. Escrever teste que falha
npm test -- --watch [test-file]

# 2. Ver teste falhar (confirma reprodução)
# 3. Implementar correção mínima
# 4. Ver teste passar
# 5. Refatorar se necessário
```

### 3.2 Correção do Código

```javascript
// ANTES (bugado):
// [código original com comentário explicando o bug]

// DEPOIS (corrigido):
// [código corrigido com comentário explicando a fix]

// JUSTIFICATIVA:
// [Por que esta correção resolve o problema]
```

### 3.3 Validações Adicionais

- [ ] Input validation adicionada
- [ ] Error handling melhorado
- [ ] Logs de debug incluídos
- [ ] Assertions para invariantes
- [ ] Guards contra estados inválidos

### 3.4 Logging Melhorado

```javascript
// Adicionar logs estratégicos:
logger.debug('Estado antes: ', { ...relevantState });
logger.info('Processando [ação]', { input, config });
logger.warn('Condição inesperada', { actual, expected });
logger.error('Falha em [operação]', { error, context });
```

## FASE 4: VALIDAÇÃO

### 4.1 Testes Automatizados

```bash
# Executar em ordem:
1. npm test -- [novo-teste]    # Confirmar fix
2. npm test -- [arquivo]       # Testes do módulo
3. npm test                    # Suite completa
4. npm run test:integration    # Integração
5. npm run test:e2e           # E2E se aplicável
```

### 4.2 Testes Manuais

- [ ] Reproduzir cenário original do bug
- [ ] Testar variações do problema
- [ ] Verificar funcionalidades relacionadas
- [ ] Testar em diferentes ambientes
- [ ] Validar com dados de produção (se possível)

### 4.3 Verificação de Regressões

- [ ] Funcionalidades adjacentes funcionando
- [ ] Performance não degradada
- [ ] Nenhum novo warning/erro
- [ ] APIs mantêm contratos
- [ ] UI/UX não afetados

### 4.4 Code Review Checklist

- [ ] Fix resolve a causa raiz, não sintomas
- [ ] Código é claro e bem documentado
- [ ] Testes cobrem o cenário do bug
- [ ] Não introduz nova complexidade
- [ ] Segue padrões do projeto

## FASE 5: DOCUMENTAÇÃO

### 5.1 Atualização do Código

```javascript
/**
 * [Função/Classe]
 * 
 * @bugfix #[ID] - [Data]
 * Corrige [descrição breve do problema].
 * O bug ocorria quando [condição].
 * Solução: [abordagem utilizada].
 * 
 * @see [link para issue/ticket]
 */
```

### 5.2 Changelog

```markdown
### Bug Fixes
- **#[ID]**: Corrige [descrição user-friendly] ([commit-hash])
  - Problema: [o que estava quebrado]
  - Solução: [o que foi feito]
  - Impacto: [quem era afetado]
```

### 5.3 Documentação Técnica

```markdown
## Bug #[ID]: [Título]

### Sumário
- **Reportado em**: [data]
- **Corrigido em**: [data]
- **Severidade**: [level]
- **Componentes**: [lista]

### Análise Post-Mortem
1. **O que aconteceu**: [descrição]
2. **Por que aconteceu**: [causa raiz]
3. **Como foi corrigido**: [solução]
4. **Como prevenir**: [melhorias no processo]

### Lições Aprendidas
- [Insight 1]
- [Insight 2]
```

### 5.4 Notas para Deploy

```markdown
## Deploy Notes - Bug #[ID]

### Pré-deploy
- [ ] Backup de [dados críticos]
- [ ] Feature flag: [nome] = false

### Deploy
- [ ] Aplicar migrations: [sim/não]
- [ ] Limpar cache: [quais]
- [ ] Restart necessário: [serviços]

### Pós-deploy
- [ ] Monitorar [métricas]
- [ ] Verificar logs por [padrões]
- [ ] Teste smoke em produção
```

## FASE 6: PREVENÇÃO

### 6.1 Melhorias no Processo

- [ ] Adicionar validação que previne o bug
- [ ] Melhorar testes de integração
- [ ] Atualizar documentação de APIs
- [ ] Adicionar alertas de monitoramento
- [ ] Review de código similar

### 6.2 Padrões para Evitar

```markdown
## Anti-pattern Identificado
- **Não fazer**: [prática que levou ao bug]
- **Fazer instead**: [prática correta]
- **Exemplo**: [código demonstrativo]
```

## CRITÉRIOS DE SUCESSO

O bug está resolvido quando:

- [ ] Não pode mais ser reproduzido
- [ ] Teste automatizado previne regressão
- [ ] Causa raiz foi eliminada
- [ ] Nenhuma funcionalidade foi quebrada
- [ ] Documentação está completa
- [ ] Time foi notificado das mudanças

## TEMPLATE DE COMUNICAÇÃO

```markdown
## Bug Resolvido: [Título]

**Status**: ✅ Resolvido
**Severidade**: [Level]
**Tempo de resolução**: [X horas]

### O que estava acontecendo
[Descrição user-friendly do problema]

### O que fizemos
[Explicação simples da correção]

### O que você precisa saber
- [Impacto para usuários]
- [Ações necessárias, se houver]
- [Quando será deployado]

### Prevenção futura
[Medidas tomadas para evitar recorrência]
```

Você DEVE seguir este fluxo de trabalho rigorosamente para garantir que o bug seja resolvido de forma completa e eficaz. NUNCA pule etapas ou faça alterações sem validação adequada. A qualidade do código e a robustez da solução são essenciais para evitar problemas futuros.

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
