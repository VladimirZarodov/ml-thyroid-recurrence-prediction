# Thyroid Cancer Recurrence Prediction / Предсказание рецидива рака щитовидной железы

[![Python](https://img.shields.io/badge/Python-3.12-blue)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)](https://jupyter.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-yellowgreen)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.x-red)](https://xgboost.readthedocs.io/)

Проект посвящён анализу клинических данных пациентов с раком щитовидной железы и построению моделей машинного обучения для предсказания рецидива заболевания.

## О проекте

Цель — на основе клинических признаков (возраст, пол, стадия, TNM, гистология и т.д.) предсказать, возникнет ли рецидив у пациента. В работе сравниваются три модели:

- **XGBoost**
- **Linear SVM**
- **Random Forest**

Ноутбук [`Pred_thyroid`](./Pred_thyroid.ipynb) содержит полный пайплайн: загрузку данных, предобработку, обучение моделей и визуализацию результатов.


## Данные

Используется файл [`filtered_thyroid_data`](./filtered_thyroid_data.csv), содержащий следующие признаки:

| Признак | Описание |
|---------|----------|
| Age | Возраст пациента |
| Gender | Пол (F/M) |
| Hx Radiothreapy | Радиотерапия в анамнезе (No/Yes) |
| Adenopathy | Аденопатия (No/Right/Extensive/Left/Bilateral/Posterior) |
| Pathology | Гистологический тип (Micropapillary, Papillary, Follicular, Hurthel cell) |
| Focality | Фокальность (Uni-Focal / Multi-Focal) |
| Risk | Группа риска (Low / Intermediate / High) |
| T | Категория T (T1a, T1b, T2, T3a, T3b, T4a, T4b) |
| N | Категория N (N0, N1a, N1b) |
| M | Категория M (M0, M1) |
| Stage | Стадия (I, II, III, IVA, IVB) |
| Response | Ответ на лечение (Excellent, Indeterminate, Biochemical Incomplete, Structural Incomplete) |
| **Recurred** | **Целевая переменная:** No / Yes |

## Методология

1. **Предобработка данных:**
   - Категориальные признаки закодированы с помощью `LabelEncoder`.
   - Целевая переменная `Recurred` преобразована в бинарную: `No → 0`, `Yes → 1`.
   - Данные разделены на обучающую и тестовую выборки в пропорции 80/20 (`random_state=42`).
   - Признаки нормализованы с помощью `StandardScaler`.

2. **Обучение моделей:**
   - XGBoost (`XGBClassifier`, `eval_metric='logloss'`)
   - Linear SVM (`LinearSVC`, `max_iter=10000`)
   - Random Forest (`RandomForestClassifier`, `n_estimators=100`)

3. **Оценка качества:**
   - Accuracy, precision, recall, f1-score для каждого класса.
   - Визуальное сравнение точности на обучающей и тестовой выборках.

## 📈 Результаты

| Модель | Accuracy | Precision (0/1) | Recall (0/1) | F1-score (0/1) |
|--------|----------|----------------|--------------|----------------|
| **XGBoost** | **0.974** | 0.98 / 0.95 | 0.98 / 0.95 | 0.98 / 0.95 |
| Linear SVM | 0.922 | 0.93 / 0.88 | 0.97 / 0.79 | 0.95 / 0.83 |
| **Random Forest** | **0.974** | 0.97 / 1.00 | 1.00 / 0.89 | 0.98 / 0.94 |

*Класс 0 — рецидива не было, класс 1 — рецидив был.*

Лучшие результаты показали **XGBoost** и **Random Forest** с точностью **97.4%**.  
График сравнения точности моделей строится в конце ноутбука.
