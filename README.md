# Cloud Agents 🤖☁️

> Plataforma full-stack de agentes AI inteligentes com integração Blender

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue.svg)](https://www.typescriptlang.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.109+-green.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18+-61dafb.svg)](https://reactjs.org/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Arquitetura](#arquitetura)
- [Quick Start](#quick-start)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Desenvolvimento](#desenvolvimento)
- [API](#api)
- [Agentes Disponíveis](#agentes-disponíveis)
- [Integração Blender](#integração-blender)
- [Testes](#testes)
- [Deploy](#deploy)
- [Contribuindo](#contribuindo)

## 🎯 Visão Geral

Cloud Agents é uma plataforma completa para criação, gerenciamento e execução de agentes AI na nuvem, com suporte a:

- 🤖 **Múltiplos LLMs**: OpenAI, Anthropic, e modelos locais
- 🎨 **Integração Blender**: Automação 3D via Python API
- ⚡ **Alta Performance**: Processamento assíncrono e cache inteligente
- 🔄 **Real-time**: WebSockets para streaming de respostas
- 🛡️ **Seguro**: Autenticação JWT e validação robusta
- 📊 **Observabilidade**: Logs estruturados e métricas

### Casos de Uso

- Automação de workflows criativos
- Geração de conteúdo 3D automatizado
- Assistentes virtuais especializados
- Processamento de dados em lote
- Integração com pipelines de produção

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                              │
│                  React + TypeScript                          │
│              (UI, State Management, API Client)              │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTP/REST + WebSockets
┌──────────────────────────▼──────────────────────────────────┐
│                         BACKEND                              │
│                       FastAPI                                │
│         (Routing, Auth, Business Logic)                      │
├──────────────────────────┬──────────────────────────────────┤
│       AGENTS LAYER       │     BLENDER INTEGRATION           │
│   (AI Processing Logic)  │   (3D Automation Scripts)         │
└──────────────┬───────────┴───────────────┬──────────────────┘
               │                           │
    ┌──────────▼──────────┐    ┌──────────▼──────────┐
    │    PostgreSQL       │    │     Redis Cache      │
    │   (Data Storage)    │    │   (Session/Queue)    │
    └─────────────────────┘    └─────────────────────┘
```

### Stack Tecnológica

**Backend:**
- FastAPI (Web Framework)
- SQLAlchemy (ORM)
- Pydantic (Validation)
- LangChain (AI Orchestration)
- Celery (Task Queue)

**Frontend:**
- React 18
- TypeScript 5
- TanStack Query
- Zustand (State)
- TailwindCSS

**Infraestrutura:**
- Docker & Docker Compose
- PostgreSQL 16
- Redis 7
- Nginx (Reverse Proxy)

## 🚀 Quick Start

### Pré-requisitos

- Python 3.11+
- Node.js 18+
- Docker & Docker Compose
- Git

### Instalação Rápida

```bash
# 1. Clone o repositório
git clone https://github.com/bentojk/Cloud-Agents.git
cd Cloud-Agents

# 2. Configure as variáveis de ambiente
cp .env.example .env
# Edite .env com suas API keys

# 3. Inicie com Docker
docker-compose up -d

# 4. Acesse a aplicação
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/docs
```

### Desenvolvimento Local (Sem Docker)

**Backend:**
```bash
cd backend

# Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou: venv\Scripts\activate  # Windows

# Instalar dependências
pip install -r requirements.txt

# Rodar migrações
alembic upgrade head

# Iniciar servidor
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

**Frontend:**
```bash
cd frontend

# Instalar dependências
npm install

# Iniciar dev server
npm run dev
```

## 📁 Estrutura do Projeto

```
Cloud-Agents/
├── backend/                    # Backend FastAPI
│   ├── agents/                # Implementação de agentes
│   │   ├── base.py           # Classe base para agentes
│   │   ├── text_agent.py     # Agente de texto
│   │   └── vision_agent.py   # Agente de visão
│   ├── api/                  # Endpoints da API
│   │   ├── v1/
│   │   │   ├── agents.py     # Rotas de agentes
│   │   │   ├── auth.py       # Autenticação
│   │   │   └── users.py      # Gerenciamento de usuários
│   │   └── deps.py           # Dependências compartilhadas
│   ├── core/                 # Configurações centrais
│   │   ├── config.py         # Settings da aplicação
│   │   ├── security.py       # JWT, passwords
│   │   └── database.py       # Conexão DB
│   ├── models/               # Modelos SQLAlchemy
│   ├── schemas/              # Schemas Pydantic
│   ├── services/             # Lógica de negócio
│   ├── tasks/                # Tarefas Celery
│   ├── tests/                # Testes
│   ├── main.py               # Entry point
│   └── requirements.txt      # Dependências Python
│
├── frontend/                  # Frontend React
│   ├── src/
│   │   ├── components/       # Componentes React
│   │   ├── pages/            # Páginas
│   │   ├── hooks/            # Custom hooks
│   │   ├── services/         # API clients
│   │   ├── store/            # State management
│   │   ├── types/            # TypeScript types
│   │   ├── utils/            # Utilidades
│   │   ├── App.tsx           # App principal
│   │   └── main.tsx          # Entry point
│   ├── public/               # Assets estáticos
│   ├── package.json          # Dependências Node
│   └── tsconfig.json         # Config TypeScript
│
├── blender/                   # Scripts Blender
│   ├── addons/               # Addons customizados
│   ├── scripts/              # Scripts de automação
│   │   ├── render_scene.py   # Renderização
│   │   └── export_models.py  # Export de modelos
│   └── templates/            # Templates de cena
│
├── agents/                    # Configurações de agentes
│   ├── configs/              # YAML configs
│   └── prompts/              # System prompts
│
├── shared/                    # Código compartilhado
│   ├── types/                # Types compartilhados
│   └── utils/                # Utilidades compartilhadas
│
├── docker-compose.yml         # Orquestração Docker
├── .cursorrules              # Regras do Cursor AI
├── .gitignore                # Git ignore
├── .env.example              # Exemplo de env vars
└── README.md                 # Este arquivo
```

## 💻 Desenvolvimento

### Comandos Úteis

**Docker:**
```bash
# Iniciar todos os serviços
docker-compose up -d

# Ver logs
docker-compose logs -f [service]

# Rebuild
docker-compose up -d --build

# Parar tudo
docker-compose down

# Limpar volumes
docker-compose down -v
```

**Backend:**
```bash
# Criar migração
alembic revision --autogenerate -m "description"

# Aplicar migrações
alembic upgrade head

# Reverter migração
alembic downgrade -1

# Testes
pytest
pytest --cov=backend --cov-report=html

# Linting
ruff check .
ruff format .
mypy .
```

**Frontend:**
```bash
# Dev server
npm run dev

# Build produção
npm run build

# Testes
npm test
npm run test:coverage

# Linting
npm run lint
npm run lint:fix
npm run type-check
```

### Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
# Application
ENVIRONMENT=development
SECRET_KEY=your-secret-key-change-in-production
API_PREFIX=/api/v1

# Database
DATABASE_URL=postgresql+asyncpg://postgres:postgres@localhost:5432/cloudagents

# Redis
REDIS_URL=redis://localhost:6379/0

# AI Models
OPENAI_API_KEY=sk-your-openai-key
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key
DEFAULT_MODEL=gpt-4

# Blender
BLENDER_PATH=/usr/bin/blender
BLENDER_RENDER_OUTPUT=/tmp/renders

# Frontend
REACT_APP_API_URL=http://localhost:8000/api/v1
REACT_APP_WS_URL=ws://localhost:8000/ws

# Monitoring (Optional)
SENTRY_DSN=your-sentry-dsn
```

## 🔌 API

### Documentação Interativa

Acesse a documentação Swagger em: `http://localhost:8000/docs`

### Endpoints Principais

**Autenticação:**
```bash
POST /api/v1/auth/register    # Criar conta
POST /api/v1/auth/login       # Login
POST /api/v1/auth/refresh     # Refresh token
```

**Agentes:**
```bash
GET    /api/v1/agents              # Listar agentes
POST   /api/v1/agents              # Criar agente
GET    /api/v1/agents/{id}         # Detalhes do agente
PUT    /api/v1/agents/{id}         # Atualizar agente
DELETE /api/v1/agents/{id}         # Deletar agente
POST   /api/v1/agents/{id}/execute # Executar agente
```

**Blender:**
```bash
POST /api/v1/blender/render    # Renderizar cena
POST /api/v1/blender/export    # Exportar modelo
GET  /api/v1/blender/status    # Status do processo
```

### Exemplo de Uso

**Python:**
```python
import httpx

async def execute_agent():
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "http://localhost:8000/api/v1/agents/execute",
            json={
                "agent_id": "text-agent-1",
                "prompt": "Explain quantum computing",
                "stream": False
            },
            headers={"Authorization": f"Bearer {token}"}
        )
        return response.json()
```

**TypeScript:**
```typescript
import axios from 'axios';

const executeAgent = async (agentId: string, prompt: string) => {
  const { data } = await axios.post(
    '/api/v1/agents/execute',
    { agent_id: agentId, prompt },
    { headers: { Authorization: `Bearer ${token}` } }
  );
  return data;
};
```

## 🤖 Agentes Disponíveis

### Text Agent
- **Propósito**: Geração e análise de texto
- **Modelos**: GPT-4, Claude 3
- **Casos de uso**: Chatbots, summarização, tradução

### Vision Agent
- **Propósito**: Análise de imagens
- **Modelos**: GPT-4 Vision, Claude 3 Opus
- **Casos de uso**: OCR, detecção de objetos, descrição de imagens

### Code Agent
- **Propósito**: Geração e revisão de código
- **Modelos**: GPT-4, Claude 3
- **Casos de uso**: Code review, debugging, documentação

### Blender Agent
- **Propósito**: Automação 3D
- **Integração**: Blender Python API
- **Casos de uso**: Renderização em lote, export de modelos, animações

## 🎨 Integração Blender

### Configuração

1. Instale o Blender 4.0+
2. Configure o caminho no `.env`:
   ```env
   BLENDER_PATH=/usr/bin/blender
   ```

### Scripts Disponíveis

**Renderização:**
```python
from blender.scripts.render_scene import render

result = await render({
    "scene": "path/to/scene.blend",
    "output": "/tmp/output",
    "format": "PNG",
    "samples": 128
})
```

**Export de Modelos:**
```python
from blender.scripts.export_models import export

result = await export({
    "scene": "path/to/scene.blend",
    "objects": ["Cube", "Sphere"],
    "format": "FBX"
})
```

## 🧪 Testes

### Backend

```bash
# Todos os testes
pytest

# Com coverage
pytest --cov=backend --cov-report=html

# Testes específicos
pytest backend/tests/test_agents.py

# Testes assíncronos
pytest -k "async"
```

### Frontend

```bash
# Todos os testes
npm test

# Watch mode
npm test -- --watch

# Coverage
npm run test:coverage

# E2E (se configurado)
npm run test:e2e
```

### Cobertura de Testes

- **Objetivo**: 80%+ de cobertura
- **Backend**: Unit + Integration tests
- **Frontend**: Component + Hook tests

## 🚢 Deploy

### Docker (Recomendado)

```bash
# Build imagens de produção
docker-compose -f docker-compose.yml --profile production build

# Iniciar em produção
docker-compose --profile production up -d
```

### Deploy Manual

**Backend:**
```bash
# Instalar dependências
pip install -r requirements.txt

# Rodar migrações
alembic upgrade head

# Iniciar com Gunicorn
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000
```

**Frontend:**
```bash
# Build
npm run build

# Servir com Nginx
cp -r dist/* /var/www/html/
```

### Variáveis de Ambiente de Produção

```env
ENVIRONMENT=production
SECRET_KEY=<strong-random-key>
DATABASE_URL=<production-db-url>
REDIS_URL=<production-redis-url>
SENTRY_DSN=<your-sentry-dsn>
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/amazing-feature`)
3. Commit suas mudanças (`git commit -m 'feat: add amazing feature'`)
4. Push para a branch (`git push origin feature/amazing-feature`)
5. Abra um Pull Request

### Padrões de Commit

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` Nova funcionalidade
- `fix:` Correção de bug
- `docs:` Documentação
- `style:` Formatação
- `refactor:` Refatoração
- `test:` Testes
- `chore:` Manutenção

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 👥 Autores

- **bentojk** - *Initial work* - [GitHub](https://github.com/bentojk)

## 🙏 Agradecimentos

- OpenAI pela API GPT
- Anthropic pela API Claude
- Blender Foundation
- Comunidade FastAPI
- Comunidade React

## 📞 Suporte

- 📧 Email: support@cloudagents.dev
- 💬 Discord: [Join our server](#)
- 🐛 Issues: [GitHub Issues](https://github.com/bentojk/Cloud-Agents/issues)

---

**Status:** 🚀 Em desenvolvimento ativo

**Versão:** 0.1.0

**Iniciado em:** 04/11/2025

Made with ❤️ by the Cloud Agents team
