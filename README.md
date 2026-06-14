# Cross-Validation, Feature Selection & Hyperparameter Optimization

Реализация с нуля методов валидации, отбора признаков и оптимизации гиперпараметров с последующим сравнением с библиотечными реализациями sklearn.

## Задача

Предсказание стоимости аренды жилья в Нью-Йорке.  
Основной фокус - не на самой модели, а на **методологии оценки и улучшения модели**.

## Что реализовано с нуля

### Разделение данных
| Метод | Описание |
| :--- | :--- |
| `my_train_test_split` | Случайное разбиение на train/test |
| `train_val_test_split` | Разбиение на train/val/test |
| `train_test_date_split` | Разбиение по дате (временной срез) |
| `train_val_test_date_split` | Трёхчастное разбиение по дате |

### Кросс-валидация
| Метод | Описание |
| :--- | :--- |
| `kfold_split` | K-Fold Cross-Validation |
| `grouped_split` | Group K-Fold (по bedrooms) |
| `stratified_split` | Stratified K-Fold (для несбалансированных классов) |
| `time_series_split` | Time Series Split (сохранение порядка) |

### Отбор признаков
| Метод | Описание |
| :--- | :--- |
| **Lasso (встроенный)** | L1-регуляризация для отбора признаков |
| `simple_feature_selection` | Фильтр: удаление пропусков + корреляция с целевой |
| `permutation_importance` | Перестановочная важность признаков |
| **SHAP** | Shapley values (LinearExplainer) |

### Оптимизация гиперпараметров
| Метод | Описание |
| :--- | :--- |
| `MyGridSearch` | Полный перебор всех комбинаций |
| `MyRandomSearch` | Случайный поиск |
| **Optuna** | Байесовская оптимизация |

## Сравнение методов отбора признаков

| Метод | Время (сек) | Лучшие признаки |
| :--- | :--- | :--- |
| Simple (корреляция) | **3.7** | bathrooms, bedrooms, doorman, elevator |
| Permutation Importance | 12.5 | bathrooms, doorman, bedrooms, laundry |
| SHAP | 12.2 | bathrooms, doorman, bedrooms, laundry |

> SHAP и Permutation дали похожий порядок важности признаков.

## Результаты Lasso на валидации

| Набор признаков | MAE (val) | R² (val) |
| :--- | :--- | :--- |
| Все признаки | 922 | 0.443 |
| Top-10 (Lasso) | 917 | 0.443 |
| Top-10 (Permutation) | 917 | 0.440 |
| Top-10 (SHAP) | **912** | **0.445** |

**Лучший результат:** SHAP + Lasso (MAE = 912, R² = 0.445)

## Оптимизация гиперпараметров (ElasticNet)

| Метод | Лучший alpha | Лучший l1_ratio | R² (val) |
| :--- | :--- | :--- | :--- |
| Grid Search | 0.001 | 0.2 | 0.307 |
| Random Search | 0.0008 | 0.90 | 0.358 |
| **Optuna** | **0.00039** | **0.40** | **0.349** |

> Optuna дала лучший баланс качества и эффективности поиска.


## Данные

Источник: [Two Sigma Connect: Rental Listing Inquiries](https://www.kaggle.com/c/two-sigma-connect-rental-listing-inquiries/data)

1. Скачайте `train.json` и `test.json` с Kaggle
2. Поместите их в папку `data/`
3. Запустите ноутбук

