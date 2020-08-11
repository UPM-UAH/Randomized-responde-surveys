# Randomized response surveys

Algunas ideas.

## Artículo de Blair et al. (2015):

Mirrored question design: con una probabilidad p (parámetro de diseño) respondes la pregunta, y con 1-p respondes la contraria. La probabilidad deseada P, en función de la probabilidad observada Q, puede expresarse como P = (Q+p-1)/(2·p-1), siendo p un parámetro de diseño.

Forced response design: con probabilidad p0 dices "no", con p1 dices "sí", con 1-p0-p1 dices la verdad. P = (Q-p1)/(1-p0-p1), siendo p0 y p1 parámetros de diseño. Parece que este diseño es el más usado.

Disguised response design (Kuk): la relación entre P y Q es como en mirrored question design. El procedimiento de encuesta es diferente, para que "psicológicamente" cueste menos responder. En función de la respuesta real "sí" o "no", el encuestado saca una carta de una baraja o de otra. En esas barajas, las proporciones de cartas rojas y negras son diferentes (eso es lo que permite obtener una estimación de P). El encuestado dice si la carta es roja o negra.

Unrelated question design: P = (Q-(1-p1)·p2)/p1, siendo p1 y p2 parámetros de diseño, que se suponen conocidos (probabilidad de que les toque la pregunta que nos interesa, y probabilidad de que la otra pregunta tenga respuesta positiva, respectivamente)

En todos los casos, P es una función afín de Q: P = a·Q+b, siendo a y b parámetros de diseño, con valores conocidos.


## Hussain et al. (2016):

Modificación de disguised response design (Kuk): en vez de una carta sacan varias (reintegrando cada una a la baraja). Así se obtiene más información (pero es más comprometedor para el encuestado).

Método de Singh y Grewal (2013): en función de si la respuesta real es "sí" o "no" eliges una baraja. Cada carta contiene "sí" o "no. Vas sacando cartas hasta que sale una con tu estado real (distribución geométrica condicionada a esa baraja). Cada baraja tiene una proporción de "sí" diferente. El encuestado dice el número de cartas que ha tenido que sacar, Z. La P deseada es una función afín de E[Z], con dos parámetros de diseño. Se conoce un estimador insesdado de P en función de Z.

Método que proponen: vas sacando cartas hasta que salen r con tu estado real (distribución binomial negativa, en vez de geométrica). Obtienen un estimador insesgado, calculan su varianza, comparan con la de los dos métodos anteriores


## Uso de IBS. Posibilidad de contribuir

IBS: permite estimar una probabilidad garantizando una calidad *relativa* (RMSE, confianza, MAE). El problema es que IBS, aplicado directamente, se refiere a Q, no a P. Garantizar un calidad relativa para Q no nos sirve; habría que garantizarla para P.

No parece que haya artículos o resultados conocidos para garantizar una calidad relativa para P a partir de observaciones de Q.


## IBS modificado con número de entradas cuadrático. NO FUNCIONA; HABÍA UN FALLO, YA IDENTIFICADO

Una forma de garantizar una calidad relativa para P = a+b·Q a partir de observaciones de una secuencia X de variables i.i.d. de Bernoulli con parámetro Q es la siguiente.

El caso más interesante es Q = (1+P)/2; P = 2·Q-1. Esto corresponde por ejemplo a forced response design con p0=0, p1 = 1/2. Además puede suponerse que P es muy pequeña, ya que es la situación de mayor interés. Entonces Q será próxima a 1/2. Suponemos también que la calidad relativa se define en términos de confianza: probabilidad de que el valor estimado esté dentro de un intervalo *relativo* en torno al valor real.

Aplicamos a la secuencia X la transformación (fábrica de Benoulli) y = (x-1/2)^2. Como la secuencia X tiene parámetro Q=(1+P)/2, la secuencia transformada Y tendrá parámetro P^2/4. Producir cada muestra de Y consume del orden de 1 muestra de X en media, para P pequeña, como se explica a continuación.

Esa transformación se hace de la siguiente manera, usando variables aleatorias auxiliares con distribuciones fijas:

 - (Q-1/2)^2 se puede expresar como (1-4·Q·(1-Q))/4 = (1-4·sqrt(Q)^2·sqrt(1-Q)^2)/4.
 - El primer factor, 1/4, se genera sin consumir muestras, simplemente usando una variable auxiliar de Bernoulli con parámetro 1/4. Si esa variable es 0 hemos terminado, y el resultado es 0. Si esa variable es 1 continuamos.
 - La operación 1-(...) es simplemente invertir el valor (intercambiar cero y uno).
 - El término 4·sqrt(Q)^2·sqrt(1-Q)^2 es 2/3 · 6·sqrt(Q)^2·sqrt(1-Q)^2, y 6·sqrt(Q)^2·sqrt(1-Q)^2 es la probabilidad de que en 4 realizaciones de sqrt(Q) haya 2 ceros y 2 unos (hay 6 posibles casos: 0011, 0101, 0110, 1001, 1010, 1100). **ESTO ESTÁ MAL: es 1-sqrt(Q), no sqrt(1-Q). ESTE ERA EL FALLO**
 - Generar 2/3 es, de nuevo, inmediato
 - Generar 6·sqrt(Q)^2·sqrt(1-Q)^2 requiere 4 muestras (como mucho) de sqrt(Q).
 - sqrt(Q) se genera a partir de muestras de X, con un consumo medio de 1/sqrt(Q) muestras de entrada para cada salida (Mendo, 2019).

En total, el número de muestras de X requeridas para obtener una muestra de Y es 1/4 · 2/3 · 4/sqrt(Q) = 2/3/sqrt(Q), que es aproximadamente 1 para Q próxima a 1/2. Es decir, para producir cada muestra de Y se consume del orden de 1 muestra de X.

Después aplicamos IBS a la secuencia Y, que tiene parámetro P^2/4. Ello permite estimar P^2/4 con confianza garantizada, consumiendo del orden de 1/P^2 muestras de Y, o de X, en media. La estimación de P^2/4 con confianza garantizada puede traducirse en una estimación de P con confianza garantizada (transformando el intervalo relativo). 

Parece excesivo. ¿Se puede hacer consumiendo del orden de 1/P muestras, en vez e 1/P^2? (Haciendo la pregunta directa, sin RRS, sí sería del orden de 1/P).


### Propiedades de |x-1/2|. NO ES UNA FUNCIÓN SIMULABLE; HABÍA UN FALLO EN LO ANTERIOR

Consideremos la transformación y = (x-1/2)^2, seguida de z = y^(1/2). La transformación compuesta es z = f(x) = |x-1/2|. Esta función es C0 pero no C1. Por tanto, según Nacu-Peres (tabla 1, teorema 2, proposición 22 con k=1):

1. z = f(x) no puede simularse de forma "fast", es decir, con cola exponencial (teorema 2).

   No hay contradicción con la proposición 14 (ii), que dice que la composición de funciones "fast" es "fast". z es la composición de y = (x-1/2)^2, z = y^(1/2). La primera se puede simular de forma "fast", pero la segunda no, porque para x ≈ 1/2 se tiene y ≈ 0, y ahí z = y^(1/2) no es analítica, por tanto no es "fast".

2. Sea N el número de observaciones necesarias. Por construcción, f puede simularse con E[N] finito. Por tanto (proposición 22 con k=1) el momento de primer orden no puede tener cola uniforme. Es decir, E[N * 1(N>n)] tiende a 0 cuando n tiende a infinito, pero no uniformemente en p (en un conjunto abierto).

Por otro lado, según Keane-O'Brian (1994) (véase también Nacu-Peres, pág. 1), no es posible simular |x-1/2|, o (x-1/2)^2, porque esas funciones no cumplen la condición de cota polinómica (teorema de Keane-O'Brian, o bien (1) de Nacu-Peres). Así que debe haber un fallo en lo anterior: no es posible simular y = (x-1/2)^2: ?

Ya está identificado el fallo: el término que aparecía como sqrt(1-Q) es realmente 1-sqrt(Q), y ahí se estropea todo


## Lista de algoritmos de fábricas de Bernoulli

Parece bastante exhaustiva: https://github.com/peteroupc/peteroupc.github.io/blob/master/bernoulli.md

(También en https://peteroupc.github.io/bernoulli.html)


## Dado que |x-1/2| y (x-1/2)^2 no son simulables: ¿enfoque contrario?

Tal vez pueda probarse que hay alguna limitación en la precisión con que puede estimarse |p-1/2| a partir de observaciones de una secuencia con parámetro p. El argumento sería: consideremos un estimador de |p-1/2| a partir de observaciones de una secuencia con parámetro p. Ese estimador quizá pueda considerarse una fábrica de Bernoulli que transforma p en |p-1/2|, la cual sabemos que es imposible. Sería análogo, pero al revés, a Mendo (2019), donde se considera la fábrica como un estimador y eso impone limitaciones sobre la fábrica.

Obtener un estimador de f(p) a partir de una fábrica de f(p) es inmediato: la salida de la fábrica es un estimador (secuencial, insesgado) de f(p). Obtener una fábrica de f(p) a partir de un estimador de f(p) (en ambos casos usando una secuencia X de parámetro p como entrada) no está tan claro. Una posibilidad es, si el estimador es consistente (converge en probabilidad al valor correcto f(p) cuando el número de observaciones de X tiende a infinito), obtener una estimación muy buena de |p-1/2|, digamos r, y sintetizar sin más uan variable de Bernoulli con parámetro r.

Más fácil: estimamos f(p). Supongamos que el estimador es una VA R continua (tiene fdp), entre 0 y 1, y que es insesgado (E[R] = f(p)). [Si esto funciona, después puede prescindirse de la suposición de que R tenga densidad, usando el formalismo de teoría de la medida e integrales de Lebesgue]. Conocido el valor que toma R, generamos una VA B de Bernoulli con parámetro R. B condicionada a R es una VA de Bernoulli de parámetro R. Es fácil ver [comprobar] que B, no condicionada, es Bernoulli con parámetro E[R], que es igual a f(p) por ser el estimador insesgado. Ya tenemos la fábrica a partir de un estimador insesgado.
