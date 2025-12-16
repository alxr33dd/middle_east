# 🌍 Предиктивный анализ конфликтов на Ближнем Востоке

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success)](https://github.com)

> Машинное обучение для раннего предупреждения гуманитарных кризисов и оптимизации распределения ресурсов

---

## 📋 Содержание

- [О проекте](#о-проекте)
- [Ключевые результаты](#ключевые-результаты)
- [Структура проекта](#структура-проекта)
- [Установка](#установка)
- [Использование](#использование)
- [Данные](#данные)
- [Методология](#методология)
- [Результаты](#результаты)
- [Визуализации](#визуализации)
- [Участие в проекте](#участие-в-проекте)
- [Лицензия](#лицензия)
- [Контакты](#контакты)

---

## 🎯 О проекте

Этот проект разрабатывает предиктивные модели машинного обучения для анализа вооружённых конфликтов на Ближнем Востоке (1997-2024). Цель — создать инструменты для:

- 🚨 **Раннего предупреждения** опасных зон (классификация)
- 📦 **Планирования ресурсов** гуманитарной помощи (регрессия)
- 📊 **Анализа факторов риска** и географических hotspots

### Целевая аудитория

- Гуманитарные организации (UN, Red Cross, MSF)
- Правительственные агентства
- Исследователи конфликтов
- Аналитики данных

---

## 🏆 Ключевые результаты

### Классификация (Random Forest)
```
F1-Score:  0.839  →  Высокий баланс точности и полноты
Recall:    0.847  →  Обнаруживает 85% опасных зон
ROC-AUC:   0.918  →  Отличная способность различать классы
```

### Регрессия (ExtraTrees)
```
R²:    0.774  →  Объясняет 77% вариативности жертв
RMSE:  8.42   →  Типичная ошибка ±8 человек
MAE:   4.18   →  Медианная ошибка 4 человека
```

### Ключевые инсайты
- 📈 Пик конфликта: 2014-2017 (эра ИГИЛ)
- 🌍 Три страны составляют 70%+ жертв: Ирак, Сирия, Йемен
- 🔑 Главные предикторы: ГОД, КОЛИЧЕСТВО СОБЫТИЙ, НАСЕЛЕНИЕ
- 🎯 Готовность к практическому применению: **99.8% данных сохранено**

---

## 📁 Структура проекта

```
MiddleEastConflictAnalysis/
│
├── data/
│   ├── data_set.xlsx              # Исходные данные (ACLED)
│   └── data_set_clean.xlsx        # Очищенные данные
│
├── notebooks/
│   ├── 01_data_exploration.ipynb  # EDA и визуализация
│   ├── 02_preprocessing.ipynb     # Очистка данных
│   ├── 03_modeling.ipynb          # Обучение моделей
│   └── 04_results.ipynb           # Интерпретация результатов
│
├── results/
│   ├── model_comparison_classification.csv
│   ├── model_comparison_regression.csv
│   ├── feature_importance.png
│   └── final_results_dashboard.png
│
├── presentation/
│   └── MiddleEast_Conflict_Analysis.pptx
│
├── requirements.txt               # Python зависимости
├── environment.yml                # Conda environment (опционально)
├── README.md                      # Этот файл
└── LICENSE                        # MIT License

```

---

## 🚀 Установка

### Требования

- Python 3.8+
- Jupyter Notebook/Lab
- 4 GB RAM минимум (рекомендуется 8 GB)
- ~500 MB дискового пространства

### Шаг 1: Клонировать репозиторий

```bash
git clone https://github.com/your-username/middle-east-conflict-analysis.git
cd middle-east-conflict-analysis
```

### Шаг 2: Создать виртуальное окружение

#### Вариант A: venv (рекомендуется)

```bash
# Создать окружение
python -m venv venv

# Активировать (Windows)
venv\Scripts\activate

# Активировать (Mac/Linux)
source venv/bin/activate
```

#### Вариант B: Conda

```bash
# Создать окружение из файла
conda env create -f environment.yml

# Активировать
conda activate conflict-analysis
```

### Шаг 3: Установить зависимости

```bash
pip install -r requirements.txt
```

### Шаг 4: Запустить Jupyter

```bash
jupyter notebook
```

Откройте `notebooks/01_data_exploration.ipynb` для начала.

---

## 📊 Использование

### Быстрый старт

```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier, ExtraTreesRegressor

# 1. Загрузить очищенные данные
df = pd.read_excel('data/data_set_clean.xlsx')

# 2. Разделить на признаки и целевые переменные
X = df.drop(['FATALITIES', 'FATALITIES_capped', 'HIGH_RISK'], axis=1)
y_class = df['HIGH_RISK']
y_reg = df['FATALITIES_capped']

# 3. Обучить модели
clf = RandomForestClassifier(n_estimators=200, max_depth=15, class_weight='balanced')
reg = ExtraTreesRegressor(n_estimators=200, max_depth=15)

clf.fit(X_train, y_train_class)
reg.fit(X_train, y_train_reg)

# 4. Предсказание
risk_prediction = clf.predict(X_new)
casualties_prediction = reg.predict(X_new)
```

### Пример предсказания

```python
# Новые данные: Багдад, март 2025, 45 событий
new_data = {
    'YEAR': 2025,
    'MONTH': 3,
    'EVENTS_capped': 45,
    'POPULATION_EXPOSURE': 250000,
    'COUNTRY_encoded': 3,  # Iraq
    'EVENT_TYPE_encoded': 1  # Battles
}

# Предсказание
high_risk = clf.predict_proba([new_data])[0][1]  # Вероятность HIGH_RISK
expected_casualties = reg.predict([new_data])[0]  # Ожидаемое число жертв

print(f"Вероятность высокого риска: {high_risk:.1%}")
print(f"Ожидаемое число жертв: {expected_casualties:.0f} ± 8")
```

---

## 📦 Данные

### Источник

**ACLED (Armed Conflict Location & Event Data Project)**
- URL: https://acleddata.com
- Период: 1997-2024
- География: 12 стран Ближнего Востока
- Лицензия: Creative Commons Attribution-NonCommercial

### Описание признаков

| Признак | Тип | Описание |
|---------|-----|----------|
| `WEEK` | datetime | Дата начала недели |
| `COUNTRY` | categorical | Страна (Iraq, Syria, Yemen, ...) |
| `REGION` | categorical | Географический регион |
| `EVENT_TYPE` | categorical | Тип события (Battles, Explosions, ...) |
| `SUB_EVENT_TYPE` | categorical | Подтип события |
| `EVENTS` | int | Количество событий за неделю |
| `FATALITIES` | int | Количество жертв за неделю |
| `POPULATION_EXPOSURE` | int | Население под риском |
| `CENTROID_LATITUDE` | float | Широта центроида |
| `CENTROID_LONGITUDE` | float | Долгота центроида |

### Целевые переменные

- **HIGH_RISK** (классификация): 1 если `FATALITIES >= 10`, иначе 0
- **FATALITIES_capped** (регрессия): Количество жертв с кэпированием на 99.9 перцентиле

---

## 🔬 Методология

### 1. Исследовательский анализ данных (EDA)

- Анализ временных трендов (пик 2014-2017)
- Географическая визуализация hotspots
- Корреляционный анализ признаков
- Изучение распределений и выбросов

### 2. Предобработка

```python
# Обработка пропусков
POPULATION_EXPOSURE: медиана по стране × год (28k пропусков)

# Борьба с выбросами
Кэпирование на 99.9 перцентиле (сохранение 99.8% данных)

# Feature Engineering
- Временные признаки: YEAR, MONTH, WEEKDAY, IS_WEEKEND
- Нормализация: LOG_FATALITIES, LOG_EVENTS
- Per capita: FATALITIES_PER_EXPOSURE, EVENTS_PER_EXPOSURE
- Индикаторы: PEAK_WAR_PERIOD (2014-2017)
- Кодирование: Label Encoding для категорий
```

### 3. Моделирование

**Классификация (HIGH_RISK):**
- Logistic Regression (baseline)
- Decision Tree
- Random Forest ⭐ (финальная модель)
- Gradient Boosting
- ExtraTrees

**Регрессия (FATALITIES):**
- Ridge (baseline)
- Decision Tree
- Random Forest
- Gradient Boosting
- ExtraTrees ⭐ (финальная модель)

**Оптимизация:**
- GridSearchCV с 5-fold cross-validation
- Метрики: F1-Score (классификация), RMSE (регрессия)

### 4. Валидация

- Train/Test split: 80/20
- Стратификация для классификации
- Cross-validation для оценки стабильности
- Анализ важности признаков

---

## 📈 Результаты

### Сравнение моделей

#### Классификация

| Модель | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|--------|----------|-----------|--------|----------|---------|
| **Random Forest (Tuned)** | **0.854** | **0.831** | **0.847** | **0.839** | **0.918** |
| Gradient Boosting | 0.841 | 0.819 | 0.835 | 0.827 | 0.905 |
| ExtraTrees | 0.838 | 0.815 | 0.831 | 0.823 | 0.902 |
| Logistic Regression | 0.782 | 0.753 | 0.788 | 0.770 | 0.851 |

#### Регрессия

| Модель | RMSE | MAE | R² |
|--------|------|-----|-----|
| **ExtraTrees (Tuned)** | **8.42** | **4.18** | **0.774** |
| Random Forest | 9.15 | 4.67 | 0.751 |
| Gradient Boosting | 9.38 | 4.82 | 0.743 |
| Ridge | 12.34 | 6.45 | 0.612 |

### Важность признаков (Топ-6)

```
1. YEAR (18%)              → Временная эволюция конфликта
2. EVENTS_capped (16%)     → Интенсивность событий
3. POPULATION_EXPOSURE (13%) → Население под риском
4. CENTROID_LAT/LON (9%)   → Географические hotspots
5. COUNTRY_encoded (7%)    → Страна-специфические паттерны
6. EVENT_TYPE_encoded (6%) → Тип события (битвы vs протесты)
```

---

## 🎨 Визуализации

Проект включает 10+ визуализаций:

1. **Эволюция конфликта** (линейный график + boxplot)
2. **География насилия** (интерактивная карта с Plotly)
3. **Топ-10 стран** по жертвам (bar chart)
4. **Типы событий** (violin plot)
5. **Корреляционная матрица** (heatmap)
6. **Важность признаков** (horizontal bar chart)
7. **Confusion Matrix** (для классификации)
8. **Предсказания vs реальность** (scatter plot для регрессии)
9. **Сравнение моделей** (bar charts)
10. **Final Dashboard** (сводный отчёт)

Все визуализации сохранены в `results/` в формате PNG (300 DPI).

---

## 🛠️ Технологии

- **Python 3.8+**
- **Data Processing:** pandas, numpy
- **Visualization:** matplotlib, seaborn, plotly
- **Machine Learning:** scikit-learn
- **Geospatial:** folium, geopandas (опционально)
- **Other:** openpyxl (для Excel), scipy

---

## 🤝 Участие в проекте

Contributions приветствуются! Вот как вы можете помочь:

### Идеи для улучшений

1. **Временные ряды:** Добавить LSTM для предсказания будущих недель
2. **NLP:** Анализ текстовых описаний событий
3. **Real-time:** Интеграция с API ACLED для актуальных данных
4. **Web App:** Создать интерактивный дашборд (Streamlit/Dash)
5. **SHAP values:** Добавить интерпретируемость предсказаний

### Как внести вклад

1. Fork репозиторий
2. Создайте ветку (`git checkout -b feature/AmazingFeature`)
3. Commit изменения (`git commit -m 'Add AmazingFeature'`)
4. Push в ветку (`git push origin feature/AmazingFeature`)
5. Откройте Pull Request

---

## ⚠️ Ограничения и этические соображения

### Технические ограничения

- **Temporal drift:** Паттерны конфликтов меняются со временем
- **Несбалансированность:** 75% данных — класс LOW_RISK
- **Long-tail:** Редкие катастрофы (100+ жертв) сложны для предсказания
- **Качество данных:** Неточности в координатах, пропуски в населении

### Этические соображения

⚠️ **ВАЖНО:** Эта модель — инструмент поддержки решений, НЕ замена человеческого анализа.

- ✅ Всегда верифицируйте предсказания с локальными источниками
- ⚠️ False negatives (пропуск опасной зоны) критичнее false positives
- 🔒 Данные о конфликтах чувствительны — соблюдайте конфиденциальность
- 🤝 Используйте модель для помощи людям, не для манипуляций

---

## 📝 Лицензия

Этот проект лицензирован под MIT License - см. файл [LICENSE](LICENSE) для деталей.

**Данные:** ACLED данные доступны под лицензией CC BY-NC.

---

## 📧 Контакты

**Автор:** [Alexey]
- Email: @thrashtilllyoudie@gmail.com
- GitHub: [@alxr33dd](https://github.com/your-username)

---

## 🙏 Благодарности

- **ACLED** за предоставление данных
- **scikit-learn** сообщество за отличную библиотеку
- Всем, кто работает над предотвращением конфликтов

---

## 📚 Цитирование

Если вы используете этот проект в исследовании, пожалуйста, цитируйте:

```bibtex
@misc{middle_east_conflict_analysis,
  author = {alxr33d},
  title = {Predictive Analysis of Middle East Conflicts using Machine Learning},
  year = {2024},
  publisher = {GitHub},
  url = {https://github.com/your-username/middle-east-conflict-analysis}
}
```

---

## 📊 Статистика проекта

![Lines of Code](https://img.shields.io/badge/Lines%20of%20Code-2500%2B-blue)
![Data Points](https://img.shields.io/badge/Data%20Points-113k%2B-green)
![Models Trained](https://img.shields.io/badge/Models%20Trained-10-orange)
![Accuracy](https://img.shields.io/badge/F1%20Score-0.839-brightgreen)

---

<div align="center">

**⭐ Если проект был полезен, поставьте звезду! ⭐**

Made with ❤️ for humanitarian purposes

</div>