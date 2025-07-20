# CRIAÇÃO DE PPR (PADRÕES DE PRÁTICAS RECOMENDADAS)

Crie um documento de Padrões de Práticas Recomendadas para $ARGUMENTS que servirá como guia definitivo para implementação consistente e de alta qualidade.

## ESTRUTURA DO PPR

### 1. CABEÇALHO E METADADOS

```markdown
# PPR-[ID]: [Nome do Padrão]

**Versão**: 1.0.0
**Data**: [YYYY-MM-DD]
**Autor**: [Nome/Time]
**Status**: [Draft | Review | Approved | Deprecated]
**Tags**: [arquitetura, segurança, performance, etc]

## Resumo Executivo
[Uma frase que descreve o padrão e seu valor principal]
```

### 2. CONTEXTO E OBJETIVO

#### 2.1 Problema que Resolve

```markdown
## O Problema

### Cenário
[Descrição detalhada da situação problemática]

### Sintomas
- [ ] [Sintoma observável 1]
- [ ] [Sintoma observável 2]
- [ ] [Sintoma observável 3]

### Impacto
- **Performance**: [Como afeta]
- **Manutenibilidade**: [Como afeta]
- **Segurança**: [Como afeta]
- **Developer Experience**: [Como afeta]

### Exemplo do Anti-Pattern
```[linguagem]
// ❌ PROBLEMA: [descrição]
[código demonstrando o problema]
```

```

#### 2.2 Quando Usar vs Quando NÃO Usar
```markdown
## Aplicabilidade

### ✅ USE este padrão quando:
- [Condição específica 1]
- [Condição específica 2]
- [Condição específica 3]

### ❌ NÃO USE este padrão quando:
- [Condição onde não é apropriado 1]
- [Condição onde não é apropriado 2]
- [Alternativa melhor existe para cenário X]

### 🤔 CONSIDERE alternativas quando:
- [Situação limítrofe 1]
- [Situação limítrofe 2]
```

#### 2.3 Trade-offs e Alternativas

```markdown
## Trade-offs

### Vantagens
| Aspecto | Benefício | Métrica |
|---------|-----------|---------|
| Performance | [Descrição] | [+X%] |
| Legibilidade | [Descrição] | [Score] |
| Testabilidade | [Descrição] | [Métrica] |

### Desvantagens
| Aspecto | Custo | Mitigação |
|---------|-------|-----------|
| Complexidade | [Descrição] | [Como mitigar] |
| Setup inicial | [Descrição] | [Como simplificar] |

### Alternativas Consideradas
1. **[Alternativa 1]**
   - Prós: [lista]
   - Contras: [lista]
   - Quando escolher: [critério]

2. **[Alternativa 2]**
   - Prós: [lista]
   - Contras: [lista]
   - Quando escolher: [critério]
```

### 3. IMPLEMENTAÇÃO PADRÃO

#### 3.1 Exemplo Canônico

```markdown
## Implementação de Referência

### Estrutura Base
```[linguagem]
/**
 * Implementação padrão do [Nome do Padrão]
 * 
 * @description [Breve descrição do que faz]
 * @pattern [Nome do design pattern se aplicável]
 * @since v[versão]
 */

// Imports necessários
import { [dependências] } from '[pacotes]';

// Configuração e constantes
const CONFIG = {
  [configuração]: [valor]
};

// Implementação principal
[código completo e funcional com comentários explicativos]

// Exemplo de uso
const example = new [Classe]();
example.[método]([parâmetros]);
```

### Explicação Passo a Passo

1. **[Passo 1]**: [Por que e como]
2. **[Passo 2]**: [Por que e como]
3. **[Passo 3]**: [Por que e como]

```

#### 3.2 Configurações e Setup
```markdown
## Setup Necessário

### Dependências
```json
{
  "dependencies": {
    "[pacote]": "^[versão]"
  },
  "devDependencies": {
    "[pacote-dev]": "^[versão]"
  }
}
```

### Configuração de Ambiente

```bash
# .env
[VARIÁVEL]=[valor]
```

### Configuração de Build/Compilador

```javascript
// [arquivo.config.js]
module.exports = {
  [configuração]: [valor]
};
```

```

### 4. VARIAÇÕES COMUNS

#### 4.1 Adaptações por Contexto
```markdown
## Variações do Padrão

### Variação 1: [Nome] - Para [Contexto]
```[linguagem]
// Quando: [situação específica]
// Mudanças: [o que muda do padrão base]

[código da variação]
```

### Variação 2: [Nome] - Com [Framework/Library]

```[linguagem]
// Integração com [Framework]
// Benefícios adicionais: [lista]

[código da variação]
```

### Variação 3: [Nome] - Performance Crítica

```[linguagem]
// Otimizações: [lista]
// Trade-off: [o que sacrifica]

[código otimizado]
```

```

#### 4.2 Integrações com Frameworks
```markdown
## Integrações

### React/Next.js
```jsx
// Hook customizado seguindo o padrão
const use[Pattern] = (options) => {
  [implementação]
};
```

### Express/Node.js

```javascript
// Middleware seguindo o padrão
const [pattern]Middleware = (options) => {
  return (req, res, next) => {
    [implementação]
  };
};
```

### Vue.js

```javascript
// Composable seguindo o padrão
export const use[Pattern] = (options) => {
  [implementação]
};
```

```

### 5. ANTI-PATTERNS

#### 5.1 O Que Evitar
```markdown
## Anti-Patterns Comuns

### Anti-Pattern 1: [Nome]
```[linguagem]
// ❌ NUNCA FAÇA ISSO
[código problemático]

// Problemas:
// 1. [Problema específico]
// 2. [Outro problema]
// 3. [Consequência]
```

### Anti-Pattern 2: [Nome]

```[linguagem]
// ❌ EVITE
[código problemático]

// Por que é ruim:
// - [Razão 1]
// - [Razão 2]
```

```

#### 5.2 Como Migrar Código Legado
```markdown
## Migração de Código Legado

### Estratégia de Migração
1. **Identificar**: Buscar por [padrões no código]
2. **Isolar**: Criar abstração temporária
3. **Migrar**: Converter incrementalmente
4. **Validar**: Garantir comportamento idêntico
5. **Limpar**: Remover código antigo

### Script de Migração
```javascript
// migrate-to-[pattern].js
const fs = require('fs');
const path = require('path');

function migrateFile(filePath) {
  // 1. Ler arquivo
  // 2. Identificar padrões antigos com regex
  // 3. Transformar para novo padrão
  // 4. Escrever arquivo atualizado
}

// Exemplo de transformação
const oldPattern = /[regex do padrão antigo]/g;
const newPattern = '[template do novo padrão]';
```

### Checklist de Migração

- [ ] Backup do código original
- [ ] Testes passando antes da migração
- [ ] Migração incremental (arquivo por arquivo)
- [ ] Testes passando após cada mudança
- [ ] Code review das mudanças
- [ ] Documentação atualizada

```

### 6. CHECKLIST DE IMPLEMENTAÇÃO

```markdown
## Checklist de Implementação

### Pré-implementação
- [ ] Problema claramente identificado
- [ ] Padrão é apropriado para o contexto
- [ ] Alternativas foram consideradas
- [ ] Time está alinhado com a decisão

### Durante Implementação
- [ ] Seguindo exemplo canônico
- [ ] Testes escritos primeiro (TDD)
- [ ] Documentação inline adequada
- [ ] Tratamento de erros completo
- [ ] Logs apropriados adicionados

### Validação
- [ ] Todos os testes passam
- [ ] Code coverage > 90%
- [ ] Performance medida e aceitável
- [ ] Sem warnings de linting
- [ ] Documentação completa

### Pós-implementação
- [ ] Code review aprovado
- [ ] Métricas de sucesso definidas
- [ ] Monitoramento configurado
- [ ] Time treinado no padrão
```

### 7. EXEMPLOS REAIS E CASOS DE USO

```markdown
## Casos de Uso no Projeto

### Exemplo 1: [Feature/Módulo]
**Contexto**: [Descrição da necessidade]
**Implementação**: `src/modules/[module]/[file].js`
**Resultados**:
- Performance: [métrica antes] → [métrica depois]
- Bugs relacionados: [número antes] → [número depois]
- Satisfação do desenvolvedor: [feedback]

### Exemplo 2: [Feature/Módulo]
**Link**: [URL para PR ou commit]
**Aprendizados**: [O que foi aprendido]

### Métricas de Sucesso
| Métrica | Antes | Depois | Melhoria |
|---------|-------|--------|----------|
| [Métrica 1] | [valor] | [valor] | [%] |
| [Métrica 2] | [valor] | [valor] | [%] |
```

### 8. RECURSOS E REFERÊNCIAS

```markdown
## Recursos Adicionais

### Documentação
- [Link oficial do padrão]
- [Artigo explicativo]
- [Video tutorial]

### Ferramentas
- **[Ferramenta 1]**: [Como ajuda]
- **[Ferramenta 2]**: [Como ajuda]

### Leitura Recomendada
1. "[Título do Livro]" - [Autor] (Capítulo X)
2. "[Artigo]" - [Link]
3. "[Talk/Video]" - [Link]

### Comunidade
- Slack: #[canal]
- Wiki: [link interno]
- Especialistas: @[pessoa1], @[pessoa2]
```

### 9. HISTÓRICO DE MUDANÇAS

```markdown
## Changelog

### v1.0.0 - [Data]
- Versão inicial do PPR
- Definição do padrão base
- Exemplos principais

### v1.1.0 - [Data]
- Adicionada variação para [contexto]
- Melhorada seção de migração
- Corrigido exemplo de [caso]

### Decisões Arquiteturais (ADRs)
- **ADR-001**: [Título] - [Link]
- **ADR-002**: [Título] - [Link]
```

## TEMPLATE DE VALIDAÇÃO

Antes de publicar o PPR, valide:

### Clareza

- [ ] Um júnior consegue entender e implementar?
- [ ] Exemplos são completos e funcionais?
- [ ] Trade-offs estão explícitos?

### Completude

- [ ] Todos os cenários comuns cobertos?
- [ ] Troubleshooting incluído?
- [ ] Métricas de sucesso definidas?

### Praticidade

- [ ] Exemplos rodam sem modificação?
- [ ] Ferramentas necessárias listadas?
- [ ] Processo de migração claro?

### Manutenibilidade

- [ ] Versionamento definido?
- [ ] Ownership claro?
- [ ] Processo de atualização estabelecido?

## EXEMPLO DE USO DO COMMAND

```bash
# Criar PPR para padrão de Repository
claude-code create-ppr "Repository Pattern para acesso a dados"

# Criar PPR para error handling
claude-code create-ppr "Error Handling e Logging Estruturado"

# Criar PPR para autenticação
claude-code create-ppr "Autenticação e Autorização com JWT"
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
