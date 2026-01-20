# Sausage Store

Учебный fullstack-проект: интернет-магазин (Angular + Spring Boot). Исходное приложение — шаблон; в репозитории — доработки по деплою и CI/CD.

---

## Доработки (портфолио)

| Область | Что сделано |
|--------|---------------|
| **Helm / Kubernetes** | Helm-чарт для развёртывания приложения в Kubernetes (frontend, backend, backend-report, PostgreSQL, Ingress, секреты). |
| **CI/CD** | GitLab CI/CD: пайплайны сборки frontend/backend, релиз 0.0.1, кэширование Maven, исправления сборки. |
| **Качество кода** | Интеграция SonarQube (SAST) для фронтенда в пайплайне. |
| **Уведомления** | Этап в CI с уведомлениями в Telegram при успешной сборке. |

Остальная кодовая база (Angular, Spring Boot, тесты) — из учебного шаблона.

---

## Стек технологий

| Часть      | Технологии                          |
|-----------|--------------------------------------|
| Frontend  | TypeScript, Angular                  |
| Backend   | Java 16, Spring Boot, Spring Data    |
| БД        | H2 (dev), PostgreSQL (prod)         |
| Инфра     | Kubernetes, Helm                     |
| CI/CD     | GitLab CI, SonarQube                 |

## Структура репозитория

```
sausage-store/
├── frontend/              # Angular SPA (шаблон)
├── backend/               # Spring Boot API (шаблон)
├── sausage-store-chart/   # Helm chart — моя разработка
├── .gitlab-ci.yml         # CI/CD — моя разработка
└── README.md
```

## Запуск локально

### Backend

Нужны Java 16 и Maven.

```bash
cd backend
mvn package
cd target
java -jar sausage-store-0.0.1-SNAPSHOT.jar
```

Бэкенд: `http://localhost:8080`. H2 Console: `http://localhost:8080/h2-console`.

### Frontend

Нужны Node.js и npm.

```bash
cd frontend
npm install
npm run build
npx http-server ./dist/frontend/ -p 4200 --proxy http://localhost:8080
```

Откройте в браузере: [http://localhost:4200](http://localhost:4200).

## Развёртывание в Kubernetes (Helm)

Чарт и инструкции: [sausage-store-chart/](sausage-store-chart/README.md).

```bash
helm lint ./sausage-store-chart
helm upgrade --install sausage-store ./sausage-store-chart \
  --set frontend.fqdn="your-domain.example.com" \
  --namespace sausage-store --create-namespace
```

Пароли и учётные данные registry передавайте через `--set` или внешние секреты, не храните их в репозитории.

## Лицензия

Учебный проект.
