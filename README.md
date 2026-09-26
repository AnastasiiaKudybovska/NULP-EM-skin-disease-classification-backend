# Skin Insight AI — бекенд

FastAPI API для класифікації зображень шкіри, XAI-пояснень та історії користувачів. Дані й зображення зберігаються в MongoDB/GridFS.

## Запуск через Docker Compose

Потрібні Docker і Docker Compose, а також файл навченої моделі `app/classification_models/resnet_model.h5`. Модель не входить до Git-репозиторію (`*.h5` у `.gitignore`). Покладіть її за цим шляхом **до** збирання образу. Назва важлива: саме `resnet_model.h5` завантажує `app/classification_models/model_loader.py`.

З каталогу бекенду виконайте:

```bash
docker compose up --build
```

API буде доступне на <http://localhost:8000>, документація OpenAPI — на <http://localhost:8000/docs>. Compose запускає API та MongoDB; база зберігається в Docker volume `mongo_data`. Під час першого збирання завантажуються великі ML-залежності, тому воно може тривати кілька хвилин. API збирається для `linux/amd64`; на Apple Silicon Docker використовуватиме емуляцію.

Для локального PoC Compose задає `SECRET_KEY` за замовчуванням. За потреби передайте власний:

```bash
SECRET_KEY=your-local-secret docker compose up --build
```

Зупинити стек: `docker compose down`. Видалити також дані MongoDB: `docker compose down -v`.

## Запуск разом із фронтендом

Якщо обидва репозиторії лежать поруч у каталозі з назвами `NULP-EM-skin-disease-classification-backend` і `NULP-EM-skin-disease-classification-frontend`, можна запустити весь стек однією командою з каталогу фронтенду:

```bash
cd ../NULP-EM-skin-disease-classification-frontend
docker compose up --build
```

Фронтенд буде на <http://localhost:3000>. Не запускайте обидва Compose-стеки одночасно: вони використовують однаковий порт API `8000`.
