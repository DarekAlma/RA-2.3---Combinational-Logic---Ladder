# RA 2.3: Industrial Automation – Chemical Tank Level Monitoring 

## Introducción

Este proyecto tiene como objetivo diseñar e implementar una automatización combinacional para el monitoreo de niveles en un tanque químico utilizando sensores discretos y un PLC. El control de niveles es   fundamental para **evitar desbordes, fugas, desperdicios y liberar personal para que se puedan encargar de labores mas importantes**, garantizando seguridad en la planta y eficiencia en el consumo de recursos. Asi mismo busca desarrolar en los estudiantes la capacidad de usar herramientas como CODESYS y OPENPLC para hacer PLCs funcionales que puedan ser probados dentro de una simulación y pasados a hardware fisico y tener un resultado similar al visto en estas herramientas.

## 1️. Diseño lógico (Combinacional)

Se definieron para el proyecto tres entradas/inputs y cinco salidas/outputs, con el fin de simular el comportamiento que tendrian los sensores que detecten donde esta el nivel del liquido dentro del tanque y los actuadores que le indican al personal en que estado se encuentra el liquido y con esto poder definir que acción tomar. Es necesario pensar en como funcionara el sistema dependiendo de sus entradas y como el estado de estas genera una salida distinta. Por esto nos tenemos que basar en la logica combinacional para saber todas las posibles combinaciones que tien nuestro sistema.

  Inputs (Los tres seran considerados sensores):

    b1: Tanque vacio.
    
    b2: Lleno al minimo nivel.
    
    b3: Tanque muy lleno, al punto de desbordarse.

  Outputs (Los cinco seran considerados LEDs):

    H1: Nivel de llenado correcto
    
    H2: Nivel de llenado demasiado bajo
    
    H3: Nivel de llenado demasiado alto
    
    H4: Tanque vacio.
    
    H5: Error, estados no posibles.

Que led este prendido depende de la combinación de los valores de los tres sensores, y solo podra haber un led prendido a la vez, es decir cada led tiene una combinación unica de los estados de la entradas que hace que unicamente este se prenda. Para esto tenemos que hacer la tabla de verdad que es donde pondremos las distintas combinaciones en las que se pueden encontrar las entradas y cual es la salida esperada.

  ### Tabla de verdad

  ![Tabla de verdad](imagenes/TablaDeVerdad.jpg)

  Esta es la tabla de verdad resultante del comportamiento planteado en la actividad, cada entrada y salida tiene su respectiva columna y hay una columna extra que nos indica cual es el estado esperado a partir de las entradas. Cada fila tiene una combinación diferente en los valores que toman las entradas, siendo 0 ausencia del liquido en ese sensor y 1 presencia del liquido en ese sensor, la cantidad de filas que hay depende de la cantidad de entradas,en nuestro caso tenemos tres entradas, por ende operamos 2^3 = 8, la cantidad de filas que tiene la tabla de verdad. De H1 a H4, solo hay una combinación que permite dichas salidas, por el mismo planteamieto del ejercicio, pero para H5 hay cuatro combinaciones distintas que dan como resultado esta salida y esto se debe a que hay diferentes maneras en las cuales se podria presentar un error en el sistema. 
  
  
  ### Funciones booleanas
  
#### 1. Principio general
   
Una función booleana se deriva identificando las filas donde la salida vale 1. Para cada fila con salida = 1 se construye un producto lógico (AND) de las entradas.
      
En ese producto, cada entrada va:
      
      Directa si en la fila su valor es 1.
        
      Negada (¬) si en la fila su valor es 0.
  
Si una salida se enciende en más de una fila, se hace la suma lógica (OR, ∨) de todos esos productos.

#### 2. Aplicando al caso

  H1 (correct)

    Solo se enciende en la fila 110 (b1=1, b2=1, b3=0).
    
    Entonces: H1 = b1 ∧ b2 ∧ ¬b3

  H2 (too low)
  
    Solo se enciende en la fila 100 (b1=1, b2=0, b3=0).
    
    Entonces: H2 = b1 ∧ ¬b2 ∧ ¬b3
  
  H3 (too high)
  
    Solo se enciende en la fila 111 (b1=1, b2=1, b3=1).
    
    Entonces: H3 = b1 ∧ b2 ∧ b3
  
  H4 (empty)
  
    Solo se enciende en la fila 000 (b1=0, b2=0, b3=0).
    
    Entonces: H4 = ¬b1 ∧ ¬b2 ∧ ¬b3
  
  H5 (error)
  
    Se enciende en varias filas (001, 010, 011, 101).
    
    Si escribiéramos todas esas filas como minterms, tendríamos 4 productos OR.
    
    Pero podemos simplificar detectando el patrón común de inconsistencia:
    
      Error si b3=1 pero b2=0 → (b3 ∧ ¬b2)
      
      Error si b3=1 pero b1=0 → (b3 ∧ ¬b1)
      
      Error si b2=1 pero b1=0 → (b2 ∧ ¬b1)
    
    Resultado simplificado: H5 = (¬b1 ∧ (b2 ∨ b3)) ∨ (b3 ∧ ¬b2)

#### 3. En resumen
  Cada función booleana es la traducción directa de la tabla:
  
  H1–H4 corresponden a un único minterm (una sola combinación de entradas).
  
  H5 corresponde a la OR de varios minterms, luego simplificada para reducir puertas.
  
  ### Circuito lógico

  ### H1 – Fill Level Correct (110)

  ![](imagenes/CircuitoH1.jpg)
  
  El circuito se activa únicamente cuando **b1=1**, **b2=1** y **b3=0**.  
  - Se implementa con un **AND de tres entradas**, donde entran directamente b1 y b2, y la señal negada de b3 (¬b3).  
  - Esto garantiza que el LED H1 solo se encienda cuando los dos sensores inferiores están activos y el superior apagado, condición que representa el nivel deseado del tanque.
  
  
  ### H2 – Fill Level Too Low (100)

  ![](imagenes/CircuitoH2.jpg)

  Este circuito detecta cuando **solo el sensor inferior b1 está activo** (b1=1, b2=0, b3=0).  
  - Se implementa con un **AND de tres entradas** que combina b1, ¬b2 y ¬b3.  
  - De esta forma, H2 se enciende únicamente cuando el tanque tiene líquido en el nivel más bajo, indicando que el nivel es insuficiente.
  
  
  ### H3 – Fill Level Too High (111)

  ![](imagenes/CircuitoH3.jpg)
  
  Se enciende cuando **todos los sensores están activos** (b1=1, b2=1, b3=1).  
  - Se implementa con un **AND de tres entradas** sin negaciones, porque los tres deben estar en estado 1.  
  - Representa que el tanque está lleno y el nivel es demasiado alto.
  
  
  ### H4 – Tank Empty (000)

  ![](imagenes/CircuitoH4.jpg)
  
  Este circuito se activa cuando **ningún sensor detecta líquido** (b1=0, b2=0, b3=0).  
  - Se implementa con un **AND de tres entradas** de las señales negadas: ¬b1, ¬b2 y ¬b3.  
  - El LED H4 se enciende cuando el tanque está vacío.
  
  
  ### H5 – Error (inconsistencias de sensores)

  ![](imagenes/CircuitoH5.jpg)
  
  El circuito de error se enciende cuando las combinaciones de sensores no tienen sentido físico (ej. b3=1 y b2=0).  
  - Se construyó detectando dos condiciones principales:  
    - **¬b1 ∧ (b2 ∨ b3)** → cuando los sensores medio o alto detectan líquido, pero el inferior no.  
    - **b3 ∧ ¬b2** → cuando el sensor alto detecta líquido pero el medio está apagado.  
  - Ambas condiciones se combinan en un **OR**, encendiendo H5 en cualquier caso de fallo.  
  - Esto asegura que el sistema no muestre un estado válido si los sensores no son coherentes entre sí.
  

## 2️. Implementación en CODESYS y uso de HMI

### Explicación lenguaje ladder

  Ahora que ya tenemos los circuitos logicos podemos pasarlos a logica/diagrama ladder, en donde cambian un poco las cosas, su forma se asemeja a la de una escalera, es secuencial y se lee de izquierda a derecha y de arriba a abajo. Las entradas son representadas como "Contactos", [entrada], ubicadas en la parte izquierda de la escalera y estos pueden estar normalmente abiertos (1, si el evento es verdadero permite el flujo de corriente) o normalmente cerrados (0 / negación, permite flujo de corriente siempre hasta que el evento sea verdadero), las salidas son representadas como "bobinas", (salida), y estan ubicadas en la parte derecha de escalera. Para representar un AND se hace poniendo un contacto al lado del otro, es decir solo fluye la corriente si la condición de ambos se cumple, y para representar un OR se hace poniendo un contacto en paralelo con otro, es decir fluye la corriente si alguna o ambas de las dos condiciones se cumple.

En Ladder cada rung (cada fila, que se asemeja a un escalon de la escalera) es equivalente a una ecuación booleana:

    Si conectamos en serie [b1]--[b2] y al final (H1), estamos implementando H1 = b1 ∧ b2.
    
    Si ponemos contactos en paralelo [b1] y [b2] antes de (H2), implementamos H2 = b1 ∨ b2.
    
    Si usamos un contacto cerrado [/b3], representamos la negación ¬b3.

Diagram ladder que sale a partir de los circuitos logicos:

![Diagrama ladder a partir de los circuitos logicos](imagenes/DiagramaLadder2.jpg)

En este caso se tomo la decisión de hacer uso de dos bobinas extras (T1 y T2), para poder simplificar la operación de H5 en el diagrama ladder.

### CODESYS

¿Qué es CODESYS?

    Es un entorno de desarrollo integrado (IDE) para programar PLCs (Programmable logic controller) usando los lenguajes definidos en la norma IEC 61131-3 (Ladder, Function Block Diagram, Structured Text, etc.).
    
    Permite crear programas de automatización industrial y simularlos para probar su funcionamiento.
    
    Incluye un módulo de visualización/HMI para construir interfaces gráficas de monitoreo y control.

Definición de variables dentro de CODESYS:

![Diagrama ladder CODESYS](imagenes/DefinicionDeVariablesCODESYS.png)

Implementamos el diagrama ladder que obtuvimos dentro de CODESYS:

![Diagrama ladder CODESYS](imagenes/DiagramaLadderCODESYS.png)

HMI en CODESYS: 

![HMI](imagenes/HMICODESYS.png)

Una **HMI (Human-Machine Interface)** es la interfaz que permite la comunicación entre el operador y el proceso automatizado.  
Su función es **mostrar de manera gráfica la información del sistema** (niveles, alarmas, estados) y permitir que el usuario **interactúe con el PLC** de forma sencilla.

En este proyecto la HMI nos sirve para:
- Visualizar los estados de los sensores de nivel (**b1, b2, b3**).  
- Observar las salidas (**H1–H5**) representadas con indicadores luminosos.  
- Simular el comportamiento del tanque sin necesidad de conectar hardware físico.  

### ¿Cómo funciona dentro de CODESYS?
- CODESYS incluye un editor de Visualizations donde se pueden crear pantallas gráficas.  
- Cada elemento gráfico (botón, lámpara, texto) se vincula directamente a una variable del PLC.  

Ejemplo:
- Un **botón toggle** se conecta a la variable `b1` → al presionarlo simula que el sensor bajo se activa.  
- Una **lámpara** se conecta a la variable `H1` → se enciende si la lógica del PLC determina que el tanque está en el nivel correcto.  

Durante la simulación, el PLC ejecuta el programa en **Ladder**, y la HMI refleja en tiempo real el estado de las entradas y salidas.






## 3. Implementación en OPENPLC

  Explica la traducción: contactos normalmente abiertos/cerrados para representar variables y negaciones.
  
  Incluye el ladder de todos los rungs (con capturas o exportado).
  
  Explica cómo se usaron bobinas intermedias para simplificar el error (T1, T2 → H5).

## 4. Validación con OPENPLC y Hardware (Arduino Uno) 

  Entradas: DIP switch de 3 posiciones conectado a pines D2–D4 (%IX0.0–%IX0.2).
  
  Salidas: 5 LEDs con resistencias en pines D7–D11 (%QX0.0–%QX0.4).
  
  Fuente: 5 V del propio Arduino.
  
  Incluye foto o esquema del cableado.
  
  Configuración de OpenPLC
  
  Muestra cómo asignaste %IX/%QX a pines.

  Pega el arduino_settings.json corregido.


## 5️. Resultados y conclusiones

  La lógica combinacional funciona correctamente.
  
  El error se detecta para estados inválidos.
  
  Se logró simular en CODESYS y validar en hardware real con Arduino Uno.

El error se detecta para estados inválidos.

Se logró simular en CODESYS y validar en hardware real con Arduino Uno.
