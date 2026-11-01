# Runbook

## Start locally / Локално стартиране

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

## Start with Docker / Стартиране с Docker

**Build**
```bash
docker build -t notes-api:0.3.0 .
```

**Run**
```bash
docker run -d --name notes -p 8000:8000 notes-api:0.3.0
```

**Logs**
```bash
docker logs -f notes
```

**Stop**
```bash
docker stop notes
docker rm notes
```

**Health status**
```bash
docker inspect --format '{{.State.Health.Status}}' notes   # трябва: healthy
```

## Check that it works / Проверка, че работи

Отворете http://localhost:8000/health (в Codespaces: таб **Ports** → порт 8000 → Open in Browser и добавете `/health` към адреса).

Трябва да видите:
```json
{"status": "ok"}
```

Или от терминала:
```bash
curl http://localhost:8000/health
```

## Run the tests / Тестове

```bash
source .venv/bin/activate
pip install -r requirements.txt
pytest -v
```

## When something breaks / Когато нещо се счупи

1. **Работи ли процесът?** `docker ps -a` (за Docker) или проверете терминала, в който е пуснат uvicorn. Ако контейнерът е спрял, вижте `docker logs notes`.
2. **Какво казват логовете?** `docker logs notes` – търсете traceback, `ModuleNotFoundError`, грешен път `app.main:app`.
3. **Слуша ли на правилния адрес и порт?** Хостът трябва да е `0.0.0.0`, а не `127.0.0.1`. Портът трябва да е пренасочен (`-p 8000:8000`) и да няма друг процес на 8000