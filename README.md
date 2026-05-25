# ChemAI: Predict the Cure — Team 29_noname

Репозиторий команды `29_noname` для учебной практики и хакатона **ChemAI: Predict the Cure**.

## Описание задачи

Цель проекта - построить ML-pipeline, который предсказывает биологическую активность химических соединений.

Для каждого соединения нужно предсказать три показателя:

* `IC50` - концентрация, при которой вещество подавляет 50% активности вируса;
* `CC50` - концентрация, при которой вещество токсично для 50% клеток;
* `SI` - индекс селективности.

Качество решения оценивается по метрике **RMSE**. Чем меньше значение RMSE, тем лучше результат.

## Структура проекта

```text
.
├── data/
├── notebooks/
├── presentation/
├── src/
├── submissions/
├── README.md
└── requirements.txt
```

## Данные

Данные хакатона не хранятся в репозитории, так как они являются закрытыми.

Для запуска решения нужно положить файлы в папку `data/`:

```text
data/
├── train.csv
├── test.csv
└── sample_submission.csv
```

В Colab данные загружаются из закрытой Google Drive папки через `gdown`. В публичном notebook ссылка на Drive не хранится, вместо неё используется placeholder:

```python
DRIVE_FOLDER_URL = "PASTE_GOOGLE_DRIVE_FOLDER_LINK_HERE"
```

## Основные notebook-файлы

Основной notebook с финальным решением:

```text
notebooks/ChemAI_Predict_the_Cure_29_noname_final.ipynb
```

Baseline-версия оставлена отдельно:

```text
notebooks/01_baseline.ipynb
```

Дальше основным считается именно файл `ChemAI_Predict_the_Cure_29_noname_final.ipynb`.

## Запуск проекта

Проект в основном запускался в **Google Colab**.

Порядок запуска:

1. Открыть основной notebook.
2. Вставить ссылку на закрытую Google Drive папку с данными в `DRIVE_FOLDER_URL`.
3. Запустить ячейки сверху вниз.
4. Проверить, что файлы появились в папке `data/`.
5. Дождаться создания submission-файла в папке `submissions/`.

При локальном запуске нужно установить зависимости:

```bash
pip install -r requirements.txt
```

## Baseline

Сначала был сделан baseline на модели `LinearRegression`.

Он нужен был, чтобы проверить полный путь:

* загрузка данных;
* подготовка признаков;
* обучение модели;
* расчёт RMSE;
* создание submission.

Результат baseline:

```text
validation mean RMSE: 571.977
```

Baseline работал, но давал слабое качество и отрицательные предсказания, поэтому дальше были проверены более сильные модели.

## Финальное решение

В финальном решении используется target-specific подход с `sklearn.Pipeline`.

Для каждого таргета обучается отдельная модель `RandomForestRegressor`:

* отдельная модель для `IC50`;
* отдельная модель для `CC50`;
* отдельная модель для `SI`.

Для `SI` дополнительно применяется логарифмирование через `np.log1p()`, потому что этот таргет имеет скошенное распределение и редкие большие значения.

Важно: `SI` не считается по формуле `CC50 / IC50`. В рамках решения он предсказывается отдельной моделью, как отдельная целевая переменная.

Финальный submission:

```text
submissions/submission_pipeline_best_params.csv
```

Лучший результат:

```text
validation mean RMSE: 323.025
```

## Сравнение результатов

```text
LinearRegression baseline:         validation RMSE 571.977
Tuned RandomForest:                validation RMSE 365.798
Cleaned + scaled RandomForest:     validation RMSE 361.908
Target-specific + log(SI):         validation RMSE 325.054
Per-target parameter tuning:       validation RMSE 323.456
Final pipeline with best params:   validation RMSE 323.025
```

Меньшее значение RMSE означает лучшее качество.

## Воспроизводимость

Для воспроизводимости в проекте:

* используется фиксированный `random_state=42`;
* данные загружаются в одну папку `data/`;
* submission сохраняется в папку `submissions/`;
* формат submission проверяется через `sample_submission.csv`;
* закрытые CSV-файлы не коммитятся в GitHub;
* все шаги предобработки обёрнуты в `sklearn.Pipeline`.

Перед сохранением notebook в GitHub нужно очищать outputs, чтобы случайно не сохранить ссылки или ID файлов из Google Drive.

## Команда

Команда: `29_noname`  
Номер команды: `29`
