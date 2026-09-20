# Desafio: Fundamentos de Kubernetes na Prática

Este projeto implementa uma API (PostgREST) integrada a um banco de dados (PostgreSQL) dentro de um cluster Kubernetes local, com configuração externalizada e persistência de dados comprovada.

## Ferramenta de Cluster Utilizada
- **Ambiente:** WSL (Ubuntu)
- **Cluster Local:** k3s / Kubernetes integrado

## Ordem de Aplicação dos Manifests
Os manifests foram separados e numerados para garantir a ordem correta de dependências. Execute os comandos na seguinte ordem:

```bash
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-config-secret.yaml
kubectl apply -f 03-postgres.yaml
kubectl apply -f 04-api.yaml
```
## Como Testar
Inserir dados no banco:

```bash
kubectl exec -n desafio-kubernetes deployment/postgres -- psql -U postgres -d appdb -c "CREATE TABLE IF NOT EXISTS mensagem (id serial PRIMARY KEY, texto text); INSERT INTO mensagem (texto) VALUES ('Teste OK');"
```
Aceder à API:

```bash
kubectl port-forward svc/api-service 8080:80 -n desafio-kubernetes
```
Apagar o pod do banco para validar o PVC:

```bash
kubectl delete pod -l app=postgres -n desafio-kubernetes
```
