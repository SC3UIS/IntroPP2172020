# Primer parcial OpenMP
En el presente repo se presenta el desarrollo del primer parcial sobre memoria compartida utilizando OpenMp

## Desarrollo

### Explicación del código
El código de busqueda binaria toma una lista de tamaño n de valores ingresados por el usuario, posteriormente mediante
el algoritmo de ordenamiento de la burbuja ordena de menor a mayor valor la lista. Una vez ordenada la lista, se empieza la búsqueda
dividiendo la lista en dos mitades, una "superior" y otra "inferior" y apartir de aca dependiendo si el valor es mayor o menor que el valor
medio de la lista se empieza a iterar recursivamente la búsqueda hasta encontrar el número. El código original fue modificado levemente para que
generara números aleatorios y rellenara la lista sin solicitar datos al usuario.

### Implementación de paralelización
El bloque de código que se decidió paralelizar fue el siguiente
```c
   #pragma omp num_threads(4) parallel shared(list)
   {
     bubble_sort(list, SIZE);
   }
```
En el ordenamiento de burbuja se identificó la mayor complejidad del código. Este ordenamiento cuenta con 3 for anidados. La razón
por la cual se decidió paralelizar el llamado a la función y no directamente el bloque de código de la función es porque de esta forma la paralelización
se aplicará a todo el proceso que sigue a esa directiva hasta su finalización. Esto incluiría la llamada a bubble_sort, la ordenación de la lista, y cualquier operación que ocurra
dentro de la función como tal.

### Compilación en Guane
Ya habiendo accedido a `GuaneExa` se realizaron los siguientes pasos para la compilación

#### Secuencial
```bash
  srun -n 10 -w ExaDELL --pty /bin/bash
  gcc  BinarySearch.c -o BinarySearch
  ./BinarySearch
```

#### Paralelo
para la compilación del código paralelizado se creó el archivo `BinarySearch.sbatch`, este se ejecutó de la siguiente forma:
```bash
  sbatch BinarySearch.sbatch
```
La salida de la compilacion y ejecucion se puede encontrar en el archivo `output_BinarySearch.txt`

### Resultados
Se pudo evidenciar que al paralelizar el segmento de código crítico con mayor complejidad, el tiempo de ejecución de este proceso
disminuyó en algunas milésimas de segundo. Se intentó jugar con la cantidad de hilos empleados mediante el atributo `num_threads` para ver qué cambios presentaba. También
se intentó ver las variaciones que presentaba incluyendo la paralelización en el llamado a la función y tambien dentro de la función justo antes del bucle for.
