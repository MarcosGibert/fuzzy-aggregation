# fuzzy-aggregation
Este es el repositorio asociado a mi Trabajo de Fin de Grado (TFG) sobre la agregación de números borrosos discretos.
El archivo `fuzzy_aggregation.py` contiene la clase DiscreteFuzzyNumber las funciones necesarias para realizar la agregación de dos números borrosos discretos mediante dos métodos principales.

El primero se basa en el artículo de *Aggregation of subjective evaluations based on
discrete fuzzy numbers* de Riera y Torrens. El método agrega dos números borrosos *A* y *B* de la siguiente manera:

1. Dada una función de agregagación en  $f$  en $L_n$ y un $\alpha$, la función `extended_aggreggation_alpha`  calcula el conjunto $f(A^{\alpha},B^{\alpha})$, donde $A^{\alpha}$ y $B^{\alpha}$ son respectivamente los $\alpha$-cuts de $A$ y $B$. 
2. la función `extended_aggreggation_cuts`  crea una lista con los valores de pertencia que aparecen en alguno de los dos números borrosos y posteriormente calcula el conjunto $f(A^{\alpha},B^{\alpha})$ mediante la función `extended_aggreggation_alpha` para cada valor de pertenencia .El resultado de la función es una lista ordenada de cada uno de estos conjuntos, que son los $\alpha$-cuts del número borroso resultante de la agregación .
3. Finalmente la función  `extended_aggreggation` construye el nuevo número borroso partiendo de la lista anterior. Empieza por el nivel $\alpha_{max}=1$ y asigna este valor de pertencia a los elementos de $f(A^{\alpha_{max}},B^{\alpha_{max}})$. A continuación pasa al siguiente nivel en orden descendente  $\alpha_{max-1}$ y evalúa el conjunto $f(A^{\alpha_{max-1}},B^{\alpha_{max-1}})$ Si hay elementos nuevos en este conjunto con respecto al conjunto del nivel anterior, asigna el valor de pertenencia $\alpha_{max-1}$ a los nuevos elementos. En caso contrario pasa al siguiente nivel y repite el proceso hasta recorrer toda la lista.


El segundo método es el que proponemos en el trabajo. Consta de las siguientes funciones:
1. `to_interval_list`: Dado un número devuelve una lista de sus $\alpha$-cuts para cada valor de pertenencia $\alpha ordenada de mayor a menor. Se le puede introducir un segundo número borroso para que considere cada valor de pertencia de ambos números borrosos.
2. `lex1_order`, `lex2_order`, `xy_order`, `inc_order`: Son distintos órdenes intervalares
3. `compare`: Dados dos números borrosos y un orden intervalar, usa la función `to_interval_list` para obtener la representación de ambos números borrosos a partir de sus $\alpha$ - cuts y compara los dos primeros intervalos de cada número borroso. Si son distintos el orden queda determinado por la relación de estos intervalos, (la función devuelve 1 si el primer número borroso es menor o igual que el segundo y 0 en caso cotrario). Si son iguales pasa a los dos segundos intervalos y así sucesivamente hasta encontrar dos intervalos diferentes. En caso de que todos los intervalos sean iguales la función devuelve 1 ya que se considera que los dos números borrosos son iguales.
4. `increasing_generation` y `decreasing_generation`: Generan todas las funciones respectivamente crecientes y decrecientes de $L_n$ a $Y_m$
5. `all_dfns`: Esta función recibe una n y m como input, también se puede especificar un tercer elemento, una lista $Y_m=[a_1=0<a_2< ... <a_m]=1$. En caso de no especificarse, $Y_m$ será una lista de $m$ valores uniformemente espaciados en el intervalo [0,1]. El objetivo de esta función es calcular todos los elemntos de $\mathcal{A}_1^{L_n \times Y_m}$ haciendo uso de las funciones `increasing_generation` y `decreasing_generation`.
6. `merge_sort`: Esta función usa al algoritmo merge sort para ordenar un conjunto de números borrosos  de menor a mayor a partir de un orden dado.
7. `ordered_fuzzy_numbers`: Esta función usa `all_dfns` para generar todos los números borrosos de $\mathcal{A}_1^{L_n \times Y_m}$ y posteriormente usa `merge_sort` para ordenarlos
8. `aggregation`: Esta función recibe dos números borrosos de $\mathcal{A}_1^{L_n \times Y_m}$
y una función de agregación $f$ definida sobre la cadena finita 
$L_n$. Primero, se genera la lista ordenada de números borrosos mediante ordered_fuzzy_numbers; luego se localizan en ella las posiciones correspondientes a cada número borroso. A continuación, se aplica la función $f$ a estos índices, obteniéndose un nuevo índice; finalmente, se utiliza este índice para devolver el elemento (número borroso) correspondiente a la posición resultante, utilizando la misma lista ordenada.
