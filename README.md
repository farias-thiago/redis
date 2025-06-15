# 🚀 Trabalho Final - In Memory DB com Redis

## 📋 Descrição
Sistema completo de ingestão de dados para quiz de Engenharia de Dados usando Redis, PostgreSQL e Metabase.

## 🏗️ Arquitetura
```
API Quiz → Redis → RedisGears → PostgreSQL → Metabase
```

## 📊 Dashboards Implementados
1. **Alternativas Mais Votadas** - Análise de preferências por alternativa
2. **Questões Mais Acertadas** - Performance por questão  
3. **Ranking Alunos (1ª Tentativa)** - Melhores performers iniciais
4. **Ranking Alunos (Com Retentativas)** - Performance geral
5. **Questões com Mais Retentativas** - Dificuldade por questão
6. **Questões Erradas Mais Votadas** - Análise de erros
7. **Questões por Assunto** - Distribuição de tópicos
8. **Questões por Dificuldade** - Distribuição de complexidade

## 🛠️ Tecnologias Utilizadas
- **Redis + RedisGears** - Banco em memória e captura de eventos
- **PostgreSQL** - Data Warehouse
- **Python + FastAPI** - API e scripts de ingestão
- **Metabase** - Business Intelligence e Dashboards
- **Docker + Docker Compose** - Orquestração de containers

## ⚙️ Como Executar

### Pré-requisitos
- Docker Desktop
- Git
- DBeaver (opcional)

### Execução
```bash
# Subir ambiente
docker-compose up -d
```

### Acessos
- **API Quiz**: http://localhost:8000
- **Swagger**: http://localhost:8000/docs  
- **Metabase**: http://localhost:3000
- **PostgreSQL**: localhost:5432

## 📈 Resultados Obtidos

### ✅ Critério 1: Mecanismo de Ingestão
- RedisGears capturando mudanças automaticamente
- Script Python transferindo dados para PostgreSQL
- Fluxo Redis → PostgreSQL funcionando em tempo real

### ✅ Critério 2: Dashboard Completa
- 8 gráficos obrigatórios implementados
- Dashboard organizado no Metabase
- Dados extraídos do Data Warehouse PostgreSQL

### ✅ Critério 3: Documentação
- PDF da dashboard exportado
- Repositório GitHub com código completo
- Documentação técnica detalhada

## 📊 Dados de Exemplo
O sistema inclui dados de exemplo com:
- **8 questões** de Engenharia de Dados
- **4 usuários** com diferentes perfis
- **31 respostas** simulando diferentes cenários

## 🔧 Estrutura do Projeto
```
trabalho-final-redis/
├── docker-compose.yml          # Orquestração completa
├── apiQuestionRedis/           # API FastAPI (submodule)
├── lab-metabase/               # Configuração Metabase (submodule)  
├── redis-to-postgres/          # Scripts de ingestão (submodule)
└── README.md                   # Documentação
```

## 👥 Equipe
- **Thiago Farias** - Desenvolvimento completo

## 📅 Data de Entrega
Junho 2025