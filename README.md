# ¿Cómo funciona el Generador de MisticaLang?

Este archivo explica de manera sencilla cómo el archivo `MisticaLangGenerator.xtend` traduce las palabras "mágicas" de MisticaLang a código real en **Java**. Cada función del generador tiene una tarea específica.

Aquí te mostramos qué hace cada función con ejemplos claros:

---

## 1. La Estructura del Programa
### `generateProgram(Model m)`
Crea el cascarón de un programa en Java (la clase principal y el método `main`). Abre el `Scanner` para poder leer el teclado y luego traduce todas las oraciones de adentro.

* **MisticaLang:**
  ```mistica
  Veamos que dicen las cartas acerca de Juego
  deja canalizo tu energia
    // ...
  si eso fue todo nos vemos una proxima vez
  ```
* **Java:**
  ```java
  import java.util.Scanner;
  public class Juego {
      public static void main(String[] args) {
          Scanner scanner = new Scanner(System.in);
          // ...
          scanner.close();
      }
  }
  ```

---

## 2. Creando Variables (Las Energías)

### `generateArcano(Arcano a)`
Crea variables de números enteros (`int`). Si no le das un valor, le pone `0`.
* **MisticaLang:** `surge el arcano 5 de poder`
* **Java:** `int poder = 5;`

### `generateOraculo(Oraculo o)`
Crea variables de texto (`String`).
* **MisticaLang:** `el oraculo de nombre nos indica que "Mimi"`
* **Java:** `String nombre = "Mimi";`

### `generatePendulo(Pendulo p)`
Crea variables de verdadero/falso (`boolean`). Transforma el "si" en `true` y el "no" en `false`.
* **MisticaLang:** `el pendulo de la suerte se inclina hacia el si`
* **Java:** `boolean suerte = true;`

---

## 3. Modificando y Leyendo Datos

### `generateAssignment(EObject a)`
Sirve para cambiar el valor de una variable que ya existe por uno nuevo.
* **MisticaLang:** `poder vibra y se transforma 10`
* **Java:** `poder = 10;`

### `generateScanner(EObject s)`
Pausa el programa y deja que el usuario escriba algo en el teclado para guardarlo en una variable. Detecta automáticamente si debe leer un número, un texto o un sí/no.
* **MisticaLang:** `la energia requiere poder`
* **Java:** `poder = Integer.parseInt(scanner.nextLine());`

### `generateRevelation(Revelation r)`
Imprime un mensaje o variable en la pantalla.
* **MisticaLang:** `las energias se canalizan y nos revelan que poder`
* **Java:** `System.out.println(poder);`

---

## 4. Condiciones (Los Caminos)

### `generateConditional`, `generateIfBranch`, `generateElseIfBranch`, `generateElseBranch`
Traducen las reglas del tipo "Si pasa esto, haz esto; de lo contrario, haz esto otro" (el famoso `if / else if / else`).

* **MisticaLang:**
  ```mistica
  Saquemos las primeras cartas 
  el sol brilla si poder es mas fuerte que 5
      las energias se canalizan y nos revelan que "Ganaste"
  la luna ilumina
      las energias se canalizan y nos revelan que "Perdiste"
  ese fue el mensaje de las cartas
  ```
* **Java:**
  ```java
  if (poder > 5) {
      System.out.println("Ganaste");
  } else {
      System.out.println("Perdiste");
  }
  ```

---

## 5. Ciclos (Las Repeticiones)

### `generateWhileLoop(EObject w)`
Hace que una serie de instrucciones se repita **mientras** se cumpla una condición (un bucle `while`).

* **MisticaLang:**
  ```mistica
  mientras se busca que poder es mas debil que 10
      poder vibra y se transforma poder crece gracias a 1
  se llega al equilibrio y se pide un consejo
  ```
* **Java:**
  ```java
  while (poder < 10) {
      poder = poder + 1;
  }
  ```

---

## 6. Operaciones Matemáticas y Lógicas

### `generateCondition(Condition c)` y `generateExpression(Expression expr)`
Traducen la "sintaxis mística" a operaciones matemáticas y lógicos de la computadora.

**Matemáticas:**
* `se une` / `crece gracias a` ➔ `+` (Suma o concatenación)
* `pierde` ➔ `-` (Resta)
* `se invierte` ➔ `*` (Multiplicación)
* `se reparte entre` ➔ `/` (División)

**Condiciones lógicas:**
* `armoniza` / `alcance` ➔ `==` (Igual que)
* `se opone` ➔ `!=` (Diferente que)
* `es mas fuerte que` ➔ `>` (Mayor que)
* `es mas debil que` ➔ `<` (Menor que)
* `y` ➔ `&&` (AND lógico)
* `o` ➔ `||` (OR lógico)

**Además:** Las constantes mágicas de la gramática como `EL_MAGO` o `EL_LOCO` son escaneadas por estas funciones para convertirse automáticamente en sus valores numéricos reales (`1` y `0` respectivamente).
