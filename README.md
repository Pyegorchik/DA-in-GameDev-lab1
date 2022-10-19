# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Пермяков Егор Владимирович

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Постановка задачи.
В данной лабораторной работе мы создадим ML-агент и будем тренировать нейросеть, задача которой будет заключаться в управлении шаром. Задача шара заключается в том, чтобы оставаясь на плоскости находить кубик, смещающийся в заданном случайном диапазоне координат.

## Задание 1
Реализовать систему машинного обучения в связке Python - ML-агент  – Unity.
Для начала созданим 3D проект в Unity и добавим ML - Agent через Package Manager и json файлы предоставленные к лабораторной.
Получим следующий результат.
![image](https://user-images.githubusercontent.com/114165213/196630099-665b51df-a5eb-408a-8fcb-12ee47abc454.png)

Затем создадим виртуальную среду для обучения модели в Anaconda Promt через серию последующих команд:

conda create -n MlAgent python=3.6.13

conda activate MlAgent

Установим также pytorch для возможности работы с ML Agent.

pip install torch~1.7.1 -f https://download.pytorch.org/whl/torch_stable.html

Чтобы не забыть в будущем, с помощью команды ниже перейдем в папку с нашим Unity проектом.

cd C:\Users\Yegor\Lab3

В нашем Unity проекте создадим сферу, плоскость и шар, и дадим им текстуры.
![image](https://user-images.githubusercontent.com/114165213/196631358-1305444e-e908-48ed-b7dc-8022828ab682.png)


Создаем C# скрипт и копируем код из RollerAgent.cs - один из приложенных к лаборатной работе файлов.

Для сферы добавляем компоненты RigidBody, Decesion Requester, Behaviour Parametrs и наш созданный скрипт в моем случае Roller Agent.
![image](https://user-images.githubusercontent.com/114165213/196632622-8efa0580-6362-48d6-ab78-51affdbed878.png)

Затем в корень проекта добавляем файл rollerball_config.yaml из приложенных материалов.

Запускаем работу ML Agent следующей командой.
mlagents-learn rollerball_config.yaml --run-id=RollerBall --force

Модель начала обучаться.
![image](https://user-images.githubusercontent.com/114165213/196635333-e5f82db7-fd43-42d3-afa7-f34707c46acd.png)

После создания 9 площадок для обучения и прохождения 500000 шагов модель обучена и записана в файл RollerBall.onnx.

Таким образом мы создали и обучили модель в связке Python (pytorch) - ML Agent - Unity, который самообучился подкатываться к кубу.

## Задание 2


## Задание 3

## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
