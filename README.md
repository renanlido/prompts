# 🚀 Claude Code Toolkit

Um conjunto completo de ferramentas para desenvolvimento de software com Claude Code, incluindo templates de projeto e commands especializados para acelerar o desenvolvimento em qualquer linguagem.

## 📋 Visão Geral

Este toolkit oferece:

- **📄 Templates de Projeto**: Templates CLAUDE.md pré-configurados para diferentes tipos de projeto
- **⚡ Commands Especializados**: Comandos para desenvolvimento, manutenção, qualidade e documentação
- **🌍 Suporte Multi-linguagem**: Funciona com JavaScript/TypeScript, PHP, Java, Python, Go, Ruby, C#/.NET e Rust
- **🔧 Configuração Flexível**: Sistema de configuração adaptável para diferentes projetos

## 🚀 Início Rápido

### 1. Setup Inicial

1. **Copie os arquivos** para o diretório do seu projeto:
   ```bash
   # Para copiar todas as ferramentas
   cp -r .claude/ seu-projeto/
   ```

2. **Escolha e configure o template** adequado para seu projeto

3. **Configure permissões** (opcional) editando `.claude/settings.local.json`

### 2. Primeiro Uso

```bash
# Execute um command básico
claude /exec-prompt "Analisar estrutura do projeto"

# Criar uma nova feature
claude /create-feat "Sistema de autenticação"

# Revisar código
claude /review-code "src/components/"
```

## 📄 Templates de Projeto

### Templates Disponíveis

| Template | Arquivo Original | Arquivo Final | Quando Usar |
|----------|------------------|---------------|-------------|
| **Backend** | `CLAUDE-back.md` | `CLAUDE.md` | APIs, serviços, microserviços |
| **Frontend** | `CLAUDE-front.md` | `CLAUDE.md` | SPAs, websites, PWAs |
| **Monorepo** | `CLAUDE-monorepo.md` | `CLAUDE.md` | Projetos com backend + frontend |

### Como Usar os Templates

1. **Identifique o tipo** do seu projeto

2. **Copie o template adequado**:
   ```bash
   # Para projeto backend
   cp CLAUDE-back.md seu-projeto/CLAUDE.md
   
   # Para projeto frontend  
   cp CLAUDE-front.md seu-projeto/CLAUDE.md
   
   # Para projeto monorepo
   cp CLAUDE-monorepo.md seu-projeto/CLAUDE.md
   ```

3. **Personalize o arquivo** `CLAUDE.md` com informações específicas do seu projeto

4. **Estruture as pastas** conforme indicado no template

### ⚠️ Importante: Renomeação de Arquivos

Os arquivos `CLAUDE-*.md` devem **sempre** ser renomeados para `CLAUDE.md` quando colocados no projeto final. O sufixo `-*` é apenas para diferenciação neste repositório de templates.

## ⚡ Sistema de Commands

### Comando Principal

- **`/exec-prompt`** - Prompt principal para execução de tarefas gerais

### Commands Especializados

#### 🛠️ Desenvolvimento
- **`/create-feat`** - Criar features rapidamente (abordagem MVP)
- **`/create-feat-tdd`** - Criar features usando Test-Driven Development

#### 🔧 Manutenção  
- **`/fix-bug`** - Corrigir bugs de forma sistemática
- **`/refactor-safe`** - Refatorar código com segurança (zero regressões)

#### ✅ Qualidade
- **`/review-code`** - Revisar código (segurança, qualidade, performance)
- **`/optimize-perf`** - Otimizar performance com métricas

#### 📚 Documentação
- **`/create-ppr`** - Criar Padrões de Práticas Recomendadas

### Como Usar os Commands

```bash
# Sintaxe básica
claude /[command-name] "descrição da tarefa"

# Exemplos práticos
claude /create-feat "Sistema de notificações em tempo real"
claude /fix-bug "Erro ao fazer login com caracteres especiais"
claude /review-code "src/api/payment/"
claude /optimize-perf "endpoint de busca de produtos"
claude /refactor-safe "função de cálculo de impostos"
```

### 🌍 Linguagens Suportadas

O sistema detecta automaticamente a linguagem do projeto e adapta:

| Linguagem | Test Runner | Linter | Build Tool | Package Manager |
|-----------|-------------|---------|------------|-----------------|
| **JavaScript/TypeScript** | Jest, Vitest, Mocha | ESLint, Biome | Webpack, Vite, esbuild | npm, yarn, pnpm, bun |
| **PHP** | PHPUnit, Pest | PHPStan, Psalm, PHPCS | Composer scripts | Composer |
| **Java** | JUnit, TestNG | SpotBugs, PMD, Checkstyle | Maven, Gradle | Maven, Gradle |
| **Python** | pytest, unittest | pylint, flake8, mypy, ruff | setuptools, poetry | pip, poetry, pipenv |
| **Go** | go test, testify | golangci-lint, go vet | go build | go mod |
| **Ruby** | RSpec, Minitest | RuboCop, Reek | rake build | Bundler |
| **C#/.NET** | xUnit, NUnit, MSTest | Roslyn Analyzers | dotnet build | NuGet |
| **Rust** | cargo test | clippy, rustfmt | cargo build | Cargo |

## 🛠️ Configuração

### Arquivo de Configuração

Edite `.claude/settings.local.json` para personalizar permissões:

```json
{
  "permissions": {
    "allow": [
      "Bash(ls:*)",
      "Bash(find:*)",
      "Bash(mv:*)",
      "Bash(grep:*)"
    ],
    "deny": []
  }
}
```

### Estrutura de Diretórios

```
seu-projeto/
├── .claude/                  # Configurações do Claude
│   ├── commands/            # Commands customizados
│   │   ├── exec-prompt.md   # Comando principal
│   │   ├── core/           # Commands especializados
│   │   └── README.md       # Documentação dos commands
│   └── settings.local.json # Configurações
├── CLAUDE.md               # Template do projeto (renomeado)
└── [arquivos do projeto]
```

## 📖 Guias de Uso

### Workflow de Desenvolvimento

1. **Setup**: Configure o template adequado
2. **Desenvolva**: Use `/create-feat` ou `/create-feat-tdd` 
3. **Mantenha**: Use `/fix-bug` e `/refactor-safe`
4. **Valide**: Use `/review-code` e `/optimize-perf`
5. **Documente**: Use `/create-ppr` para padrões

### Boas Práticas

- **Sempre leia** a documentação do command antes de usar
- **Teste incrementalmente** as mudanças sugeridas
- **Combine commands** para workflows completos
- **Mantenha** o arquivo CLAUDE.md atualizado
- **Versione** mudanças em commands customizados

### Troubleshooting

**Commands não funcionam?**
- Verifique se está no diretório correto do projeto
- Confirme que o arquivo CLAUDE.md existe
- Verifique permissões em settings.local.json

**Template não se adapta ao projeto?**
- Personalize o CLAUDE.md para seu caso específico
- Crie commands customizados em `.claude/commands/`
- Consulte a documentação específica da linguagem

## 🤝 Contribuição

### Criando Novos Commands

1. **Crie um arquivo** em `.claude/commands/core/`
2. **Siga a estrutura** dos commands existentes
3. **Inclua seção** "Adaptações por Linguagem"
4. **Documente no README** dos commands

### Criando Novos Templates

1. **Analise os templates** existentes
2. **Identifique o padrão** comum
3. **Crie novo arquivo** `CLAUDE-[tipo].md`
4. **Teste com projetos** reais

### Estrutura de um Command

```markdown
# NOME DO COMMAND

Descrição do que o command faz.

## FASE 1: [Nome da Fase]
[Instruções detalhadas]

## FASE 2: [Nome da Fase]  
[Instruções detalhadas]

## Adaptações por Linguagem
[Tabela com especificidades de cada linguagem]
```

## 📚 Recursos Adicionais

### Documentação Detalhada

- **Commands**: Consulte `.claude/commands/README.md`
- **Templates**: Leia os comentários nos arquivos CLAUDE-*.md
- **Configuração**: Veja exemplos em settings.local.json

### Exemplos de Uso

Para exemplos práticos de cada command, consulte:
- [Documentação dos Commands](.claude/commands/README.md)
- [Templates de Projeto](CLAUDE-back.md)
- [Configurações Avançadas](.claude/settings.local.json)

### Comunidade

- **Issues**: Reporte problemas ou sugestões
- **Discussions**: Compartilhe cases de uso
- **Wiki**: Contribua com exemplos e dicas

---

**Desenvolvido para acelerar o desenvolvimento com Claude Code em qualquer linguagem e tipo de projeto.**