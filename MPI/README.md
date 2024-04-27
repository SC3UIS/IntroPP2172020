# Segundo parcial MPI
En el presente repo se presenta el desarrollo del segundo parcial en parejas sobre MPI (Message Passing Interface)

## Integrantes:

- **Kevin Santiago Gutierrez Mendez - 2172020**
- **Jhon Edinson Salazar lemus - 2222962**

# Desarrollo
## Descripción del Código MPI para Ordenamiento y Búsqueda Binaria

Este código implementa el uso de MPI (Message Passing Interface) para paralelizar un algoritmo de ordenamiento y búsqueda binaria en un entorno distribuido. A continuación, se detallan los aspectos más relevantes de MPI utilizados en el código y una breve explicación del funcionamiento del programa:

## Aspectos Relevantes de MPI Utilizados

- **MPI_Init:** Inicializa el entorno MPI, permitiendo la comunicación y cooperación entre los procesos de MPI.
  
- **MPI_Comm_rank:** Obtiene el rango (ID) del proceso actual en el comunicador dado. En este caso, el comunicador es `MPI_COMM_WORLD`, que incluye todos los procesos.
  
- **MPI_Comm_size:** Obtiene el tamaño (número de procesos) del comunicador.
  
- **MPI_Scatter:** Divide un arreglo (en este caso, la lista generada aleatoriamente) en partes más pequeñas y distribuye estas partes entre los procesos del comunicador.
  
- **MPI_Gather:** Recolecta los datos de todos los procesos y los reúne en un solo proceso (en este caso, el proceso con rango 0).
  
- **MPI_Bcast:** Transmite un dato desde un único proceso (en este caso, el proceso con rango 0) a todos los demás procesos del comunicador.

## Funcionamiento del Código

El código comienza generando una lista aleatoria de números y la divide entre los procesos MPI. Cada proceso ordena su sublista utilizando el algoritmo de ordenamiento de burbuja (`bubble_sort`). Después de la ordenación, los resultados se recopilan en un solo proceso (rango 0) y se ordenan nuevamente para tener una lista ordenada completa.

Luego, se genera una clave aleatoria de la lista ordenada completa, que se transmite a todos los procesos utilizando `MPI_Bcast`. El proceso con rango 0 realiza una búsqueda binaria (`binary_search`) de esta clave en la lista ordenada completa.

Finalmente, se calcula el tiempo tomado para la ordenación y la búsqueda binaria, y se muestra el resultado junto con la lista ordenada y la clave aleatoria generada.
## Ejecucion dentro de Guane

dentro de guane, se realiza la carga del modulo MPI

```bash
  module av
  module load devtools/mpi/openmpi/4.0.2
```
reserva interactiva 
```bash
  srun -n 4 --pty /bin/bash
```
compilacion y ejecucion
```bash
  mpicc mpi_BinarySearch.c -o mpi_BinarySearch
  mpirun -np 4 mpi_BinarySearch
```
## Análisis de ejecución
    
| Secuencial (s) |   OPenMP  (s)  |     MPI (s)   |
|----------------|----------------|---------------|
|   0,000027     |    0.000005    |   0.000003    |

### Conclusion
En la compilacion del programa se jugo con el tipo de banderar o1, o2, o3, sin embargo el cambio en muy pocos segundo no fue tan relevante de resalta. Por otro lado, en el tiempo que se evidencia en la tabla anterio, podemos observar que para la medicion especifica del ordenamiento de la lista, el que resulta mas eficaz es el que usan MPI. Podemo concluir que es evidente que MPI se puede usar para aplicaciones que necesitan un gran paralelismo ya que garantiza la distribución eficiente del trabajo en entornos distribuidos.

## Descripción del Código MPI para Postman Sort

Este proyecto implementa un algoritmo de ordenamiento que utiliza tanto MPI (Message Passing Interface) como OpenMP (Open Multi-Processing) para mejorar el rendimiento en entornos de computación de alto rendimiento (HPC). El objetivo es demostrar cómo las técnicas de paralelización pueden mejorar significativamente el tiempo de ejecución para tareas intensivas de computación, como el ordenamiento de grandes conjuntos de datos.

## Estructura del Código

El repositorio incluye los siguientes archivos:

-   `mpi_postmanSort.c`: Código principal utilizando MPI para distribuir la carga de trabajo en múltiples nodos.
-   `omp_postmanSort.c`: Versión que utiliza OpenMP para el paralelismo a nivel de hilos en un único nodo.
-   `run_mpi_postmanSort.sbatch`: Script para ejecutar `mpi_postmanSort.c` en un sistema con SLURM usando sbatch.
-   `run_omp_postmanSort.sbatch`: Script para ejecutar `omp_postmanSort.c` en un sistema con SLURM usando sbatch.

## Instrucciones de Compilación y Ejecución

### Compilación

#### Para MPI:

```bash
module load devtools/mpi/openmpi/4.1.2
mpicc -o mpi_postmanSort mpi_postmanSort.c
```

#### Para OpenMP:

```bash
module load devtools/gcc/9.2.0
gcc -fopenmp -o omp_postmanSort omp_postmanSort.c
```

### Compilación en Modo Interactivo

#### Para MPI:

```bash
module load devtools/mpi/openmpi/4.1.2
mpirun -np 4 ./mpi_postmanSort
```

#### Para OpenMP:

```bash
module load devtools/gcc/9.2.0
export OMP_NUM_THREADS=4
./omp_postmanSort
```

### Ejecución en Modo Pasivo (usando sbatch)

```bash
sbatch run_mpi_sort.sbatch
sbatch run_omp_sort.sbatch
```

## Tiempos de Ejecución y Análisis de Resultados

-   **Original (Serial)**: `0.000000 seconds`
  
  ![image](https://github.com/jhonsl/IntroPP2222962/assets/69093836/a8a029b1-637d-478e-9cf9-07106dbf4144)

-   **MPI**: `0.000088 seconds`
  
  ![image](https://github.com/jhonsl/IntroPP2222962/assets/69093836/24902ce4-79ed-4513-a541-1cffc54763be)

-   **OpenMP**: `0.000057 seconds`
  
  ![image](https://github.com/jhonsl/IntroPP2222962/assets/69093836/c4bf1c26-208c-4ce6-bc7c-e24fbe2c6bd1)


Estos tiempos muestran que, aunque la versión MPI tiene un tiempo ligeramente mayor, esto se debe principalmente a la sobrecarga de la inicialización y comunicación de MPI. La versión OpenMP muestra una mejora significativa en el tiempo de ejecución, aprovechando efectivamente la arquitectura de memoria compartida para reducir el tiempo de procesamiento. La implementación OpenMP fue la más rápida debido a su eficiencia en la utilización de recursos computacionales locales y menor sobrecarga en comparación con MPI en este contexto específico de una carga de trabajo relativamente pequeña.
