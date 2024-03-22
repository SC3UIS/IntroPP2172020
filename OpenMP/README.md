# Primer parcial OpenMP
En el presente repo se presenta el desarrollo del primer parcial sobre memoria compartida utilizando OpenMp

## Desarrollo 

### Explicacion del codigo
El codigo de busqueda binaria toma una lista de tamaño n de valores ingresados por el usuario, posteriormente mediante
el algoritmo de ordenamiento de la burbuja ordena de menor a mayor valor la lista. Una vez ordenada la lista, se empieza la busqueda
dividiendo la lista en dos mitades, una "superior" y otra "inferior" y apartir de aca dependiendo si el valor es mayor o menor que el valor
medio de la lista se empieza a iterar recursivamente la busqueda hasta encontrar el numero. El codigo original fue modificado levemente para que 
generara numeros aleatorios y rellenara la lista sin solicitar datos al usuario.

### Implementacion de paralelizacion
El bloque de codigo que se decidio paralelizar fue el siguiente
```c
   #pragma omp num_threads(1) parallel shared(list)
    {
        bubble_sort(list, SIZE);
    }
```
En el ordenamiento de burbuja se identifico la mayor complejida del codigo. Este ordenamiento cuenta con 3 for anidados. La razon
por la cual se decidio paralelizar el llamado a la funcion y no directamente el bloque de codigo de la funcion es porque de esta forma la paralelización
se aplicará a todo el proceso que sigue a esa directiva hasta su finalización. Esto incluiría la llamada a bubble_sort, la ordenación de la lista, y cualquier operación que ocurra 
dentro de la funcion como tal.


