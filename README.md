# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Пермяков Егор Владимирович

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
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

## Задание 2

Код конфигурации для обучения сети.

>behaviors:
> RollerBall:
>    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4   
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99 
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
    
Разберем каждую строку отдельно.

#hyperparameters

trainer_type: ppo

Тип нейросети расшифровывающийся как Proximal Policy Optimization.

batch_size: 10

Количество экспериментов перед итерацией обновления градиентного спуска.

buffer_size: 100

Количество экспериментов, то есть количество наблюдений, действий и полученных вознаграждений, которых необходимо провести перед тем как обновить модель. Кратно batch_size.

learning_rate: 3.0e-4 

Сила каждого шага при обновлении при градиентном спуске.

beta: 5.0e-4

Регулирует меру хаоса - энтропийную регулирязации, что делает поведение агента более разнообразным и обеспечивает полное ислледование пространства действий.

epsilon: 0.2

Устанавливает допустимый порог расхождения между старым и новым значением при обновлении при прохождении градиентного спуска.

lambd: 0.99

Соотвествует параметру, использоемому при расчете generalized advantage estimate (GAE). Это можно рассматривать как то, насколько агент полагается на свою текущую оценку стоимости при вычислении обновленной оценки стоимости.

num_epoch: 3

Количество проходов через buffer size во время градиентного спуска.

learning_rate_schedule: linear

Описывает что learning_rate будет изменяться линейно.


#network_settings

normalize: false

Описывает применять ли нормализацию, основанную на скользящем среднем и дисперсии, на входящие данные.

hidden_units: 128

Количество нейронов в каждом слое нейронной сети.

num_layers: 2

Количество слоев в нейронной сети.


#reward_signals
gamma: 0.99 

Коэффицент обеценивания будущих вознаграждений. Параметр влияет на то на какие награды будет ориентироваться модель - на краткосрочные или долгосрочные награды.

strength: 1.0

Величина награды.

max_steps: 500000

Количество шагов симуляции.

time_horizon: 64

Параметр соответствует тому, сколько шагов опыта необходимо собрать для каждого агента, прежде чем добавить его в буфер опыта.

summary_freq: 10000

Количество опытов, которое необходимо собрать перед созданием и отображением статистики обучения.


## Задание 3

## Выводы

Таким образом мы создали и обучили модель в связке Python (pytorch) - ML Agent - Unity, который самообучился подкатываться к кубу. А также разобрали какие гиперпараметры влияют на обучение сети и их значение.

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
