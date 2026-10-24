Исправленный форк оригинального репозитория, файл с графиками и прогоном см. в `results/benchmark_results.ipynb`.

Вопросы и комментарии:
 - В репозитории нет результатов измерений: нет CSV-файлов, папки results (то что есть создала я) и скриптов построения графиков; есть только один коммит с исходным кодом от 2025 года. В статье сказано, что результаты сохранены в формате CSV, но эти файлы не опубликованы, а опубликованный код косячный и грустный((
 - Конфигурация 6-to-8, которую рекомендуют авторы, в коде отсутствует: для каждого алгоритма есть классическая версия и 10 оптимизированных вместо 11.
 - Тест производительности использует размеры массивов, равные степеням двойки от 1 024 до 1 048 576, а в статье приведены размеры от 10 000 до 1 000 000. Как бы вроде и округление, но оно не проговорено.
 - Измеряется только реальное время выполнения, хотя в статье указано процессорное и реальное время.
 - Как есть код не собирается: отсутствуют файлы bitonic_sort.h, bitonic_sort.cc, benchmark_bitonic.cc и merge_sort_demo.cc, на которые ссылаются код и файл сборки. Я не прогаю на плюсах, поэтому чинила нейронкой.
 - Я слышала, что довольно часто маленькие подмассивы сортируют чем-то типа сортировки вставками, и это помогает эффективности, интересно, почему авторы с чем-то подобным не сравнились.
 - Скорее придирка, но в статье не определено, что такое почти отсортированные массивы, из кода получается установить, что они перестанавливали 5% массива.

Из того, что выловила нейронка (я поверхностно проверила, не получилось более детально, потому что я не знаю плюсов):
 - Макрос регистрации тестов объявлен как REGISTERBENCHMARK, а вызывается как REGISTER_BENCHMARK; имена функций не совпадают с объявленными.
 - Для сетей VarSort каждый малый подмассив копируется в новый массив std::vector с длиной в начале и затем копируется обратно. `Комментарий от меня: эти телодвижения не обсуждаются в статье и могут частично объяснять слабые результаты VarSort.`

# alphadev-merge-quick-sort

## Introduction

This project improves Merge Sort and Quick Sort performance by using optimized sorting networks, discovered by AlphaDev, as base cases for small subarrays.

## Requirements

- Linux system or WSL Ubuntu
- Bazel version 7.4.1
- Clang compiler

## Build Instructions

To build the project, run the following command:

```bash
CC=clang bazel build -c opt --cxxopt='-std=c++17' :benchmark
```

## Running the Benchmark

To execute the benchmark and generate CSV output:

```bash
./bazel-bin/benchmark --benchmark_format=console --benchmark_out_format=csv --benchmark_out=results/benchmark_$(date +%Y%m%d_%H%M%S).csv
```

This command generates CSV files with timestamps in their names, facilitating easy data analysis.

## Results

- **Location**: `results` directory
- **Format**: CSV files with timestamped names
- **Content**: Benchmark results for analysis
