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
  
  
  ### Funciones booleanas
  
    Explica cómo sacaste cada función de la tabla (usa las ecuaciones finales):
    
    H1 = b1 ∧ b2 ∧ ¬b3
    
    H2 = b1 ∧ ¬b2 ∧ ¬b3
    
    H3 = b1 ∧ b2 ∧ b3
    
    H4 = ¬b1 ∧ ¬b2 ∧ ¬b3
    
    H5 = (¬b1 ∧ (b2 ∨ b3)) ∨ (b3 ∧ ¬b2)
  
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
