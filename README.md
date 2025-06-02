# Actividad de seguimiento - Simulación 2

|Integrante|correo|usuario github|
|---|---|---|
|Silvio Jose Otero Guzman|silvio.otero@udea.edu.co|silviootero|
|Gaia Ramirez Hincapie|gaia.ramirez@udea.edu.co|gaiamilenium99|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).


## Homework (Simulation)

This program, [mlfq.py](mlfq.py), allows you to see how the MLFQ scheduler presented in this chapter behaves. See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-sched-mlfq/README.md) for details.


### Questions

1. Run a few randomly-generated problems with just two jobs and two queues; compute the MLFQ execution trace for each. Make your life easier by limiting the length of each job and turning off I/Os.
![Parte 1](https://drive.google.com/uc?export=view&id=1Fttanw4sdFtkCSsPgt1ze0UG1YuzJV6G)
![Parte 2](https://drive.google.com/uc?export=view&id=1Cd6NpeBc2llEYChcshn7IRmvnG6K4Bnm)
2. How would you run the scheduler to reproduce each of the examples in the chapter?
   
   <details>
   <summary>Answer</summary>
   Para reproducir los ejemplos, se debe usar la misma configuración que se menciona en cada uno. Esto incluye la cantidad de trabajos, cuántas colas de prioridad hay, cuánto dura cada turno y si se activan o no características especiales como el refuerzo de prioridad o las reglas antiguas del planificador. Siguiendo los mismos parámetros, se puede correr el simulador y deberíamos obtener un resultado muy parecido al del ejemplo, incluyendo el orden en que se ejecutan los trabajos y el tiempo que tardan en completarse.
   </details>
   <br>

3. How would you configure the scheduler parameters to behave just like a round-robin scheduler?

   <details>
   <summary>Answer</summary>
   Para que el scheduler funcione como Round-Robin, se usa una sola cola para todos los trabajos y definir un turno fijo de tiempo para cada uno. Además, hay que asegurarse de que no se cambie la prioridad de los trabajos ni se apliquen reglas especiales, como subirlos o bajarlos de cola. De esta manera, todos los trabajos se van turnando, uno después del otro, como en un carrusel. Esta configuración tendria el comportamiento clásico de Round-Robin, donde ningún trabajo tiene mayor prioridad que otro.
   </details>
   <br>

4. Craft a workload with two jobs and scheduler parameters so that one job takes advantage of the older Rules 4a and 4b (turned on
with the -S flag) to game the scheduler and obtain 99% of the CPU over a particular time interval.

   <details>
   <summary>Answer</summary>
   En la simulación MLFQ sin boost y con dos colas, Job 0 (con I/O frecuente) y Job 1 (sin I/O) compiten por el CPU; Job 0 comienza ejecutándose pero entra en I/O cada 5 ticks y, gracias a la opción -S, permanece en su cola tras volver; mientras tanto, Job 1, al no hacer I/O, agota su allotment y baja de prioridad, pero termina usando el CPU desde la cola baja debido a la ausencia de boost, lo que provoca que Job 0, aunque con prioridad inicial más alta, quede frecuentemente bloqueado esperando CPU.
   </details>
   <br>
![Parte 1](https://drive.google.com/uc?export=view&id=1phEvGtI_yibchTS9pYthgsPHWLUHirgO)
![Parte 2](https://drive.google.com/uc?export=view&id=13Y6hU4LU_D7ioker-TyFE0rKZqM2Rint)

5. Given a system with a quantum length of 10 ms in its highest queue, how often would you have to boost jobs back to the highest priority level (with the `-B` flag) in order to guarantee that a single longrunning (and potentially-starving) job gets at least 5% of the CPU?

   <details>
   <summary>Answer</summary>
   Se debe hacer boost al job cada 200 ms para garantizar que reciba al menos un 5% del CPU.
   </details>
   <br>
![Parte 1](https://drive.google.com/uc?export=view&id=1AJn8_Cf6X91gVwkUmYtvkFwypZ0FHqJf)

6. One question that arises in scheduling is which end of a queue to add a job that just finished I/O; the -I flag changes this behavior
for this scheduling simulator. Play around with some workloads and see if you can see the effect of this flag.

   <details>
   <summary>Answer</summary>
   Al correr python mlfq.py -Q 5,10 -A 1,2 -l 0,50,5:0,50,0 -I -c, el programa crea dos niveles de prioridad para los trabajos, donde cada nivel tiene un tiempo máximo para ejecutarse y un número de oportunidades. Hay dos trabajos que empiezan juntos: uno hace pausas para hacer operaciones de entrada/salida (I/O) y el otro no. El trabajo con pausas usa su primer nivel de prioridad, pero como tiene poco tiempo, luego baja a la prioridad más baja donde puede usar más tiempo. Cada vez que termina una pausa, su prioridad sube para que pueda seguir avanzando. Mientras tanto, el otro trabajo aprovecha para correr en el nivel más alto. La opción -I activa que, después de una pausa de I/O, el trabajo sube de prioridad (lo que se llama “iobump”), ayudando a que no se quede esperando demasiado tiempo. Si no se usa -I, el trabajo que hace pausas bajaría su prioridad cada vez que termine su quantum, lo que puede hacer que avance más lento porque pasa más tiempo en las colas bajas. Así, usar -I favorece que los trabajos con pausas no se retrasen demasiado y mantengan mejor sus tiempos.
   </details>
   <br>
![Parte 1 I](https://drive.google.com/uc?export=view&id=1O8kgQU9bssdDgtVFy9OG1fe3KCbhpQHI)
![Parte 2 I](https://drive.google.com/uc?export=view&id=1vhmPZHUhOqkx5NU-fqgwgM12AkZzWRkV)
![Parte 1 I c](https://drive.google.com/uc?export=view&id=1ljQ-b0n1h-fO7nbEiXSTRJdCX8aGetY1)
![Parte 2 I c](https://drive.google.com/uc?export=view&id=1lQgQHvQ76UJFVkRR5QsjMSTLfO8Pz1qi)

## Conclusions

- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `Esta simulación del scheduler MLFQ permitió explorar su comportamiento bajo diferentes configuraciones, demostrando cómo ajustar parámetros como el número de colas, la duración del quantum y las reglas de prioridad puede replicar esquemas como Round-Robin o permitir que ciertos trabajos aprovechen políticas antiguas para obtener más tiempo de CPU.` 
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `Se comprobó que un boost cada 200 ms evita la inanición de tareas largas, mientras que el flag -I mejora la respuesta de trabajos con I/O.`
- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `MLFQ ofrece flexibilidad pero requiere una configuración cuidadosa para equilibrar equidad y eficiencia en entornos multitarea.`


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
- [x] Sección con las conclusiones de los experimentos realizados.
