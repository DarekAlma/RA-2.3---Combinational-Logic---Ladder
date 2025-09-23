# RA 2.3: Industrial Automation – Chemical Tank Level Monitoring 

## Introducción

  Objetivo del proyecto: monitorear niveles de un tanque químico mediante sensores discretos y un PLC.

  Breve explicación de por qué es importante: seguridad, evitar desperdicios, control de procesos.

## 1️. Diseño lógico (Combinacional)

  ### Tabla de verdad
  
    Incluye la tabla con b1, b2, b3 → H1–H5 (la que armamos).
  
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
