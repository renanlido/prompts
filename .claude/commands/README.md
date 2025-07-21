# 📚 Commands Disponíveis

## 🎯 Comando Principal

- **`/exec-prompt`** - Prompt principal de execução de tarefas

## 🛠️ Commands Core (Funcionam em Qualquer Linguagem)

### Desenvolvimento

- **`/create-feat`** - Criar feature rapidamente (MVP approach)
- **`/create-feat-tdd`** - Criar feature com Test-Driven Development

### Manutenção

- **`/fix-bug`** - Corrigir bugs de forma sistemática
- **`/refactor-safe`** - Refatorar código com segurança (zero regressões)

### Qualidade

- **`/review-code`** - Revisar código (segurança, qualidade, performance)
- **`/optimize-perf`** - Otimizar performance com métricas

### Documentação

- **`/create-ppr`** - Criar Padrões de Práticas Recomendadas

## 🚀 Como Usar

### Uso Básico

```bash
# Sintaxe
claude /[command-name] "descrição da tarefa"

# Exemplos
claude /create-feat "Sistema de notificações em tempo real"
claude /fix-bug "Erro ao fazer login com caracteres especiais"
claude /review-code "src/api/payment/"
```

### Detecção Automática

O Claude detecta automaticamente:

- Linguagem do projeto (via arquivos de configuração)
- Gerenciador de pacotes (via lock files)
- Framework utilizado (via dependências)
- Ferramentas de teste e build

### Prioridade de Uso

1. **Sempre tente primeiro** um command core
2. **Crie commands específicos** apenas quando necessário
3. **Documente** qualquer command específico criado

## 📋 Convenções

### Placeholders Comuns

- `[PKG_MANAGER]` - Gerenciador de pacotes detectado
- `[TEST_COMMAND]` - Comando de teste da linguagem
- `[BUILD_COMMAND]` - Comando de build
- `[LINT_COMMAND]` - Comando de linting

### Estrutura de Commands

Todos os commands seguem a estrutura:

1. Objetivo
2. Fases de execução
3. Validações
4. Critérios de sucesso
5. Adaptações por linguagem

## 🔄 Manutenção

### Adicionando Novos Commands

1. Crie em `/core` se for agnóstico
2. Documente neste README
3. Inclua seção de adaptações
4. Teste em projetos diferentes

### Versionamento

- Commands core são versionados junto com o projeto
- Mudanças breaking devem ser documentadas
- Mantenha retrocompatibilidade quando possível
