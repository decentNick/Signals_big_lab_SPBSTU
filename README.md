# Signals_big_lab_SPBSTU

Этот проект сделан в учебных целях. Проект решает задачу семантической сегментации ногтевой пластины.


## Постановка задачи ##
На вход подается фотография с изображением кисти на расстоянии от камеры не более вытянутой руки.
Требуется найти маску сегментации с двумя метками классов (ногтевая пластина и фон).


## Испробованные варианты решения и причины их не использования ##
1) Локализация ногтя путем сравнения с шаблонами
    Отсутствует стандарт ногтевой пластины. Ногти часто имеют разный маникюр, форму и поворот к камере, таким образом значительно разрастается количество шаблонов. Фактически для каждого изображения нужен свой шаблон. Медленная скорость работы алгоритма.
2) Локализация ногтей по особым точкам.  
    Много шумовых точек чаще всего на границах пальцев. 
3) Выделение границ с помощью метода Canny
    Часто ногтевые пластины сливаются с границами пальцев, очень сильно различается оптимальный выбор параметров для различных изображений.
4) Уточнение границ с помощью сегментации активного контура
    Точность сегментации получается хорошей, только если на границе хороший контраст и во внутренней областе изначальной кривой находится один ноготь.


Мной рассматривались два варианта решения: обучить нейронную сеть для задачи сегментации или локализовать методами машинного обучения ногтевые пластины и сегментировать их с помощью активного контура.

### Выбор нейронной сети для сегментации ###
Мною был выбран U-Net. Так как она считается стандартной архитектурой для решения задач сегментации. Его архитектура состоит из стягивающего пути для захвата контекста и симметричного расширяющегося пути, который позволяет осуществить точную локализацию. Сеть обучается сквозным способом на небольшом количестве изображений и превосходит предыдущий наилучший метод на соревновании ISBI по сегментации нейронных структур в электронно-микроскопических стеках. Используя ту же сеть, которая была обучена на изображениях световой микроскопии пропускания, U-Net заняла первое место в конкурсе ISBI 2015 года по трекингу клеток в этих категориях с большим отрывом. Кроме того, эта сеть работает быстро. Сегментация изображения 512×512 занимает менее секунды на современном графическом процессоре.
![U-Net arch](stuff/U-net-neural-network.png)

Для U-Net хатактерно:
1) достижение высоких результатов в различных реальных задачах
2) использование небольшого количества данных для достижения хороших результатов.

Второй пункт особенно важен, ведь весь датасет состоит из 1533 изображений.

## План решения этой задачи ##
Подготовить входные данные. Обучить нейронную сеть U-Net (https://arxiv.org/pdf/1505.04597.pdf) на оставшихся данных(1483 размеченных изображения). Улучшить результат методами постобработки.


## Результат ##
Средняя метрика IoU (https://www.pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/) для тестовых изображений равна 0.8216338116912081.  Считаю, на данный момент очень хороший результат. Хотя по прежнему с некоторыми изображениями возникают трудности, тк минимальное значение метрики IoU равно 0.2126672857078948.

