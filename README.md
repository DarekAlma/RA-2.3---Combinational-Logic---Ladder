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
  
  1. Principio general
  Una función booleana se deriva identificando las filas donde la salida vale 1. Para cada fila con salida = 1 se construye un producto lógico (AND) de las entradas.
    
  En ese producto, cada entrada va:
    
    Directa si en la fila su valor es 1.
      
    Negada (¬) si en la fila su valor es 0.

  Si una salida se enciende en más de una fila, se hace la suma lógica (OR, ∨) de todos esos productos.

  2. Aplicando al caso

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

  3. En resumen
  Cada función booleana es la traducción directa de la tabla:
  
  H1–H4 corresponden a un único minterm (una sola combinación de entradas).
  
  H5 corresponde a la OR de varios minterms, luego simplificada para reducir puertas.
  
  ### Circuito lógico
  
    Sube el diagrama de compuertas que ya dibujamos (imagen .png).

## 2️. Implementación en CODESYS y uso de HMI

  Explica la traducción: contactos normalmente abiertos/cerrados para representar variables y negaciones.
  
  Incluye el ladder de todos los rungs (con capturas o exportado).
  
  Explica cómo se usaron bobinas intermedias para simplificar el error (T1, T2 → H5).

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
