# Examen Parcial - izanherranz07

**Usuario GitHub:** izanherranz07
**Fecha:** 4 de noviembre de 2025
**Retos tenidos en cuenta:** Reto 003

---

## Instrucciones

A continuación encontrarás fragmentos de código extraídos de tus entregas. Cada fragmento contiene una o más situaciones relacionadas con los conceptos vistos en clase.

Para cada pregunta debes:
1) Identificar a qué se refiere la observación
2) Explicar si es o no un error y por qué
3) Proponer la corrección

Nota: Responde 5 de las 9 preguntas (en función a lo indicado en el examen).


---

## Pregunta 1

Archivo: `RetoEdificioBase.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioBase.java) (Reto 003)

```java
final int PLANTAS = 8;
for (int dia = 1; dia <= 7; dia++) {
    for (int hora = 1; hora <= 24; hora++) {
        // ...
        for (int planta = PLANTAS - 1; planta >= 0; planta--) {
            // ... planta == 0 como planta baja
        }
    }
}
```

¿Qué observas en este código?

---

## Pregunta 2

Archivo: `RetoEdificioBase.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioBase.java) (Reto 003)

```java
if (persianaAbierta) {
    ventana = "[ ]";
} else if (luzEncendida) {
    ventana = "[*]";
} else {
    ventana = "[º]";
}
System.out.print(ventana + ":");
```

¿Qué observas en este código?

---
## Descripción funcional

Este fragmento corresponde a la parte del programa que decide qué símbolo ASCII mostrar para representar el estado visual de una habitación del edificio, en función de dos variables booleanas generadas aleatoriamente:

persianaAbierta: indica si la persiana de la habitación está abierta (probabilidad 70%).

luzEncendida: indica si la luz interior está encendida (probabilidad 60%).

El flujo lógico es:

Si la persiana está abierta → se asigna "[ ]".

Si la persiana está cerrada pero la luz está encendida → [*].

Si la persiana está cerrada y la luz está apagada → [º].

Finalmente, el resultado se imprime concatenado con un separador ":".

## Observaciones y posibles errores de lógica

- Inversión de semántica respecto al enunciado

El enunciado del reto dice que “las persianas cierran por completo la visibilidad de la habitación hacia el exterior”.
Esto implica que:

Si la persiana está abierta, sí debería verse el estado interior (la luz encendida o apagada).

Si la persiana está cerrada, no debería verse nada dentro, solo una ventana opaca.

Sin embargo, el código hace lo contrario:

Cuando persianaAbierta == true, imprime "[ ]", que simboliza una ventana cerrada.

Solo cuando la persiana está cerrada (else) muestra el estado de la luz ([*] o [º]).

## Recomendación de corrección

La condición debería expresarse de forma que si la persiana está abierta se vea la luz o la oscuridad interior.
Una versión corregida sería:

```java
if (persianaAbierta) {
    if (luzEncendida) {
        ventana = "[*]";
    } else {
        ventana = "[º]";
    }
} else {
    ventana = "[ ]";
}
System.out.print(ventana + ":");
```


Ahora la relación lógica es coherente:

Persiana abierta → se ve la luz si está encendida o un símbolo de luz apagada si está apagada.

Persiana cerrada → ventana opaca sin visibilidad interior.

Otras observaciones menores de estilo y buenas prácticas

Es preferible usar nombres más explícitos para los símbolos, por ejemplo:

```java
final String VENTANA_CERRADA = "[ ]";
final String LUZ_ENCENDIDA = "[*]";
final String LUZ_APAGADA = "[º]";
```

Esto mejora la legibilidad y facilita modificaciones futuras.

La impresión ```java System.out.print(ventana + ":"); ``` podría construirse con un StringBuilder dentro del bucle para optimizar concatenaciones y controlar el formato global de la planta.
## Pregunta 3

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
int[] consumoPorDia = new int[8];
for (int dia = 1; dia <= 7; dia++) {
    // ...
    consumoPorDia[dia] += consumoHora;
}
```

¿Qué observas en este código?

---

## Qué hace este bloque de código

Se declara e inicializa un arreglo de enteros:
```java
int[] consumoPorDia = new int[8];
```

Este arreglo pretende almacenar el consumo total de energía (en luces encendidas) por cada día de la simulación.

Luego, dentro del bucle que itera los días:
```java
for (int dia = 1; dia <= 7; dia++)
```

se acumula el consumo horario de cada día en su posición correspondiente:
```java
consumoPorDia[dia] += consumoHora;
```

Es decir, para cada dia, el valor acumulado del consumo energético total se guarda en consumoPorDia[dia].

## Observación principal: error

En Java, los índices de los arreglos comienzan en 0.
Por tanto, un arreglo declarado como new int[8] tiene índices válidos del 0 al 7.

Sin embargo, el bucle recorre los días con valores 1, 2, ..., 7, y usa directamente ese valor como índice del arreglo:

```java
consumoPorDia[dia] += consumoHora;
```
Usar índices base 0, ajustando la referencia al arreglo:
```java
int[] consumoPorDia = new int[7]; // Días 0 a 6
for (int dia = 0; dia < 7; dia++) {
    consumoPorDia[dia] += consumoHora;
}
```

Y, cuando se desee mostrar el día en pantalla, simplemente sumar 1 al número del día:

```java
System.out.println("Día " + (dia + 1));
```
El código funciona correctamente, pero presenta una inconsistencia en el uso del índice del arreglo: se declara new int[8] y se usan las posiciones 1–7, dejando el índice 0 sin utilizar.
Aunque no genera error en ejecución, no es una práctica limpia en Java.
Lo más correcto sería declarar el arreglo de tamaño 7 e indexar desde 0, o bien mantener el tamaño 8 pero documentar explícitamente que la posición 0 se ignora por motivos de legibilidad.

## Pregunta 4

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
boolean hayMantenimiento = random.nextDouble() < 0.05;
int plantaMantenimiento = hayMantenimiento ? (1 + random.nextInt(7)) : -1;
// ...
if (planta == plantaMantenimiento) {
    ventana = "[#]";
}
```

¿Qué observas en este código?

---

## Pregunta 5

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
System.out.println("CONSUMOS: ");
for (int d = 1; d <= dia; d++) {
    System.out.print("D" + d + ": " + consumoPorDia[d] + " | ");
}
```

¿Qué observas en este código?

---

## Propósito del código

Este bloque de código tiene como objetivo mostrar en consola el consumo acumulado por día hasta el momento de la simulación.

consumoPorDia es un arreglo que almacena el consumo energético (por ejemplo, cantidad de luces encendidas) de cada día.

El bucle for recorre todos los días transcurridos hasta el actual (d <= dia) y muestra su respectivo consumo acumulado.

Ejemplo de salida:
```java
CONSUMOS: 
D1: 611 | D2: 570 | D3: 642 | 
```

## Aspecto funcional

El bloque cumple correctamente su función de reporte acumulativo:
por cada hora simulada, imprime el consumo total de los días completados hasta ese punto.

```java System.out.println("CONSUMOS: "); ``` imprime el encabezado.

El bucle ```for```java muestra todos los días desde el día 1 hasta el día actual ```(dia)```, con el formato D# : valor |.

No hay errores de ejecución en este fragmento siempre que:

```consumoPorDia``` haya sido correctamente inicializado (por ejemplo, new int[8]).

El bucle de días (dia) comience en 1.

## Observación principal: uso de índices desde 1

El código usa índices del 1 al 7 para recorrer el arreglo consumoPorDia, lo cual implica que la posición 0 nunca se utiliza.

Esto se debe a que el arreglo fue definido como:
```java
int[] consumoPorDia = new int[8];
```

(índices válidos del 0 al 7).
Sin embargo, el bucle comienza desde 1, dejando el índice 0 vacío.

- Esto no genera error en ejecución, pero rompe la convención estándar de indexado en Java, donde los arreglos comienzan en 0.
Aun así, parece haber sido una decisión intencional del programador para que el número de índice coincida con el número de día (por legibilidad en la impresión).

Esto permite visualizar de forma progresiva cómo evoluciona el consumo total del edificio a lo largo de la semana.
## Pregunta 6

Archivo: `RetoEdificioBase.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioBase.java) (Reto 003)

```java
try {
    Thread.sleep(300); // Pausa para ver el cambio (opcional)
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

¿Qué observas en este código?

---

## Pregunta 7

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
int[] consumoPorDia = new int[8];

for (int dia = 1; dia <= 7; dia++) {
```

¿Qué observas en este código?

---
## Declaración e inicialización del arreglo

La línea:
```java
int[] consumoPorDia = new int[8];
```

declara un arreglo de tipo entero con capacidad para 8 elementos, indexados desde 0 hasta 7.
Este arreglo se usa para almacenar el consumo total diario de energía eléctrica del edificio, día por día, a lo largo de una simulación semanal.

El bucle for:
```java
for (int dia = 1; dia <= 7; dia++)
```

## Posible confusión semántica

El hecho de declarar un arreglo de 8 posiciones para una semana de 7 días puede llevar a errores futuros o malentendidos de mantenimiento.
Cualquier programador que lea el código puede pensar que hay 8 días simulados o que se está cometiendo un error en los índices.

itera desde el valor 1 hasta 7 inclusive, representando los 7 días de la semana simulada (Día 1 → Día 7).

## Mejora del código

Este código se puede mejorar:

— Usando índices base 0 (más idiomático en Java)
```java
int[] consumoPorDia = new int[7];

for (int dia = 0; dia < 7; dia++) {
    // usar consumoPorDia[dia]
    // mostrar día como (dia + 1)
}
```

El fragmento de código funciona correctamente, pero presenta una inconsistencia en el manejo de índices del arreglo.
Se declara un arreglo de 8 posiciones ```java ([0–7]) ``` y solo se utilizan las posiciones ```java [1–7]``` , dejando la posición ```java [0] ``` sin uso.
Aunque esto no genera errores en tiempo de ejecución, viola el principio de código limpio y las convenciones de indexado en Java.
Lo más recomendable sería ajustar el tamaño del arreglo a 7 elementos e iterar desde 0 a 6, o mantener el índice desde 1 documentando explícitamente que la posición 0 está reservada o no se usa.

## Pregunta 8

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
if (rayoCayo && hora == 8) {
    System.out.println(" Un rayo ha inutilizado la columna " + columnaAveriada);
}
if (hayMantenimiento && hora == 8) {
    System.out.println(" Planta " + plantaMantenimiento + " en mantenimiento");
}
```

¿Qué observas en este código?

---

## Pregunta 9

Archivo: `RetoEdificioExtendido.java` — [Ver archivo](https://github.com/mmasias/25-26-PRG1/blob/5775efe8e9a4ee060cbcd6372a5e10ffabe22ba1/entregas/Reto-003/RetoEdificioExtendido.java) (Reto 003)

```java
if (persianaAbierta) ventana = "[ ]";
else if (luzEncendida) {
    ventana = "[*]";
    consumoHora++;
} else ventana = "[º]";
```

¿Qué observas en este código?

---

## Funcionalidad del bloque condicional

Este fragmento de código controla la representación visual (en ASCII) del estado de cada habitación en el edificio, de acuerdo con las siguientes condiciones:
```java
persianaAbierta → [ ]
```
La persiana está abierta, por lo tanto, no se ve el interior de la habitación.
```java
luzEncendida → [*]
```
La persiana está cerrada y la luz está encendida, por lo que desde el exterior se observa el brillo de la luz.
Además, se incrementa el contador de consumo horario (consumoHora++), reflejando que la habitación consume energía.
```java
Caso contrario (else) → [º]
```
La persiana está cerrada y la luz está apagada, por lo que no hay consumo eléctrico ni iluminación visible.

En conjunto, este bloque modela correctamente la interacción entre la visibilidad de la persiana y el estado de la iluminación interior, mostrando un comportamiento coherente con la descripción del reto.

## Aspecto a mejorar: claridad de lectura (código limpio)

Aunque el código es funcional, podría mejorarse en legibilidad y consistencia de estilo, de acuerdo con las buenas prácticas vistas en clase (“código limpio”):

Recomendación: emplear llaves {} en todas las ramas, incluso cuando solo haya una instrucción.
Esto previene errores futuros si se agregan más líneas o se reestructura el código:
```java
if (persianaAbierta) {
    ventana = "[ ]";
} else if (luzEncendida) {
    ventana = "[*]";
    consumoHora++;
} else {
    ventana = "[º]";
}
```
## Revisión lógica: orden correcto

El orden de las condiciones es adecuado:

Se evalúa primero si la persiana está abierta, porque en ese caso no importa el estado de la luz, ya que no se verá desde el exterior.

Solo si la persiana está cerrada, se considera el estado de la luz.

Finalmente, si ninguna condición se cumple, la ventana se muestra como “[º]”.

Esto muestra un entendimiento correcto del enunciado del reto y refleja una buena modelización del problema.

El bloque condicional cumple correctamente su función: determina la apariencia de cada ventana según el estado de la persiana y la luz, y contabiliza el consumo eléctrico cuando la luz está encendida.
Su lógica es correcta y coherente con el modelo del problema, pero podría mejorarse en estilo y legibilidad mediante el uso sistemático de llaves ```{}``` , nombres de variables más expresivos y una indentación uniforme.
Desde la perspectiva del código limpio, el comportamiento es correcto, pero se recomienda reforzar la claridad y consistencia del formato para facilitar su mantenimiento y escalabilidad futura.
