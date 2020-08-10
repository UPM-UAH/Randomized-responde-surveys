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


## IBS modificado con número de entradas cuadrático

Una forma de garantizar una calidad relativa para P = a+b·Q a partir de observaciones de una secuencia X de variables i.i.d. de Bernoulli con parámetro Q es la siguiente.

El caso más interesante es Q = (1+P)/2; P = 2·Q-1. Esto corresponde por ejemplo a forced response design con p0=0, p1 = 1/2. Además puede suponerse que P es muy pequeña, ya que es la situación de mayor interés. Entonces Q será próxima a 1/2. Suponemos también que la calidad relativa se define en términos de confianza: probabilidad de que el valor estimado esté dentro de un intervalo *relativo* en torno al valor real.

Aplicamos a la secuencia X la transformación (fábrica de Benoulli) y = (x-1/2)^2. Como la secuencia X tiene parámetro Q=(1+P)/2, la secuencia transformada Y tendrá parámetro P^2/4. Producir cada muestra de Y consume del orden de 1 muestra de X en media, para P pequeña, como se explica a continuación.

Esa transformación se hace de la siguiente manera, usando variables aleatorias auxiliares con distribuciones fijas:

 - (Q-1/2)^2 se puede expresar como (1-4·Q·(1-Q))/4 = (1-4·sqrt(Q)^2·sqrt(1-Q)^2)/4.
 - El primer factor, 1/4, se genera sin consumir muestras, simplemente usando una variable auxiliar de Bernoulli con parámetro 1/4. Si esa variable es 0 hemos terminado, y el resultado es 0. Si esa variable es 1 continuamos.
 - La operación 1-(...) es simplemente invertir el valor (intercambiar cero y uno).
 - El término 4·sqrt(Q)^2·sqrt(1-Q)^2 es 2/3 · 6·sqrt(Q)^2·sqrt(1-Q)^2, y 6·sqrt(Q)^2·sqrt(1-Q)^2 es la proabilidad de que en 4 realizaciones de sqrt(Q) haya 2 ceros y 2 unos (hay 6 posibles casos: 0011, 0101, 0110, 1001, 1010, 1100).
 - Generar 2/3 es, de nuevo, inmediato
 - Generar 6·sqrt(Q)^2·sqrt(1-Q)^2 requiere 4 muestras (como mucho) de sqrt(Q).
 - sqrt(Q) se genera a partir de muestras de X, con un consumo medio de 1/sqrt(Q) muestras de entrada para cada salida (Mendo, 2019).

En total, el número de muestras de X requeridas para obtener una muestra de Y es 1/4 · 2/3 · 4/sqrt(Q) = 2/3/sqrt(Q), que es aproximadamente 1 para Q próxima a 1/2. Es decir, para producir cada muestra de Y se consume del orden de 1 muestra de X.

Después aplicamos IBS a la secuencia Y, que tiene parámetro P^2/4. Ello permite estimar P^2/4 con confianza garantizada, consumiendo del orden de 1/P^2 muestras de Y, o de X, en media. La estimación de P^2/4 con confianza garantizada puede traducirse en una estimación de P con confianza garantizada (transformando el intervalo relativo). 

Parece excesivo. ¿Se puede hacer consumiendo del orden de 1/P muestras, en vez e 1/P^2? (Haciendo la pregunta directa, sin RRS, sí sería del orden de 1/P).


## Propiedades de |x-1/2|

Consideremos la transformación y = (x-1/2)^2, seguida de z = y^(1/2). La transformación compuesta es z = f(x) = |x-1/2|. Esta función es C0 pero no C1. Por tanto, según Nacu-Peres (tabla 1, teorema 2, proposición 22 con k=1):

1. z = f(x) no puede simularse de forma "fast", es decir, con cola exponencial (teorema 2). No hay contradicción con la proposición 14 (ii), que dice que la composición de funciones "fast" es "fast". z es la composición de y = (x-1/2)^2, z = y^(1/2). La primera se puede simular de forma "fast", pero la segunda no, porque para x ≈ 1/2 se tiene y ≈ 0, y ahí z = y^(1/2) no es analítica, por tanto no es "fast".
2. Sea N el número de observaciones necesarias. Por construcción, f puede simularse con E[N] finito. Por tanto (proposición 22 con k=1) el momento de primer orden no puede tener cola uniforme. Es decir, E[N * 1(N>n)] tiende a 0 cuando n tiende a infinito, pero no uniformemente en p (en un conjunto abierto).

