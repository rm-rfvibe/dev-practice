# Sausage Store Helm Chart

Helm-чарт для развертывания приложения Sausage Store в Kubernetes.

## Структура

```
sausage-store-chart/
├── Chart.yaml
├── values.yaml
├── templates/
│   └── docker-secret.yaml
└── charts/
    ├── backend/
    ├── backend-report/
    └── frontend/
```

## Требования

- Kubernetes 1.19+
- Helm 3.x
- Образы приложения (frontend, backend, backend-report) в доступном container registry

## Установка

### Локальная установка из репозитория

```bash
# Клонировать репозиторий и перейти в каталог
git clone https://github.com/YOUR_USERNAME/sausage-store.git
cd sausage-store

# Установка с переопределением значений (укажите свой registry и домен)
helm upgrade --install sausage-store ./sausage-store-chart \
  --set environment=production \
  --set imageCredentials.registry="ghcr.io" \
  --set imageCredentials.username="your-username" \
  --set imageCredentials.password="your-password" \
  --set frontend.image="ghcr.io/your-username/sausage-frontend" \
  --set frontend.fqdn="your-domain.example.com" \
  --set backend.image="ghcr.io/your-username/sausage-backend" \
  --set backend-report.image="ghcr.io/your-username/sausage-backend-report" \
  --set backend.database.password="your-db-password" \
  --namespace sausage-store \
  --create-namespace \
  --atomic --timeout 15m
```

### Установка из Helm-репозитория (если чарт опубликован)

```bash
helm repo add sausage-store https://your-helm-repo.example.com/charts
helm repo update
helm upgrade --install sausage-store sausage-store/sausage-store-chart \
  --set frontend.fqdn="your-domain.example.com" \
  --namespace sausage-store --create-namespace
```

## Проверка

```bash
# Линтер
helm lint ./sausage-store-chart

# Просмотр сгенерированных манифестов
helm template sausage-store ./sausage-store-chart

# Статус релиза после установки
helm list -n sausage-store
kubectl get pods -n sausage-store
```

## Упаковка и публикация

```bash
# Упаковка чарта
helm package ./sausage-store-chart

# Загрузка в Helm-репозиторий (пример для OCI registry)
helm push sausage-store-chart-0.1.0.tgz oci://ghcr.io/your-username/charts
```

## Секреты

- Учётные данные registry и БД **не** храните в values.yaml в открытом виде.
- Передавайте их через `--set`, Helm secrets или внешние системы (Vault, Sealed Secrets).
