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
