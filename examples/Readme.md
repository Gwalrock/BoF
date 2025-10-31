# Readme
Los ejemplos incluidos en este directorio son vulnerables y sensibles a ataques bufferoverflow, no los expongas en redes o equipos no confiables.

## Explotación auth.c
#### Previo
ASLR​  
Disposición aleatoria de espacio de direcciones​  

Desactivación del ASLR​  
echo "0" | sudo dd of=/proc/sys/kernel/randomize_va_space​  

Compilación​  
gcc -m32 -no-pie -fno-stack-protector -ggdb -mpreferred-stack-boundary=2 -z execstack -o auth.bin auth.c​

#### Explotación variable password

#./auth.bin user AAAAAAAAAAAAAAAAaaa  
Si escribimos por ejemplo 19 caracteres como contraseña, se consigue el acceso sin necesidad de conocer la contraseña  

#./auth.bin user AAAAAAAAAAAAAAAAaaaa  
Si escribimos 20 caracteres como contraseña, se provoca un desbordamiento de la memoria

Si utilizas gdb o similar pon el punto de ruptura en la linea 11 para obtener más información

#### Explotacion variable usuario

De manera análoga a la variable password pero esta vez con 103 y 104 caracteres

Si utilizas gdb o similar pon el punto de ruptura en la linea 25 para obtener más información


