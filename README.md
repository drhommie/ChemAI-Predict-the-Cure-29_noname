# ChemAI: Predict the Cure — Team 29_noname

Репозиторий команды 29_noname для учебной практики и хакатона ChemAI: Predict the Cure.

## Описание задачи

Нужно построить ML-pipeline и предсказать три показателя для химических соединений:

- IC50 — концентрация, при которой вещество подавляет 50% активности вируса;
- CC50 — концентрация, при которой вещество токсично для 50% клеток;
- SI — индекс селективности.

Метрика оценки: RMSE.

## Структура проекта

data/
notebooks/
presentation/
src/
submissions/
README.md
requirements.txt

## Данные

Данные хакатона не хранятся в репозитории — они закрытые.

Нужно положить файлы в папку data/:
- train.csv
- test.csv
- sample_submission.csv

## Запуск

Проект запускается в Google Colab.

1. Открыть notebook из папки notebooks/.
2. Вставить ссылку на Google Drive с данными в DRIVE_FOLDER_URL.
3. Запустить ячейки сверху вниз.

При локальном запуске:
pip install -r requirements.txt

## Команда

Команда: 29_noname | Номер команды: 29
