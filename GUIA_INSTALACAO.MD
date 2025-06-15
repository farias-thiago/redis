# 📋 Guia de Instalação - Trabalho Final Redis

## 🎯 Objetivo
Sistema completo de ingestão de dados Redis → PostgreSQL com Dashboard Metabase para análise de Quiz de Engenharia de Dados.

---

## 1️⃣ Criando o Projeto Principal com Submodules

### Setup do Projeto - Trabalho Final Redis

#### Criar o projeto principal e Adicionar os repositórios como submodules

```bash
# Clonar repositórios
mkdir trabalho-final-redis
cd trabalho-final-redis

git init

git submodule add https://github.com/commithouse/apiQuestionRedis.git
git submodule add https://github.com/commithouse/lab-metabase.git
git submodule add https://github.com/commithouse/redis-to-postgres.git

ls -la
git submodule status
```

---

## 2️⃣ Configuração Docker

### Subir ambiente completo

```bash
docker-compose up -d
docker ps
```

### Registrar RedisGears

```bash
docker exec -it redis-gears bash
redis-cli RG.PYEXECUTE "$(cat /tmp/registra-gear-interno.py)"
exit
```

### Testar API

```bash
curl http://localhost:8000
# abrir no navegador: http://localhost:8000/docs
```

---

## 3️⃣ Popular com Dados Fake

### Inserir questões e respostas via API

```bash
curl -X POST "http://localhost:8000/questions" \
  -H "Content-Type: application/json" \
  -d @apiQuestionRedis/carga-dados-fake/questions.json

curl -X POST "http://localhost:8000/answers" \
  -H "Content-Type: application/json" \
  -d @apiQuestionRedis/carga-dados-fake/answers.json
```

### Verificar dados no Redis

```bash
docker exec -it redis-gears bash
redis-cli KEYS "*"
exit
```

---

## 4️⃣ Configurar RedisGears

### Registrar script de captura e testar

```bash
docker exec -it redis-gears bash
redis-cli RG.PYEXECUTE "$(cat /tmp/registra-gear-interno.py)"
redis-cli RG.DUMPREGISTRATIONS
exit

# Criar uma chave pessoa para testar
docker exec -it redis-gears redis-cli SET pessoa:01 "teste_engenheiro_dados"
docker exec -it redis-gears redis-cli LRANGE minha_pilha 0 -1
```

### Testar com dados de answers

```bash
curl -X POST "http://localhost:8000/answers" \
  -H "Content-Type: application/json" \
  -d @apiQuestionRedis/carga-dados-fake/answers.json
```

---

## 5️⃣ Configurar PostgreSQL

### Criar tabela para histórico Redis

```bash
docker exec -it postgres-dw psql -U user -d dw
```

```sql
CREATE TABLE IF NOT EXISTS redis_history (
    id SERIAL PRIMARY KEY,
    valor VARCHAR,
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

\dt
\q
```

---

## 6️⃣ Testar Aplicação

### Verificar funcionamento completo

```bash
# Verificar logs de ingestão
docker logs ingestao-redis

# Criar novo dado para testar fluxo
docker exec -it redis-gears redis-cli SET pessoa:04 "engenheiro_ml"

# Verificar se chegou no PostgreSQL
docker exec -it postgres-dw psql -U user -d dw -c "SELECT * FROM redis_history;"
```

---

## 7️⃣ Evidência Aplicação Funcionando

### ✅ Critério de Avaliação 1
**Mecanismo de ingestão funcionado (Redis gear ou outro de preferencia do grupo)**

**Resultados Esperados:**
- RedisGears capturando mudanças em chaves `pessoa:*`
- Script Python transferindo dados da pilha Redis para PostgreSQL
- Dados aparecendo na tabela `redis_history` com timestamps
- Fluxo automático: Redis → RedisGears → Pilha → PostgreSQL

---

## 🌐 URLs de Acesso

- **API Quiz**: http://localhost:8000
- **Swagger API**: http://localhost:8000/docs
- **Metabase**: http://localhost:3000
- **PostgreSQL DW**: localhost:5432 (user: user, senha: senhaForte2025, db: dw)

---

## 🛠️ Troubleshooting

### Problema: API não conecta no Redis
**Solução**: Editar `apiQuestionRedis/main.py`
```python
# Alterar linha:
IP_CONTAINER = "localhost"
# Para:
IP_CONTAINER = "redis-gears"
```

### Problema: Script de ingestão não conecta no PostgreSQL
**Solução**: Editar `redis-to-postgres/pool-pilha-rodar-fora-container.py`
```python
# Alterar linhas:
PG_HOST = 'localhost'
REDIS_HOST = 'localhost'
# Para:
PG_HOST = 'postgres-dw' 
REDIS_HOST = 'redis-gears'
```

### Problema: Metabase não mostra tabelas
**Solução**: Forçar sincronização
1. Administração → Bancos de dados
2. Clicar em "DW Quiz"
3. "Sincronizar esquema de banco de dados"

---

## 📊 Tecnologias Utilizadas

- **Redis + RedisGears** - Banco em memória e captura de eventos
- **PostgreSQL** - Data Warehouse
- **Python + FastAPI** - API e scripts de ingestão
- **Metabase** - Business Intelligence
- **Docker + Docker Compose** - Orquestração