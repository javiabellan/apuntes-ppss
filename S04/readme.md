# Caja negra

También llamado **functional testing**. Obtener los casos de prueba a partir de la especificación.
Exiten muchos tipos de métodos según el nivel:

* Pruebas unitarias
  * **Método de particiones equivalentes**
  * Método de análisis de valores límite
  * Método de tablas de decisión
  * Método de grafos causa-efecto

* Pruebas de sistema
  * **Método de diagramas de transición de estados**
  * Método de pruebas basado en casos de uso

* Pruebas de aceptación
  * Método de pruebas basado en requerimientos
  * Método de pruebas basado en escenarios

# Método de particiones equivalentes
Partición = clase de equivalencia = subconjunto de datos de entrada

### 1. Análizar la especificación y particionar
Identificar de clases de equivalencia para cada E/S. (Ver entradas y salidas de forma clara).
Las particiones deben ser disjuntas y cubrir todo el conjunto S.

> V=clase válida N=clase inválida (R, L, C, son para aclararse y diferenciar las particiones)

* Rango: x = 1..12
  * **RV1**: dentro del rango
  * **RN1**: fuera por abajo
  * **RN2**: fuera por arriba
* Lista de tamaño 1 hasta n
  * **LV1**: tamaño correcto
  * **LN1**: lista vacía
  * **LN2**: lista muy grande
* Conjunto: x = (1,3,4)
  * **CV1**: Pertenece al cjto.
  * **CN1**: No pertenece al cjto.
* Debe ser: unaCondición(x)
  * **DV1**: Es
  * **DN1**: No es

De las particiones anteriores, si alguna tiene varias salidas distintas, **hay que particionarla**.

* Salidas
  * **SV1**: Una salida
  * **SV2**: Otra salda
  * **SN1**: Una salid no válida (suelen ser excepciones)

### 2. Selección de un conjunto de particiones
* Ejemplo
  * **RV1**-**LV1**-**CV1**-**DV1**-**SV1**
  * **RN1**-LV1-CV1-DV1-**SN1**
  * ...
* Pasar por **todas** las clases al menos una vez (no hay que probar todas las combinaciones).
* Las clases de entrada inválidas (**N**) se prueban de una en una (aislar entre ellas)
* Se permite que varias clases de entrada tengan la misma clase de salida.
* Una clase de entrada inválida tiene una clase de salida inválida.

### 3. Obtención del conjunto de casos de prueba
Elegir UN caso de prueba (seleccionar los datos de entrada) para cada par de clasesDeEntrada-claseDeSalida obtenidos.

> Recuerda que somos:
> * EFECTIVO: Probar todas las particiones.
> * EFICIENTE: No probar de más.

# Método de diagrama de transición de estados (STD)

Son pruebas a nivel de sistema. El diagrama representa a una **entidad** (una reserva por ejemplo).

> Un error común es incluir varias entidades (reservas y pasajeros por ejemplo).

### 0. Análisis de la especificación

Determinar:
* **Entidad**: ¿Qué entidad queremos modelar?
* **Estados**: Conjunto de valores de variables.
* **Transiciones**: Cambios entre estados (provocado por eventos).
* **Eventos**: Entradas que provocan cambio de estado (transición). Ej: `cancalar`
  * **Acciones**: (opcional) Operaciones tras la ocurrencia de un evento. Ej: `cancalar / Refund`
  * **Guardas**: (opcional) Parámetros del evento. Ej. `cancelar [returnTicket==true]`

### 1. Construcción del STD 

> Un estado inicial solo, pero puede haber varios estados fianles.

### 2. Seleccionar un conjunto de caminos
Conjunto que ejercite todas las transiciones

### 3. Obtención de casos de prueba
* Entradas: eventos
* resultado esperado: Secuencia de acciones correspondiente.

## Ejemplos y ejercicios
