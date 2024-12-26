# Оценка тональности отзывов о музее (Умный городской гид)

Введение: 
Современные музеи активно работают над улучшением качества обслуживания, стремясь предоставлять посетителям комфортные условия для знакомства с культурными ценностями. Однако однако одной из ключевых проблем, с которым сталкиваются такие учреждения, — это анализ отзывов посетителей. Отзывы часто содержат важные замечания, но их анализ вручную затруднен из-за большого объема текстов и субъективности восприятия. В настоящее время использование машинного обучения, связанных с обработкой  текста является важным инструментом для решения подобных задач. 


Актуальность:
1. Улучшение пользовательского опыта в музеях.  
   Современные музеи стремятся не только сохранять культурное наследие, но и предоставлять своим посетителям комфортные и приятные условия. Анализ отзывов позволяет выявить ключевые проблемы, такие как неудобства в навигации, состояние экспонатов или высокую стоимость билетов, и своевременно реагировать на них.  
2. Большие объемы данных.  
   В условиях цифровизации посетители музеев активно оставляют отзывы на платформах, таких как TripAdvisor, Яндекс.Карты и Google Reviews. Однако ручная обработка таких данных становится практически невозможной из-за их огромного объема и разнородности.  
3. Автоматизация рутинных процессов.  
   Автоматический анализ отзывов помогает музеям экономить ресурсы, минимизируя затраты на обработку данных вручную. Это позволяет учреждениям фокусироваться на реализации выявленных рекомендаций.  
4. Выявление ключевых тенденций и проблем.  
   Использование современных технологий NLP, таких как дообученные языковые модели, дает возможность не только классифицировать отзывы по тональности, но и выделять ключевые темы, что помогает музеям сосредотачиваться на наиболее актуальных для посетителей аспектах.  
5. Применимость к другим культурным учреждениям.  
   Разработанные инструменты могут быть масштабированы и использованы для анализа отзывов других культурных объектов, таких как театры, выставочные залы и галереи, что делает их универсальными и полезными для всей отрасли.  

Цель проекта:  
Получить модель, которая будет определять положительный или отрицательный был оставлен отзыв о музее.

Постановка задач: 
1. Выгрузка данных с помощью парсинга сайта.
2. Проивзести разметку отзывов на 0(отрицательный) и 1(положительный).
3. Проанализировать полученный датасет с размеченными данными.
4. Дообучить датасет с использованием готовой модели.

# Данные  
Источником данных являлся сайт, с которого были взяты отзывы. Для этого был написан скрипт для выгрузки отзывов: 1.py.  
Была произведена разметка тональности: 0 - негативный, 1 - позитивный.  

В итоге структура данных:  
- Текст отзыва (столбец Review).  
- Метка тональности (столбец label): 0 - негативный, 1 - позитивный.  

Объем данных:  
- 1470 записей для обучения и тестирования.

# Выбор модели  
cointegrated/rubert-tiny-sentiment-balanced была выбрана по нескольким основным причинам:   
- Адаптация к русскоязычным текстам.  
Модель RuBERT-tiny-sentiment-balanced была специально разработана для анализа тональности текстов на русском языке.      
- Готовность к дообучению.  
rubert-tiny-sentiment-balanced позволяет легко адаптироваться под конкретные задачи.  

# Подход к реализации  

После этого было произведенно разделение данных: обучающая выборка (80%) и тестовая выборка (20%) для оценки качества модели.  
Для начала была произведена балансировка классов с использованием техники undersampling для предотвращения доминирования одного класса. (resample(positive, replace=False, n_samples=len(negative), random_state=42)). 
Далее был использован встроенный метод токенизации AutoTokenizer, обеспечивающий: Padding — выравнивание длины текстов, Truncation — усечение текстов, превышающих максимальную длину.  
После было произведенно дообучение модели с выбранными оптимальными гиперпараметрами: скорость обучения (5e-5), размер батча (16), число эпох (3), использование регуляризации (weight decay = 0.01).  
Также были использованы инструменты: библиотека Hugging Face Transformers для тренировки модели, класс Trainer для упрощения настройки обучения и оценки.  
После этого сохраняется данная модель и далее используется.

# Оценка качества модели  
Для проверки производительности модели использованы метрики: accuracy, f1-score, recall, precision. Были получены результаты: 
Также было сравнение с исходной моделью. 


    
# Результаты  
- Качество модели:  
  - Основные метрики (accuracy, F1-score).  
  - Сравнение с базовой предобученной моделью без дообучения.  

- Примеры анализа:  
  - Классификация конкретных отзывов.  
  - Выделение ключевых тем на основе кластеризации текстов (например, жалобы на стоимость билетов или состояние экспонатов).  

# Выводы и рекомендации  
- Выводы:  
  - Эффективность дообучения модели для анализа отзывов.  
  - Возможности применения модели для автоматического мониторинга обратной связи.  

- Рекомендации для музеев:  
  - Улучшение качества обслуживания по итогам анализа.  
  - Регулярное использование подобного инструмента для мониторинга отзывов.  

# Будущие улучшения  
- Расширение датасета: добавление отзывов из социальных сетей и форумов.  
- Внедрение многозадачного обучения: определение тональности и выделение ключевых тем в одном процессе.  
- Интеграция модели в систему рекомендаций для музеев.  

