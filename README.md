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

Obtener un estimador de f(p) a partir de una fábrica de f(p) es inmediato: la salida de la fábrica es un estimador (secuencial, insesgado) de f(p). Obtener una fábrica de f(p) a partir de un estimador de f(p) (en ambos casos usando una secuencia X de parámetro p como entrada) no está tan claro. Una posibilidad es, si el estimador es consistente (converge en probabilidad al valor correcto f(p) cuando el número de observaciones de X tiende a infinito), obtener una estimación muy buena de |p-1/2|, digamos r, y sintetizar sin más una variable de Bernoulli con parámetro r.

Más fácil: supongamos que tenemos un estimador de f(p) que es una VA R continua (tiene fdp), entre 0 y 1, y que es insesgado (E[R] = f(p)). [Si esto funciona, después puede prescindirse de la suposición de que R tenga densidad, usando el formalismo de teoría de la medida e integrales de Lebesgue]. Conocido el valor que toma R, generamos una VA B de Bernoulli con parámetro R. B condicionada a R es una VA de Bernoulli de parámetro R. Es fácil ver [comprobar] que B, no condicionada, es Bernoulli con parámetro E[R], que es igual a f(p) por ser el estimador insesgado. Ya tenemos la fábrica a partir de un estimador insesgado.

Otra forma de obtener una fábrica de Bernoulli a partir de un estimador R insesgado entre 0 y 1 es comparar con una VA independiente U uniforme en (0,1). Si R>U el resultado es 1, si no es 0. La media e esa VA es f(p) [Se plantea la integral doble respecto a r y u; luego se reduce a una integral simple respecto a u de la FD de R; luego se integra por partes]

Esto es parecido a Latuszynski (2010; "Simulating Events of Unknown Probabilties via Reverse Time Martingales"), lemma 2.1, aunque allí hablan de equivalencia entre eventos y estimadores, no entre fábricas y estimadores.


# Equivalencia entre fábricas de Bernoulli y estimadores insesgados

Ya que hay una equivalencia entre fábricas de Bernoulli y estimadores insesgados entre 0 y 1, quizá se puedan

 - Utilizar las limitaciones de estimadores insesgados de f(p) para deducir limitaciones sobre fábricas de Bernoulli. Se me ocurren: cota de Wolfowitz (1947) (https://projecteuclid.org/euclid.aoms/1177730439) (Cramér-Rao secuencial); resultados de  DeGroot (1959) (https://projecteuclid.org/euclid.aoms/1177706361) (los resultados de Girshick et al. (1946) son para p, no para f(p): (https://projecteuclid.org/euclid.aoms/1177731018). Este enfoque es similar al de Mendo (2019) (https://arxiv.org/pdf/1612.08923.pdf), donde se considera la fábrica de Bernoulli como un estimador.
 - Utilizar las limitaciones conocidas de una fábrica de Bernoulli para deducir limitaciones del estimador insesgado equivalente. Creo que Nacu-Peres tiene un buen resumen


## DeGroot / Wästlund

De acuerdo con DeGroot, pág. 87:: 

 1. Con tamaño fijo n se pueden estimar de forma insesgada polinomios de grado hasta n
 2. Con muestreo binomial inverso: esperar a tener c unos, para c>=2, se puede estimar de forma insesgada cualquier función h(q) (q=1-p) con desarrollo de Taylor en torno a q=0 en |q|<1: h(q) = p^c \sum_{k=0}^\infty b_k q^k. El estimador es: (4.5): $\hat h = b_k / {k+c-1 \choose c-1}$, con k = n-c, siendo n el número de observaciones que han sido necesarias: k es el número de ceros que han salido.

Respecto a 2: Esto se puede convertir en fábrica si el estimador \hat h está entre 0 y 1, es decir, si los b_k están entre 0 y {k+c-1 \choose c-1}. Para c=2 (a menor c mayor varianza del estimador, pero no nos importa, salvo que el estimador pueda salirse de [0,1]): \hat h = b_k/(k+1): la condición es que b_k esté entre 0 y k+1. El número medio de observaciones necesarias es c/p = 2/p.
 En Mendo (2019) se consideran funciones f(p) = 1-\sum_{k=1}^\infty c_k q^k, o equivalentemente (vía 1-(·)) \sum_{k=1}^\infty c_k q^k, c_k >=0, sum_k c_k=1 (esto último equivale a que la función tiende a 1 cuando p tiende a 0). Se pueden simular con f(p)/p observaciones en media.
 Es decir, Mendo (2019) parece ¿más general? y usa menos muestras en media. Por tanto esta vía no parece interesante

Respecto a 1: Con tamaño fijo n, según DeGroot: se puede estimar de forma insesgada cualquier polinomio de grado <=n, porque se puede estimar p^k, k=1, ..., n. Supongo que habría que hacer a mano la combinación lineal que define el polinomio en función de los términos p^k. Con eso obtienes un estimador del polinomio, y de ahí se puede obtener una fábrica si el estimador está entre 0 y 1. El problema es que, aunque los estimadores de p^k estén en [0,1], su combinación lineal (estimador del polinomio) en general no. Por tanto no vale.

Wästlund (1999) (http://www.math.chalmers.se/~wastlund/coinFlip.pdf), apartado 4: un polinomio es simulable (fábrica de Bernoulli) de forma finita si y sólo si es un coin-flipping polynomial. El número de observaciones necesarias puede ser mayor que el grado del polinomio, y se llama coin-flipping degree. Ej.: 3x(1-x) requiere 3 observaciones, no 2. Equivalentemente (teorema 4.5), el polinomio debe tener coeficientes enteros y debe ser 0, ó 1, o bien la imagen del intervalo (0,1) debe estar en (0,1).

 Un problema abierto (que parece difícil) es calcular o acotar el coin-flipping degree de un coin-flipping polynomial en función de su grado
 
 
 ## Cota de Wolfowitz (Cramér-Rao sequencial)
 
 Para poder aplicarla hay que comprobar que se cumplen unas condiciones de regularidad (Wolfowitz, 1947), un poco pesadas. Para funciones diferenciables y fábricas rápidas en el sentido de Nacu-Peres se cumplen esas condiciones, y se tiene la cota del teorema 3 de Mendo (2019). No sé si se puede avanzar mucho más. Algunas vías serían:
 
 - Intentar relajar las condiciones suficientes: función diferenciable con fábrica rápida (realmente, si hay una fábrica rápida la función debe ser analítica; Nacu-Peres).
 - Un resultado intermedio que se utiliza para demostrar eso es: fábrica rápida implica que E[N] es finita y es una función continua de p en (0,1) (Mendo, 2019, proposición 2). A lo mejor E[N] es función continua de p en un caso más general. Quizá puede probarse con la función característica de N (la función característica siempre existe); mejor en términos de transformada z: serie de potencias en el campo complejo. Habría que ver si es derivable en z=1, para ver si E[N] es finito.
 - Creo que para fábrica rápida se puede demostrar que E[N^k], no sólo E[N], es finito y función continua de p, usando el mismo método que en Mendo (2019) con una pequeña modificación. Este resultado se cumple no sólo para fábricas rápidas, sino para cualquier método secuencial de lo que sea (estimador, test de hipótesis) siempre que sea rápido, en el sentido de cola exponencial.
 - Seguramente para fabrica rápida se tiene también que E[N^k] es una función C^\infty de p. Por ejemplo, para ver que es C^1: si cada sumando Pr[N=n] de la serie original se deriva respecto a p sigue siendo un polinomio en p, y esta nueva serie también converge uniformemente. Como la serie original converge y la serie de derivadas converge uniformemente, esta última converge a la derivada de la función, que por tanto es continua (es el límite uniforme de las derivadas de los sumandos, y esas derivadas son funciones continuas). Iterando (inducción) se tiene que E[N^k] es C^m para todo m.
