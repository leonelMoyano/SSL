# Trabajo Practico 1 - Fases de la Traducción y Errores

## Objetivos

 * Identificar las fases de traducción y errores.

## Resolución


Una vez creado el archivo `hello2.c` 
Para preprocesar el archivo fuente puede correrse el siguiente comando:

``` gcc hello2.c -std=c11 -E -o hello2.i ```

Luego de correr este comando no obtenemos nada impreso por consola, obtenemos el archivo `hello2.i`


El archivo obtenido es el resultado del preprocesamiento. Lo primero que se lee en el archivo es el contenido del archivo de encabezado de la biblioteca standard, que ocupa la mayoria de las lineas del archivo. Al final del archivo, `hello2.i`, se lee nuestro codigo fuente preprocesado, los cambios que se notan son los siguientes:
 * Falta de la sentencia ``` #include <stdio.h> ```
 * Reemplazo de los comentarios por un espacio


Una vez creado el archivo `hello3.c` se analiza la sintaxis.
``` int printf(const char *s, ...); ```
La primer sentencia del archivo fuente es el prototipo de la funcion ```printf```.
``` int main(void){ ```
Luego se observa la declaración de la funcion ```main``` que devuelve un dato de tipo ```int``` y no toma ningun parametro.
``` int i=42; ```
Esta sentencia declara una variable de tipo ``` int ``` con el valor ``` 42 ```
``` prontf("La respuesta es %d\n"); ```
Se llama a la función ``` prontf ``` con un parametro de tipo ``` char[] ```


Para preprocesar el archivo fuente puede correrse el siguiente comando:

``` gcc hello3.c -std=c11 -E -o hello3.i ```

Luego de correr este comando no obtenemos nada impreso por consola, obtenemos el archivo `hello3.i`


La diferencia que se nota entre `hello2.i` y `hello3.i` es el contenido del archivo encabezado de la biblioteca standard dado que en el nuevo archivo fuente no esta presente la sentencia que importa el archivo de encabezado.

Para compilar el archivo fuente puede correrse el siguiente comando:

``` gcc hello3.c -std=c11 -S -o hello3.s ```

Luego de correr este comando obtenemos el siguiente output por consola:
```
hello3.c: In function ‘main’:
hello3.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
hello3.c:5:2: error: expected declaration or statement at end of input
```
Este mensaje nos indica que: 
 * La función ```prontf``` no fue declarada en el archivo fuente marcandolo como un **Warning**
 * La función ```main``` no es cerrada correctamente como un **Error**


Luego corrijo el **Error** en `hello3.c` en un nuevo archivo fuente llamado `hello4.c`.

Para compilar el archivo fuente puede correrse el siguiente comando:

``` gcc hello4.c -std=c11 -S -o hello4.s ```

Luego de correr este comando obtengo el siguiente mensaje por consola:
```
hello4.c: In function ‘main’:
hello4.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
```
También obtengo el archivo `hello4.s`.


El archivo `hello4.s` contiene sentencias de Assembler correspondientes al archivo fuente `hello4.c`.


Para ensamblar el archivo Assembler puede correrse el siguiente comando:

``` gcc hello4.s -std=c11 -c -o hello4.o ```

Luego de correr este comando no obtengo salida por consola, obtengo el archivo `hello4.o`.


Para ensamblar el archivo Assembler puede correrse el siguiente comando:

``` gcc hello4.o -std=c11 -o hello4 ```

Luego de correr este comando obtengo la siguiente salida por consola:
```
hello4.o: En la función `main':
hello4.c:(.text+0x1a): referencia a `prontf' sin definir
collect2: error: ld returned 1 exit status
```
El mensaje nos indica que la función `prontf` no esta definida.


Corrijo la llamada a la función sin definir cambiando el nombre de `prontf` a `printf` en un nuevo archivo fuente llamado `hello5.c`.

Para generar el ejectuble  puede correrse el siguiente comando:

``` gcc hello5.c -std=c11 -o hello5 ```

Luego de correr este comando obtengo la siguiente salida por consola:
```
hello5.c: In function ‘main’:
hello5.c:5:9: warning: format ‘%d’ expects a matching ‘int’ argument [-Wformat=]
  printf("La respuesta es %d\n");
         ^
```
El mensaje nos indica que faltan parametros en la llamada a la función `printf` como un **Warning** y obtengo el archivo `hello5`.


Para corregir esto llamo a la función añadiendo como parametro la variable `i` en un nuevo archivo fuente llamado `hello6.c`.


Para generar el ejectuble puede correrse el siguiente comando:

``` gcc hello6.c -std=c11 -o hello6 ```

Luego de correr este comando no obtengo salda por consola, obtengo el archivo `hello6`.
Luego de correr el comando

``` ./hello6 ```

Obtengo la siguiente salida:
`La respuesta es 42`


Una vez creado el archivo `hello7.c` y generado el ejecutable obtengo la siguiente salida:
```
hello7.c: In function ‘main’:
hello7.c:3:2: warning: implicit declaration of function ‘printf’ [-Wimplicit-function-declaration]
  printf("La respuesta es %d\n", i);
  ^
hello7.c:3:2: warning: incompatible implicit declaration of built-in function ‘printf’
hello7.c:3:2: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’
```
Comunica que la función `printf` no fue declarada como **Warning** pero obtengo el archivo `hello7` de todas formas.
Corro el comando:
``` ./hello7 ```
Obtengo la misma salida que obtuve con el archivo `hello6` debido a que mas allá de que la función `printf` no haya sido definida en el archivo fuente es parte de la libreria standard. Dado a que la definición de la función `printf` es parte de la libreria standard el programa funciona de todas formas.
