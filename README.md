<div align="center">

# 📰 kube-news

**Portal de notícias construído para demonstrar conceitos de containerização e deploy em Kubernetes.**

<br/>

<img src="https://img.shields.io/badge/Node.js-20-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js"/>
<img src="https://img.shields.io/badge/Express-4-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express"/>
<img src="https://img.shields.io/badge/PostgreSQL-15-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
<img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"/>
<img src="https://img.shields.io/badge/Kubernetes-Ready-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" alt="Kubernetes"/>
<img src="https://img.shields.io/badge/Prometheus-Metrics-E6522C?style=for-the-badge&logo=prometheus&logoColor=white" alt="Prometheus"/>

</div>

---

## 🧱 Stack

| Camada | Tecnologia |
|--------|-----------|
| ⚙️ Runtime | Node.js 20 (Alpine) |
| 🚀 Framework | Express 4 |
| 🖼️ Template engine | EJS |
| 🗄️ ORM | Sequelize 6 |
| 🐘 Banco de dados | PostgreSQL 15 |
| 📊 Métricas | Prometheus (`express-prom-bundle` + `prom-client`) |

---

## 🔐 Variáveis de ambiente

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `DB_HOST` | Host do PostgreSQL | `postgres` |
| `DB_DATABASE` | Nome do banco | `kubedevnews` |
| `DB_USERNAME` | Usuário do banco | `kubedevnews` |
| `DB_PASSWORD` | Senha do banco | `Pg#123` |

---

## 🐳 Executar com Docker Compose

```bash
docker compose up --build
```

A aplicação ficará disponível em `http://localhost:8080`.

> 💡 O Compose aguarda o PostgreSQL passar no healthcheck antes de iniciar a aplicação.

---

## ☸️ Deploy no Kubernetes

### ✅ Pré-requisitos

- Cluster Kubernetes em execução (`kubectl` configurado)
- Imagem `cadugr/kube-news:v2` disponível (ou ajuste o campo `image` no manifesto)

### 📦 Aplicar os manifestos

```bash
kubectl apply -f k8s/manifests.yaml
```

Isso cria os seguintes recursos:

| Recurso | Tipo | Descrição |
|---------|------|-----------|
| 🔑 `kube-news-secret` | Secret | Credenciais do banco |
| 💾 `postgres-pvc` | PersistentVolumeClaim | Volume de 5 Gi para o PostgreSQL |
| 🐘 `postgres` | Deployment + Service | Banco de dados |
| 📰 `kube-news` | Deployment (2 réplicas) + Service (LoadBalancer) | Aplicação |

### 🔍 Verificar status

```bash
kubectl get pods
kubectl get svc kube-news   # obter o EXTERNAL-IP
```

### 🗑️ Remover recursos

```bash
kubectl delete -f k8s/manifests.yaml
```

---

## 🌐 Endpoints disponíveis

### Aplicação

| Método | Caminho | Descrição |
|--------|---------|-----------|
| `GET` | `/` | Lista todas as notícias |
| `GET` | `/post` | Formulário para nova notícia |
| `POST` | `/post` | Cria uma nova notícia |
| `GET` | `/post/:id` | Exibe uma notícia específica |
| `POST` | `/api/post` | Cria notícias em lote (JSON) |

### 🩺 Observabilidade

| Método | Caminho | Descrição |
|--------|---------|-----------|
| `GET` | `/health` | Liveness probe — retorna `{"state":"up","machine":"<hostname>"}` |
| `GET` | `/ready` | Readiness probe — retorna `200 Ok` quando pronto |
| `GET` | `/metrics` | Métricas Prometheus |

### 🔥 Chaos Engineering

| Método | Caminho | Descrição |
|--------|---------|-----------|
| `PUT` | `/unhealth` | Força estado unhealthy |
| `PUT` | `/unreadyfor/:seconds` | Força estado not-ready por N segundos |

---

## 📁 Estrutura do projeto

```
kube-news/
├── 🐳 docker-compose.yml          # Stack local com PostgreSQL
├── ☸️  k8s/
│   └── manifests.yaml             # Todos os recursos Kubernetes
├── 🔌 popula-dados.http           # Exemplos de requisições HTTP (REST Client)
└── 📦 src/
    ├── Dockerfile
    ├── .dockerignore
    ├── package.json
    ├── server.js                  # Entry point — rotas e configuração do Express
    ├── middleware.js               # Middleware de contagem de requisições
    ├── system-life.js              # Rotas /health, /ready e middleware de liveness
    ├── models/
    │   └── post.js                # Model Sequelize para Post
    ├── views/
    │   ├── index.ejs              # Listagem de notícias
    │   ├── view-news.ejs          # Detalhe da notícia
    │   └── edit-news.ejs          # Formulário de criação
    └── static/                    # Arquivos estáticos (CSS, imagens)
```

---

<div align="center">

Feito com ☕ para fins educacionais — **AIOps Aula**

</div>
