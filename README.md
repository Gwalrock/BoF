# BoF

🔍 ¿Qué es un Buffer Overflow?​

Un buffer overflow (desbordamiento de búfer) ocurre cuando un programa escribe más datos de los que puede almacenar en una zona de memoria reservada (el "buffer"). Esto puede sobrescribir otras partes de la memoria, provocando errores o permitiendo la ejecución de código malicioso.​

🧠 ¿Por qué sucede?​

Los lenguajes como C no verifican automáticamente los límites de los arrays o buffers.​
Si el programador no controla el tamaño de los datos que se escriben, se puede sobrescribir memoria adyacente, incluyendo direcciones de retorno o variables importantes.​

🧱 ¿Qué es la pila (stack) y por qué es importante?​

La pila (stack) es una estructura de memoria que sigue el orden LIFO (Last In, First Out):​  
Se usa para almacenar variables locales, direcciones de retorno y parámetros de funciones.​  
Cuando se llama a una función, se apilan datos; al terminar, se desapilan.​

📌 Principales punteros involucrados​

ESP (Stack Pointer): apunta al tope de la pila.​  
EBP (Base Pointer): marca el inicio del marco de pila de la función actual.​  
EIP (Instruction Pointer): indica la próxima instrucción a ejecutar.​

⚠️ ¿Por qué es peligroso? Un atacante puede:​

Sobrescribir el EIP para redirigir la ejecución del programa.​  
Ejecutar código arbitrario (por ejemplo, abrir una shell o escalar privilegios).​  
Evitar protecciones si el binario no está compilado con medidas como ASLR, DEP o stack canaries.​

## Medidas de protección del SO


| **Protección**                              | **Descripción breve**                                                                                   | **Linux (por defecto)**                                      | **Windows (por defecto)**                     |
|--------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|----------------------------------------------|
| **ASLR (Address Space Layout Randomization)** | Aleatoriza las direcciones de memoria para dificultar la predicción de ubicaciones.                     | ✅ Activado (desde kernel 2.6.12+)                           | ✅ Activado (desde Vista)                    |
| **DEP / NX (Data Execution Prevention / No eXecute)** | Impide la ejecución de código en regiones de memoria marcadas como datos.                               | ✅ Activado (con CPU compatible)                             | ✅ Activado (desde XP SP2)                   |
| **Stack Canaries**                         | Inserta valores aleatorios antes del EIP para detectar sobreescrituras.                                 | ✅ Activado si se compila con `-fstack-protector`            | ✅ Activado en compilaciones modernas        |
| **Fortify Source**                         | Reemplaza funciones inseguras por versiones seguras en tiempo de compilación.                           | ✅ Activado si se compila con `-D_FORTIFY_SOURCE=2`          | ❌ No disponible nativamente                 |
| **SafeSEH / SEHOP**                        | Protege el manejo de excepciones estructuradas contra sobreescrituras.                                  | ❌ No aplica                                                 | ✅ Activado (SEHOP desde Windows 7)          |
| **Control Flow Guard (CFG)**               | Protege el flujo de ejecución contra redirecciones maliciosas.                                          | ❌ No disponible nativamente                                  | ✅ Activado (desde Windows 8.1/10)           |
| **RELRO (Partial/Full)**                   | Protege las tablas de resolución de funciones en binarios ELF.                                          | ✅ Activado si se compila con `-Wl,-z,relro`                 | ❌ No aplica directamente                    |
| **PIE (Position Independent Executable)**   | Permite que el binario se cargue en direcciones aleatorias.                                             | ✅ Activado en muchas distros modernas                        | ❌ No aplica directamente                    |

## Medidas de protección en C

| **Medida / Técnica**                     | **Descripción**                                                                                                   | **¿Evita buffer overflow?**       |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| **Uso de funciones seguras**           | Reemplazar funciones inseguras como `gets`, `strcpy`, `sprintf` por `fgets`, `strncpy`, `snprintf`.             | ✅ Reduce el riesgo              |
| **Validación de entradas**             | Comprobar tamaño de buffers antes de copiar datos.                                                               | ✅ Esencial                      |
| **Compilación con protecciones**       | Usar flags como `-fstack-protector`, `-D_FORTIFY_SOURCE=2`, `-Wl,-z,relro`.                                      | ✅ Añade protección extra        |
| **Uso de bibliotecas modernas**        | Usar bibliotecas como **Safe C Library** (`safecLib.h`) que implementan funciones seguras.                       | ✅                               |
| **Evitar aritmética de punteros insegura** | No manipular punteros sin control de límites.                                                                    | ✅                               |
| **Separación de privilegios**          | Evitar ejecutar código como `root` innecesariamente.                                                             | ✅ Reduce impacto                |
| **Revisión con herramientas estáticas**| Usar `cppcheck`, `clang-tidy`, **Coverity** para detectar vulnerabilidades.                                       | ✅ Prevención temprana           |

