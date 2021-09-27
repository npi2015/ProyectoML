# Conecta 4 con Reinforcement Learning

El objetivo de este proyecto es crear un modelo de ML que aprende a jugar solo al conecta 4. Se realiza un análisis de varios métodos para lograr este objetivo, empezando por modelos simples que no requieren de machine learning y acabanado por la implementación de distintos algoritmos de RL (en este caso PP01, A2C y ACER). 

## Dependencias:


Python == 3.7

tensorflow-gpu == 1.15.0

matplotlib == 3.4.3

pandas == 1.3.3

stable-baselines == 2.9.0

numpy == 1.21.2

kaggle-environments == 1.8.12

requests == 2.26.0

gym == 0.20.0

### Modelos simples

Se empieza con algunos modelos simples que realizan movimientos predeterminados, como por ejemplo poner siempre fichas en la fila de la derecha. Como uno se puede imaginar, la efectividad de estos modelos es baja, ya que son muy fáciles de contrarrestar. 

### Modelo heurístico

Un modelo heurístico asigna puntuaciones a distintos tableros dependiendo de como estén las fichas tras un movimiento. Una mayor puntuación implica un movimiento más favorable. En un primer instante, se realiza un modelo simple que solo mira como estará el tablero en un movimiento. Aunque vence a los modelos simples en casi todos los casos,hay ciertos casos en los que realiza decisiones que son detrimentales en el futuro de la partida. 

Este problemas se soluciona utilizando modelos heurísticos de mayor profundidad, que hacen uso en este caso del algoritmo minimax para encontrar el mejor movimiento en cada caso. Este algoritmo consiste en encontrar la mejor puntuación asumiendo que el oponente siempre hará también el mejor movimiento. Aunque aumenta mucho el tiempo de computación, ya que tiene que calcualr $7^n$ tableros (n es la profundidad), aumenta mucho la efectividad del modelo. 

Se puede disminuir el tiempo de computación haciendo uso de poda alfa beta, una implementación del algoritmo minimax que ignora todas las ramas que sabe que no pueden devolver un resultado más favorable que uno ya conocido (https://es.wikipedia.org/wiki/Poda_alfa-beta). 

### Modelos de Deep Learning

Para estos modelos se utiliza la librería de stable_baselines, que contiene varios algoritmos de Reinforcement Learning que se pueden aplicar. En este caso se aplicarán los tres siguientes: 

* PPO1 (Proximal Policy Optimization): https://stable-baselines.readthedocs.io/en/master/modules/ppo1.html
* A2C: https://stable-baselines.readthedocs.io/en/master/modules/a2c.html
* ACER: https://stable-baselines.readthedocs.io/en/master/modules/acer.html

El objetivo de este método es definir una red neuronal que actué como función heurística. Esto se lleva al cabo utilizando una red convolucional de dos capas. Luego, cada modelo va ajustando los pesos de cada neurona para optimizar la función heurística. En este proyecto la implementación de la red neuronal es bastante simple, ya que no intenta predecir los movimientos futuros, si no que más bien funciona como la primera heúristica más optimizada. Esto implica que la función heurística gana la mayoría de las partidas, ya que tiene una profunidad de 5. Lo más probable es que una combinación de ambos métodos sea lo más efectivo. Esta combinación es la que utilizan programas como Stockfish para jugar al ajedrez.

#### Porcentajes de victorias (en 50 partidas)
PPO1 contra modelo heurístico: 
* PP01: 28%
* Heurístico: 72% 

A2C contra modelo heurístico: 
* A2C: 6%
* Heurístico: 94%  

ACER contra modelo heurístico: 
* ACER: 16%
* Heurístico: 84%

