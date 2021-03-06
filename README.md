## Условие задачи

Предложите алгоритм, который позволит находить на изображении участки с градиентной заливкой (участки прямоугольной формы, стороны прямоугольника параллельны границам изображения).

## Решение

Градиентная заливка - эта та область, в которой цвет меняется плавно, т.е. соседние пиксели незначительно отличаются друг от друга. Алгоритм нахождения этой заливки основан на нахождении наибольшей нулевой подматрицы за время O(nm), где n и m - размеры матрицы. Сначала в матрице, полученной применением градиентого фильтра к изображению, ищется наибольшая подматрица с значениями градиента, не превышающими установленное значение (max_grad в конфигурации). Затем накладывается условие про среднее значение в матрице: оно не должно быть меньше установленного (min_avg в конфигурации). Так мы исключаем из ответа обычную сплошную заливку.

Кратко опишем алгоритм нахождения наибольшей нулевой подматрицы за O(nm). Сначала посчитаем вспомогательную динамику: для каждой a[i][j] найдём ближайшую сверху единицу (где a - матрица), обозначим за d[i][j]. Тогда d[i][j] равно либо -1 (если такой строки нет), либо некоторому числу, которое равно наибольшему номеру строки, в которой в j-ом столбце стоит единица. Это легко посчитать, если двигаться сверху вниз. Далее перебираем номер i нижней строки нулевой подматрицы, затем перебираем столбец j, в котором мы будем упирать вверх нулевую подматрицу. Пользуясь уже подсчитанным значением d[i][j], сразу можно получить номер верхней строки нулевой подматрицы. Осталось определить оптимальные левую и правую границы нулевой подматрицы, — т.е. максимально раздвинуть эту подматрицу влево и вправо от j-го столбца. Это делается за О(1) в среднем с помощью стека.

Проанализируем время работы алгоритма. Применение графиентного фильтра, подсчёт интегральных сумм  и алгоритм поиска наибольшей подматрицы занимает O(nm). Расход по памяти также O(nm).

## Запуск

Программа реализована на языке Python (версия 3.7). Для успешной работы программы необходимо установить пакеты numpy и opencv-python с помощью следующих команд:

```
pip3 install numpy
pip3 install opencv-python
```

Сама программа запускается следующим образом:
```
python3 gradient_detector.py <name file with config>
```
Файл с конфигурацией должен быть написан в JSON-формате.

## Формат конфигурации

Файл с конфигурацией задает некоторые параметры программы. Он включает в себя следующие поля:
* **images_dir** - папка, в которой лежат изображения, на которых пользователь хочет найти участки с градиентной заливкой.
* **images** - список с именами картинок (должны лежать в папке images_dir).
* **dir_for_answer** - папка, в которую сохранятся картинки со взятыми в рамку градиентными областями.
* **grad_max** - максимальное допустимое значение градиента изображения, при котором заливка прямоугольника считалась градиентной.
* **min_avg** -  минимальное среднее значение градиента в прямоугольнике.
* **rect_colour** - цвет рамки, в которую заключают градиентную заливку.
* **rect_thickness** - толщина рамки, в которую заключают градиентную заливку.

## Пример работы

Склонируйте репозиторий, запустите программу командой
```
python3 gradient_detector.py conf.json
```

Результат работы:

|  1.jpg | 2.jpg | 3.jpg | 4.jpg |
|:----------------:|:----------------:|:----------------:|:----------------:|
| <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/images/1.jpg" alt="drawing1" width="300"/> | <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/images/2.jpg" alt="drawing2" width="300"/> | <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/images/3.jpg" alt="drawing3" width="300"/> | <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/images/4.jpg" alt="drawing4" width="300"/> |
|  <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/results/1.jpg" alt="drawing4" width="300"/>  | <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/results/2.jpg" alt="drawing5" width="300"/> | <img src="https://github.com/x-ENIAC/Finder-gradient-areas/blob/master/results/3.jpg" alt="drawing6" width="300"/> | Gradient areas could not be found in the picture 4.jpg. Try changing the setting in the config file. |
