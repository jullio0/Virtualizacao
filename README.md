# Disciplina de Virtualização
Projeto Fullstack Kubernetes: Flask + React + PostgreSQL

Grupo: Júlio Cézar Netto de Araújo, Jardson Lúcio Perez da Silva e Clebson Luiz da Silva

# 📦 Sistema de Mensagens - Deploy Kubernetes

Este projeto consiste em uma aplicação fullstack com:

- **Frontend**: React + Vite
- **Backend**: Flask + PostgreSQL
- **Banco de Dados**: PostgreSQL com volume persistente
- Deploy completo em **Kubernetes**, com:
  - Ingress NGINX
  - ConfigMap e Secrets
  - StatefulSet e PVC
  - 2+ réplicas para frontend/backend (alta disponibilidade)

---

## 📁 Estrutura do Projeto

projeto-k8s-deploy/
├── backend/
│   ├── configmap.yaml # Variáveis de ambiente do backend
│   └── deployment.yaml # Deployment do Flask API
│
├── database/
│   ├── secret.yaml # Secrets do PostgreSQL e backend
│   ├── service.yaml # Service do PostgreSQL
│   └── statefulset.yaml # StatefulSet com PVC
│
├── frontend/
│   └── deployment.yaml # Deployment do React App
│
├── ingress/
│   └── ingress.yaml # IngressController com rotas / e /api
│
└── namespace.yaml # Definição dos namespaces usados


---

## ✅ Pré-requisitos

- Minikube ou cluster Kubernetes funcional
- `kubectl` configurado
- Ingress NGINX habilitado (`minikube addons enable ingress`)

---

## 🚀 Etapas de Deploy

### 1. Criar Namespaces

```bash
kubectl apply -f namespace.yaml
```

### 2. Criar Secrets e ConfigMaps

```bash
# Database Secrets
kubectl apply -f database/secret.yaml

# Backend ConfigMap (exemplo de conteúdo)
kubectl create configmap backend-config \
  --from-literal=API_HOST=http://backend:5000 \
  -n app-namespace
```

### 3. Subir o PostgreSQL (StatefulSet + PVC)

```bash
kubectl apply -f database/statefulset.yaml
kubectl apply -f database/service.yaml
```

### 4. Backend - Flask API

```bash
kubectl apply -f backend/deployment.yaml
```

### 5. Frontend - React + NGINX

```bash
kubectl apply -f frontend/deployment.yaml
```

### 6. Ingress Controller

```bash
kubectl apply -f ingress/ingress.yaml
```

# 🌐 Acesso à Aplicação

### Configure seu /etc/hosts:

```bash
sudo echo "127.0.0.1 app.local" >> /etc/hosts
```

### Acesse via navegador:

```bash
http://app.local
```
