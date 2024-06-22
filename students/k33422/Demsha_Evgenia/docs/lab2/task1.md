# Задание 1

`Задача:` Напишите три различных программы на Python, использующие каждый из подходов: threading, multiprocessing и async. Каждая программа должна решать считать сумму всех чисел от 1 до 1000000. Разделите вычисления на несколько параллельных задач для ускорения выполнения.

Напишем функцию, где в цикле(чтобы выглядело еще более неоптимизированно) будем складывать цифры от старта до финиша.
В наивном подходе проскладываем так все числа подряд последовательно:

```Python
--8<-- "laboratory_work_2/asynchrony/task1/naive.py"
```

Теперь распараллелим сложение с помощью потоков. Количество "работников" задается в конфигурационном файле, каждый выполняет 
одинаковое количество работы:

```Python
--8<-- "laboratory_work_2/asynchrony/task1/thrd.py"
```
Теперь используем несколько процессов:

```Python
--8<-- "laboratory_work_2/asynchrony/task1/mltypr.py"
```

Применим асинхронность:

```Python
--8<-- "laboratory_work_2/asynchrony/task1/asnc.py"
```
Протестируем на числах от 1 до 1 млн (результаты в секундах):

| № | naive | threading | multiprocessing | asyncio |
|---|-------|-----------|----------------|---------|
| 1 | 0.04633 | 0.07138     | 0.38010          | 0.05205   |
| 2 | 0.04681 | 0.04980     | 0.31648          | 0.03665   |
| 3 | 0.05610 | 0.05826    | 0.34530         | 0.05594  |

Наивный метод, потоки и асинхронность показали похожий результат. Мультипроцессность медленнее, потому что инициализировать процессы - это долго.

А теперь протестируем на числах от 1 до 100 млн (результаты в секундах):

| № | naive | threading | multiprocessing | asyncio |
|---|-------|-----------|----------------|---------|
| 1 | 5.13490 | 5.74657     | 1.66466          | 5.24498   |
| 2 | 4.33271 | 5.92088     | 1.57994          | 4.26132   |
| 3 | 5.07361 | 6.01106    | 1.74795         | 4.55392  |

Как видно, длительность инициализации процессов окупилась, и скрипт с использованием библиотеки multiprocessing сработал быстрее других.

Возвращать значения из функций - кажется не очень подходящее дело для распараллеливания.
Обычно такие библиотеки используются при задачах ввода-вывода, сервера-клиента. Возможно поэтому здесь они не показали все, на что способны)