# Descripción
GDB (GNU Debugger) es una herramienta de depuración utilizada principalmente en sistemas Unix y Linux para analizar y corregir programas escritos en lenguajes como C, C++ y otros. Permite ejecutar un programa paso a paso, inspeccionar el contenido de variables, modificar valores en tiempo de ejecución, establecer puntos de interrupción (breakpoints) y examinar la memoria y los registros del procesador. Su objetivo principal es ayudar a los desarrolladores a identificar errores lógicos, fallos de ejecución y comportamientos inesperados en el código, ofreciendo un control detallado sobre el flujo del programa y su estado interno.

## Comandos útiles

| **Comando**                          | **Descripción**                                                                 |
|--------------------------------------|---------------------------------------------------------------------------------|
| `gdb <programa>`                     | Inicia GDB con el programa especificado.                                       |
| `run`                                | Ejecuta el programa dentro de GDB.                                             |
| `break <función/línea>`              | Coloca un punto de interrupción (**breakpoint**) en una función o línea.       |
| `info breakpoints`                   | Muestra los puntos de interrupción activos.                                    |
| `delete <n>`                         | Elimina el **breakpoint** número n.                                            |
| `continue`                           | Continúa la ejecución después de un **breakpoint**.                            |
| `next`                               | Ejecuta la siguiente línea (sin entrar en funciones).                          |
| `step`                               | Ejecuta la siguiente línea (entrando en funciones si las hay).                 |
| `finish`                             | Ejecuta hasta salir de la función actual.                                      |
| `print <variable>`                   | Muestra el valor de una variable.                                              |
| `x/<n><f> <dirección>`               | Examina memoria (n unidades de formato f en dirección dada). Ej: `x/4x 0xaddress` |
| `info registers`                     | Muestra el contenido de los registros del procesador.                          |
| `set <variable>=<valor>`             | Modifica el valor de una variable.                                             |
| `disassemble <función>`              | Muestra el código ensamblador de una función.                                  |
| `quit`                               | Sale de GDB.                                                                   |
