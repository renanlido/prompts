# CLAUDE.md - Projeto Monorepo

## 🎯 Estrutura do Projeto

Este é um projeto **monorepo** que contém tanto o backend quanto o frontend em pastas separadas.

```
projeto/
├── .claude/              # 📁 Configurações globais do Claude
│   ├── commands/         # Comandos customizados
│   ├── pprs/            # Padrões de Práticas Recomendadas
│   └── docs/            # Documentação adicional
├── backend/             # 🔧 API e lógica de servidor
│   └── CLAUDE.md        # Documentação específica do backend
├── frontend/            # 🎨 Interface do usuário
│   └── CLAUDE.md        # Documentação específica do frontend
└── CLAUDE.md           # 📄 Este arquivo (visão geral)
```

## ⚠️ IMPORTANTE: Navegação e Contexto

### Para alterações no BACKEND

- **Navegue para**: `backend/`
- **Consulte**: `backend/CLAUDE.md` para convenções específicas
- **Arquivos**: Todas as alterações de API, banco de dados, serviços devem ser feitas em `backend/`

### Para alterações no FRONTEND

- **Navegue para**: `frontend/`
- **Consulte**: `frontend/CLAUDE.md` para convenções específicas
- **Arquivos**: Todas as alterações de UI, componentes, páginas devem ser feitas em `frontend/`

### Recursos Compartilhados

- **Commands**: `.claude/commands/` - Comandos customizados disponíveis globalmente
- **PPRs**: `.claude/pprs/` - Padrões que se aplicam a todo o projeto
- **Docs**: `.claude/docs/` - Documentação geral e decisões arquiteturais

## 🔧 Comandos Principais

```bash
# Na raiz do projeto
cd backend   # Para trabalhar no backend
cd frontend  # Para trabalhar no frontend

# Comandos devem ser executados dentro de cada pasta
# Backend
cd backend && [PKG_MANAGER] dev

# Frontend  
cd frontend && [PKG_MANAGER] dev
```

## 📋 Regras Gerais

1. **SEMPRE** verifique em qual contexto está trabalhando (backend ou frontend)
2. **NUNCA** misture código de backend e frontend
3. **USE** os comandos da pasta `.claude/commands/` quando disponíveis
4. **SIGA** os PPRs em `.claude/pprs/` para manter consistência
5. **CONSULTE** o CLAUDE.md específico de cada projeto para convenções detalhadas

## 🚀 Quick Start

```bash
# 1. Clone o repositório
git clone [repo-url]
cd [projeto]

# 2. Instale dependências do backend
cd backend
[PKG_MANAGER] install

# 3. Instale dependências do frontend
cd ../frontend
[PKG_MANAGER] install

# 4. Configure ambientes
# Backend
cd ../backend
cp .env.example .env

# Frontend
cd ../frontend
cp .env.example .env.local

# 5. Execute os projetos (em terminais separados)
# Terminal 1
cd backend && [PKG_MANAGER] dev

# Terminal 2
cd frontend && [PKG_MANAGER] dev
```

## 📚 Documentação Detalhada

Para informações específicas sobre:

- **Arquitetura Backend**: Veja `backend/CLAUDE.md`
- **Arquitetura Frontend**: Veja `frontend/CLAUDE.md`
- **Comandos Customizados**: Veja `.claude/commands/`
- **Padrões de Código**: Veja `.claude/pprs/`
- **Decisões Técnicas**: Veja `.claude/docs/adr/`

## 🔄 Fluxo de Trabalho

1. **Identifique** se a tarefa é de backend ou frontend
2. **Navegue** para a pasta apropriada
3. **Consulte** o CLAUDE.md específico
4. **Use** os comandos em `.claude/commands/` quando aplicável
5. **Siga** os padrões definidos nos PPRs
6. **Execute** comandos sempre dentro da pasta correta

## ⚡ Dicas Importantes

- Os projetos podem ter **gerenciadores de pacotes diferentes** (verifique cada pasta)
- As **portas padrão** geralmente são:
  - Backend: `http://localhost:3000` ou `http://localhost:3001`
  - Frontend: `http://localhost:3000` ou `http://localhost:5173`
- **Variáveis de ambiente** são configuradas separadamente
- **Testes** devem ser executados em cada projeto individualmente

## 🚨 Avisos

> ⚠️ **ATENÇÃO**: Este é apenas um arquivo de navegação. Para qualquer implementação ou alteração de código, SEMPRE consulte o CLAUDE.md específico da pasta onde você está trabalhando.

> 📌 **LEMBRE-SE**: Os comandos customizados, PPRs e documentações adicionais estão em `.claude/` na raiz do projeto.

---

**Para começar a trabalhar**:

1. Decida se vai trabalhar no backend ou frontend
2. Navegue para a pasta correspondente
3. Consulte o CLAUDE.md específico daquela pasta
4. Siga as instruções detalhadas encontradas lá

Este arquivo serve apenas como **ponto de entrada** e **guia de navegação** para o projeto monorepo.
