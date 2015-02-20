# Модуль управления для браслета **_Myo_**
-----------------------------

Браслет **_Myo_** может получать данные напряжении мышц кисти и следовательно о жестах показываемых рукой, с последующей передачей данных на ПК.

[Подробнее о Myo](https://www.thalmic.com/en/myo/)

Данный модуль позволяет реализовать управление роботом с помощью жестов в среде RCML, посредством браслета Myo и ПК. Оси модуля частично соответствуют жестам в Myo, часть жестов была объединена в одну ось для адаптации применительно к роботу. Дополнительно модуль имеет бинарную ось, так называемой "блокировки", которая зависит от того блокирован или не блокирован браслет и которая может быть использована для блокировки робота с целью предотвращения неожидаемых движений.

Myo умеет распознавать следующие жесты по умолчанию:
- fist;
- fingers spread;
- double tap;
- wave out;
- wave in.

Узнать подробнее о жестах и о том как их правильно выполнять, можно в программе Myo Conenct при прохождении начального обучения пользования браслетом.

В среде RCML модуль доступен под именем **_Myo_**.
Модуль не нуждается в какой-либо настройке и подключается по умолчанию к первому браслету в системе.

-----------------------------

Для компиляции модуля потребуется [Myo SDK](https://developer.thalmic.com/downloads).

Для работы модуля потребуется браслет Myo, предварительно установленный в программе [Myo Conenct](https://www.thalmic.com/start/) и откалиброванный под Вашу руку.

*Перед использованием рекомендуется поставить последние версии прошивки браслета и программы Myo Conenct*

-----------------------------

####Доступные оси в модуле
Наименование  | Верхняя граница  | Нижняя граница  | Примечание
------------  | -----------------  | ------------------  | ------------------
locked  | 1  | 0  | Бинарная ось. Отражает статус браслета Myo: блокирован - 1, разблокирован - 0. 
fist  | 1  | 0  | Бинарная ось. Если наблюдается жест **_fist_** - 1, в противном случае - 0.
left_or_right  | 1  | 0  | Дискретная ось. Удобна для задания направления перемещения или поворота. Основа на жестах **_wave out_** и **_wave in_**. Автоматически определяет направление кисти в данных жестах направо или налево (независимо от того на какую руку надет браслет) и принимает следующие значения: -1 - налево, 0 - нет жеста, 1 - направо.
fingers_spread  | 1  | 0  | Бинарная ось. Если наблюдается жест **_fingers spread_** - 1, в противном случае - 0.
double_tap  | 1  | 0  | Бинарная ось. Если наблюдается жест **_double tap_** - 1, в противном случае - 0.
arm_pitch_angle  | 18  | 0  | Непрерывная ось. Отмечает угол между предплечьем с согнутым локтем и воображаемой вертикальной линией направленной в землю (ось Z в декартовой системе координат). Для изменения значения этой оси должен наблюдаться жест **_fist_**. Если данный жест перестает наблюдаться во время изменения значения данной оси, то значение запоминается и будет передаваться каждый раз. Значение оси соответствует десяткам градусов угла. Подробнее ниже.

####Примечания
- **Значения всех осей (кроме arm_pitch_angle) передаются 20 раз в секунду**.
- Значение оси **_arm_pitch_angle_**, передается только при изменении, если браслет не блокирован.
- Если браслет блокирован, то значения всех осей кроме оси **_locked_**, будут равны 0.
- Браслет блокируется автоматически, если некоторое малое время (1-2 сек) по датчикам наблюдается не известный браслету жест, а также если обнаружен сдвиг браслета на руке или снятие браслета с руки.
- Разблокировка браслета происходит жестом **_double tap_**, как и во многих Myo-совместимых приложениях.
- Ось **_arm_pitch_angle_** наиболее просто представить через сигнал-жест "стоп" принятый в спецназе - зажатый кулак поднятый к вверху (в небо) с согнутым локтем в плече, данная поза соответствует верхнему значению - 18. Опускание кулака вперед и вниз по дуге, не меняя сгиба в плече будет уменьшать значение данной оси. Значение 0 будет соответствовать направленному в землю кулаку.
- **Не рекомендуется** использовать для управления роботом одновременно оси **_fist_** и **_arm_pitch_angle_**, т.к. ось **_fist_** принимает значение 1 при попытке изменения значения оси **_arm_pitch_angle_**, поскольку для этого требуется появления жеста **_fist_**.

####Компиляция
Для компиляции модуля потребуется библиотека [SimpleIni](https://github.com/brofield/simpleini)